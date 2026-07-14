# Java 連線 IIS FTP（Port 21）無法使用 FTPClient 的完整診斷與處理指南

> 適用情境：只知道主機、Port 21、帳號與密碼；Java 使用 Apache Commons Net，但無法登入、列目錄或下載檔案。  
> 文件版本：2026-07-14  
> 核心原則：**Port 21 不能單獨證明是明文 FTP；IIS 的 Explicit FTPS 通常也使用 Port 21。**

---

## 1. 快速結論

IIS FTP 搭配 Java 失敗，最常見的根因如下：

1. IIS 要求 Explicit FTPS，但程式使用明文 FTPClient。
2. 控制通道已連線、甚至登入成功，但 Passive Mode 的資料通道被 Windows Firewall、外部防火牆或 NAT 阻擋。
3. FTPS 已開始 TLS 握手，但 JVM 不信任伺服器憑證，或連線主機名不在憑證 SAN 中。
4. 未切換 Binary Mode，導致 ZIP、PDF、圖片、Office 檔案等二進位內容損毀。
5. 中文檔名的控制通道編碼與 IIS 不一致。
6. 使用 retrieveFileStream() 後漏掉 completePendingCommand()，使下一個 FTP 指令卡住或失敗。
7. 帳號格式、FTP Authorization Rules、NTFS 權限、FTP User Isolation 或遠端路徑不正確。

先取得完整 FTP 指令與回應紀錄，再依回應碼判斷；不要只處理一個籠統的 IOException。

---

## 2. FTP、Explicit FTPS、Implicit FTPS 與 SFTP

| 類型 | 常見連接埠 | Java 用戶端 | 建立方式 |
|---|---:|---|---|
| 明文 FTP | 21 | FTPClient | 一開始就是未加密 FTP |
| Explicit FTPS | 21 | FTPSClient(false) | 先收到 FTP banner，再以 AUTH TLS 升級為 TLS |
| Implicit FTPS | 990 | FTPSClient(true) | TCP 連上後立刻做 TLS 握手 |
| SFTP | 22 | SSH/SFTP 函式庫 | SSH File Transfer Protocol，與 FTP/FTPS 不相容 |

Microsoft 說明 IIS 的 Explicit FTPS 預設使用 Port 21，而 Implicit FTPS 通常使用 Port 990。因此「Port 21」只能縮小到明文 FTP 或 Explicit FTPS，不能在連線前二選一。參考：[Microsoft IIS FTP over SSL 設定](https://learn.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/sitedefaults/ftpserver/security/ssl)。

**FTPS 不等於 SFTP。** IIS 原生 FTP 站台是 FTP/FTPS；若對方實際回傳 SSH-2.0-...，應改用 SFTP client。

---

## 3. 為什麼會「登入成功但下載失敗」

FTP 同時使用兩類連線：

- **控制通道**：通常是 TCP 21，傳送 USER、PASS、FEAT、PASV、RETR 等指令。
- **資料通道**：列目錄、上傳、下載時另外建立；被動模式下，由 Client 連往 Server 宣告的另一個 TCP port。

因此：

- 220、230 只證明控制通道與登入大致成功。
- LIST、MLSD、RETR 才會真正測到資料通道。
- 425 或下載逾時，通常要查 Passive Port、防火牆、NAT 與 PASV 回傳 IP。

---

## 4. 第一階段：先做控制通道診斷

### 4.1 開啟安全的協定紀錄

Apache Commons Net 的 PrintCommandListener 可記錄指令與回應。suppressLogin=true 會遮蔽登入內容：

~~~java
client.addProtocolCommandListener(
    new PrintCommandListener(new PrintWriter(System.out, true), true)
);
~~~

正式環境仍應限制 log 存取，並確認自訂 logger 不會記錄密碼。參考：[PrintCommandListener API](https://commons.apache.org/proper/commons-net/apidocs/org/apache/commons/net/PrintCommandListener.html)。

可能看到：

~~~text
220 Microsoft FTP Service
USER account
331 Password required
PASS *******
230 User logged in
~~~

或：

~~~text
220 Microsoft FTP Service
USER account
534 Policy requires SSL
~~~

### 4.2 查詢 FEAT

FEAT 用來查伺服器宣告的擴充功能。若包含 AUTH TLS、PBSZ、PROT，表示支援 Explicit FTPS；若包含 UTF8，表示宣告支援 UTF-8 路徑。

~~~java
FTPClient probe = new FTPClient();
probe.setConnectTimeout(10_000);
probe.setDefaultTimeout(10_000);
probe.connect(host, 21);

System.out.printf("connect: %d %s%n",
        probe.getReplyCode(), probe.getReplyString());

int featCode = probe.sendCommand("FEAT");
System.out.println("FEAT code: " + featCode);
for (String line : probe.getReplyStrings()) {
    System.out.println(line);
}
~~~

正確解讀：

- 有 AUTH TLS：伺服器**支援** FTPS，不代表一定強制。
- 沒有 AUTH TLS：不能百分之百證明不支援；有些伺服器未完整實作 FEAT。
- 明文登入回 534 或 SSL required：政策要求 FTPS。
- FTPS 握手出現 PKIX 或 hostname 錯誤：這是憑證驗證問題，不應自動降級成明文 FTP。

### 4.3 安全的協定判斷策略

1. 維運設定若明確指定協定，直接依設定使用。
2. 完全未知時，先做不含密碼的 banner／FEAT 診斷。
3. 優先嘗試 Explicit FTPS。
4. 只有在伺服器明確不支援 AUTH TLS 時，才由人工或受控設定允許明文 FTP。
5. 憑證不受信任、過期、SAN 不符或 TLS 握手失敗時停止，不可自動 fallback。
6. Port 990 才通常考慮 Implicit FTPS；Port 22 且 banner 是 SSH 才考慮 SFTP。

以下寫法有安全風險：

~~~java
try {
    useFtps();
} catch (Exception any) {
    usePlainFtp(); // 憑證驗證失敗也會降級
}
~~~

---

## 5. IIS FTP SSL Settings

在 IIS Manager：

~~~text
Sites
└─ 目標 FTP Site
   └─ FTP SSL Settings
~~~

檢查：

- SSL Certificate：是否選到正確且未過期的憑證。
- Allow SSL connections：允許 Client 選擇 FTP 或 FTPS。
- Require SSL connections：Client 必須使用 FTPS。
- Custom / Advanced：控制通道與資料通道可分別設定 Allow、Require；資料通道還可設定 Deny。

IIS 組態示意：

~~~xml
<ssl
  serverCertHash="..."
  controlChannelPolicy="SslRequire"
  dataChannelPolicy="SslRequire" />
~~~

若控制與資料通道都要求 SSL，Java 應使用 Explicit FTPSClient(false)，登入後執行：

~~~java
client.execPBSZ(0);
client.execPROT("P");
~~~

PROT P 代表保護資料通道。若只升級控制通道而資料通道仍為 Clear，IIS 的 dataChannelPolicy="SslRequire" 會拒絕列目錄或下載。參考：[Microsoft FTP SSL Settings](https://learn.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/siteDefaults/ftpServer/security/ssl) 與 [FTPSClient API](https://commons.apache.org/proper/commons-net/apidocs/org/apache/commons/net/ftp/FTPSClient.html)。

---

## 6. Passive Mode、IIS FTP Firewall Support 與 NAT

### 6.1 Java 端

每次 connect() 後執行：

~~~java
client.enterLocalPassiveMode();
~~~

Passive Mode 是由 Client 主動連到 Server 的資料 port，通常比 Active Mode 更容易穿越 Client 端防火牆。

### 6.2 IIS 端固定資料連接埠

在 IIS Manager 的**伺服器層級**：

~~~text
Server
└─ FTP Firewall Support
   └─ Data Channel Port Range: 50000-50100
~~~

50000-50100 只是範例，應依同時傳輸數與資安政策決定。固定且足夠的窄範圍通常較容易管理。

若 FTP Server 位於 NAT 後方，在 FTP Site 的 FTP Firewall Support 設定：

~~~text
External IP Address of Firewall: 對外可達的公網 IPv4
~~~

若 PASV 回覆內網位址或錯誤公網位址，控制通道仍可能成功，但資料通道會逾時。

設定後重新啟動 Microsoft FTP Service：

~~~powershell
Restart-Service ftpsvc
~~~

參考：[Microsoft：Configuring FTP Firewall Settings in IIS](https://learn.microsoft.com/en-us/iis/publish/using-the-ftp-service/configuring-ftp-firewall-settings-in-iis-7)。

### 6.3 Windows Firewall、外部防火牆與 NAT

至少允許：

- TCP 21：控制通道。
- TCP 50000-50100：Passive Data Channel 範圍。

PowerShell 範例（須由管理員依組織政策執行）：

~~~powershell
New-NetFirewallRule -DisplayName "IIS FTP Control 21" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 21
New-NetFirewallRule -DisplayName "IIS FTP Passive 50000-50100" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 50000-50100
~~~

硬體防火牆、雲端 NSG/Security Group、負載平衡器與 NAT 也必須允許並轉送**完全相同**的範圍。FTPS 內容已加密，依賴 FTP ALG 動態開 port 可能失效；固定 Passive Port 並明確開放通常較可預期。

### 6.4 確認資料通道問題

~~~text
230 User logged in
PASV
227 Entering Passive Mode (203,0,113,10,195,80)
RETR /reports/a.zip
425 Can't open data connection
~~~

195 × 256 + 80 = 50000，表示 Server 要 Client 連 TCP 50000。應從**執行 Java 的那台機器**測試該 IP／port；只在 IIS 主機本機測試不夠。

---

## 7. Binary Mode

Apache Commons Net 的 FTP 預設類型是 ASCII，而且重新連線後會重設。下載 ZIP、PDF、圖片、Excel、Word 或其他非純文字內容時，登入後明確設定：

~~~java
if (!client.setFileType(FTP.BINARY_FILE_TYPE)) {
    throw new IOException("無法切換 Binary Mode: " + client.getReplyString());
}
~~~

參考：[FTPClient API](https://commons.apache.org/proper/commons-net/apidocs/org/apache/commons/net/ftp/FTPClient.html)。

下載後若有來源雜湊值，應比對 SHA-256；至少也要檢查檔案大小，並避免留下被誤用的半成品。

---

## 8. 中文檔名與 UTF-8

FTP 檔案**內容**的傳輸模式，與控制通道上**路徑／檔名**的編碼是兩件事。Binary Mode 不會修正中文檔名。

建議：

1. 在 connect() 前設定控制通道編碼。
2. 可啟用 Commons Net 的 UTF-8 自動偵測，或明確指定 UTF-8。
3. 連線後檢查 FEAT 是否含 UTF8。
4. 必要時送 OPTS UTF8 ON，並檢查回應。
5. 若舊系統實際使用 Big5，Client 必須與伺服器一致，不可盲目強制 UTF-8。

~~~java
FTPClient client = new FTPClient();
client.setAutodetectUTF8(true); // connect 前
client.setControlEncoding(StandardCharsets.UTF_8);
client.connect(host, 21);

int utf8Reply = client.sendCommand("OPTS", "UTF8 ON");
System.out.println(utf8Reply + " " + client.getReplyString());
~~~

建議用中文、空白、中英混合與多層目錄實測。RFC 2640 建議 FTP 路徑使用 UTF-8，並定義伺服器可在 FEAT 回覆 UTF8；檔案內容不因此自動轉碼。參考：[RFC 2640](https://www.rfc-editor.org/info/rfc2640/) 與 [Commons Net 控制編碼 API](https://commons.apache.org/proper/commons-net/apidocs/org/apache/commons/net/ftp/FTP.html)。

---

## 9. retrieveFile 與 retrieveFileStream

### 9.1 優先使用 retrieveFile

retrieveFile(remote, outputStream) 會替你處理資料串流與最終回應，較不容易漏步驟：

~~~java
try (OutputStream out = Files.newOutputStream(tempFile)) {
    if (!client.retrieveFile(remoteFile, out)) {
        throw new IOException(
            "下載失敗: code=" + client.getReplyCode()
            + ", reply=" + client.getReplyString());
    }
}
~~~

### 9.2 retrieveFileStream 必須完成 pending command

~~~java
try (InputStream in = client.retrieveFileStream(remoteFile)) {
    if (in == null) {
        throw new IOException(
            "無法開啟下載串流: code=" + client.getReplyCode()
            + ", reply=" + client.getReplyString());
    }
    Files.copy(in, tempFile, StandardCopyOption.REPLACE_EXISTING);
}

if (!client.completePendingCommand()) {
    throw new IOException(
        "伺服器未確認傳輸完成: code=" + client.getReplyCode()
        + ", reply=" + client.getReplyString());
}
~~~

正確順序：

1. 呼叫 retrieveFileStream()。
2. 完整讀取 InputStream。
3. 關閉 InputStream。
4. 呼叫並檢查 completePendingCommand()。

漏掉第 4 步，控制通道上的最終 226 等回應可能尚未被消化，下一個 FTP 指令就可能卡住或讀到錯誤回應。

---

## 10. Java TrustStore 與憑證

FTPS 常見例外：

~~~text
javax.net.ssl.SSLHandshakeException
PKIX path building failed
unable to find valid certification path
No subject alternative names matching ...
~~~

檢查：

- 憑證是否過期或尚未生效。
- 憑證鏈是否完整，伺服器是否送出 intermediate CA。
- 簽發 CA 是否在 JVM 使用的 TrustStore。
- 連線 DNS 名稱是否出現在憑證 Subject Alternative Name（SAN）。
- 是否誤用 IP 連線，但憑證只簽給 ftp.example.com。

建立應用程式專用 TrustStore：

~~~bash
keytool -importcert \
  -alias company-ftp-ca \
  -file company-ftp-ca.cer \
  -keystore ftp-truststore.p12 \
  -storetype PKCS12
~~~

先核對憑證指紋，再接受匯入。啟動應用程式：

~~~bash
java \
  -Djavax.net.ssl.trustStore=/secure/path/ftp-truststore.p12 \
  -Djavax.net.ssl.trustStorePassword='由安全機制提供' \
  -jar application.jar
~~~

正式環境必須啟用主機名稱驗證：

~~~java
FTPSClient client = new FTPSClient(false);
client.setEndpointCheckingEnabled(true);
~~~

Apache Commons Net 文件提醒 FTPSClient 預設不一定驗證憑證主機名稱。不要使用 trust-all TrustManager，也不要因 PKIX 失敗而自動改回明文 FTP。參考：[FTPSClient API](https://commons.apache.org/proper/commons-net/apidocs/org/apache/commons/net/ftp/FTPSClient.html) 與 [Oracle keytool](https://docs.oracle.com/en/java/javase/19/docs/specs/man/keytool.html)。

受控環境可暫時加入下列參數分析 TLS；輸出可能含憑證與環境資訊，不宜長期開啟：

~~~text
-Djavax.net.debug=ssl,handshake
~~~

---

## 11. 常見 FTP 回應碼

回應碼要和當時指令一起解讀。參考：[RFC 959](https://www.rfc-editor.org/info/rfc959/)。

| 回應碼 | 意義／常見情境 | 優先排查 |
|---:|---|---|
| 220 | Service ready | 控制 port 可達；仍未證明帳密、TLS 與資料通道正常 |
| 230 | User logged in | 登入成功；接著測 PWD、MLSD/LIST、RETR |
| 425 | Can't open data connection | Passive port、防火牆、NAT、PASV IP、資料通道 TLS |
| 426 | Connection closed; transfer aborted | 網路中斷、逾時、傳輸中止、網路設備重設 |
| 530 | Not logged in | 帳密、帳號格式、Basic Authentication、Authorization Rules、User Isolation、NTFS 權限 |
| 534 | 安全政策未滿足，IIS 常見 SSL required | Explicit FTPS、控制／資料通道 SSL 政策、PROT P |
| 550 | File unavailable／動作未完成 | 遠端路徑、大小寫、Read 權限、虛擬目錄、User Isolation、檔案鎖定 |

530 不一定只是密碼錯；550 不一定代表檔案不存在；425 若只在 FTPS 出現，還要查 PROT P、TLS inspection 與 FTP ALG。

---

## 12. 建議設定檔與程式架構

正式流程應保存明確設定：

~~~yaml
ftp:
  host: ftp.example.com
  port: 21
  protocol: FTPS_EXPLICIT
  username: ${FTP_USERNAME}
  password: ${FTP_PASSWORD}
  passive-mode: true
  binary-mode: true
  control-encoding: UTF-8
  enable-utf8-option: true
  connect-timeout: 10s
  control-timeout: 15s
  data-timeout: 60s
  endpoint-checking: true
  remote-base-directory: /outbound
  trust-store: /secure/path/ftp-truststore.p12
~~~

推薦分層：

~~~text
FtpProperties
├─ 驗證協定、port、timeout、路徑
FtpClientFactory
├─ 建立 FTPClient 或 FTPSClient
├─ TLS、TrustStore、hostname policy
└─ logging listener
FtpSession
├─ connect、login
├─ PBSZ/PROT（FTPS）
├─ passive、binary、encoding
└─ logout/disconnect
FtpTransferService
├─ list
├─ download 到暫存檔
├─ 大小／雜湊驗證
└─ atomic move 成正式檔名
~~~

設計原則：

- 不在每次下載時自動猜協定。
- 不把帳密或 TrustStore 密碼寫入 log／Git。
- Client instance 不跨執行緒共用。
- 下載先寫 .part，成功後 atomic move。
- 對 connect、control、data 分別設 timeout。
- retry 只用於暫時性網路錯誤，採有限次數與 backoff；530、534、550 不應無腦重試。
- 記錄 host、port、protocol、階段、reply code、耗時與 correlation id。

---

## 13. Maven 相依套件

截至本文版本日期，Apache 官方頁面列出的 Commons Net 為 3.13.0：

~~~xml
<dependency>
  <groupId>commons-net</groupId>
  <artifactId>commons-net</artifactId>
  <version>3.13.0</version>
</dependency>
~~~

部署時仍應由相依治理工具確認目前核准版本與安全公告。參考：[Apache Commons Net Maven Coordinates](https://commons.apache.org/proper/commons-net/dependency-info.html)。

---

## 14. 完整 Java 範例：FTP／Explicit FTPS 下載器

下例採 Java 17+，協定由設定明確指定，不做不安全的自動降級，並包含 Passive Mode、Binary Mode、UTF-8、主機名稱驗證、安全 logging、暫存檔及完整清理。

~~~java
import java.io.IOException;
import java.io.OutputStream;
import java.io.PrintWriter;
import java.nio.charset.StandardCharsets;
import java.nio.file.AtomicMoveNotSupportedException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;
import java.time.Duration;
import java.util.Objects;

import org.apache.commons.net.PrintCommandListener;
import org.apache.commons.net.ftp.FTP;
import org.apache.commons.net.ftp.FTPClient;
import org.apache.commons.net.ftp.FTPReply;
import org.apache.commons.net.ftp.FTPSClient;

public final class IisFtpDownloader {
    public enum Protocol { FTP, FTPS_EXPLICIT }

    public record Config(
            String host,
            int port,
            Protocol protocol,
            String username,
            String password,
            boolean enableProtocolLog,
            boolean enableUtf8Option,
            Duration connectTimeout,
            Duration controlTimeout,
            Duration dataTimeout) {

        public Config {
            Objects.requireNonNull(host, "host");
            Objects.requireNonNull(protocol, "protocol");
            Objects.requireNonNull(username, "username");
            Objects.requireNonNull(password, "password");
            Objects.requireNonNull(connectTimeout, "connectTimeout");
            Objects.requireNonNull(controlTimeout, "controlTimeout");
            Objects.requireNonNull(dataTimeout, "dataTimeout");
            if (host.isBlank()) throw new IllegalArgumentException("host 不可空白");
            if (port < 1 || port > 65535) {
                throw new IllegalArgumentException("port 超出範圍");
            }
        }
    }

    public void download(Config config, String remoteFile, Path localFile)
            throws IOException {

        Objects.requireNonNull(config, "config");
        Objects.requireNonNull(remoteFile, "remoteFile");
        Objects.requireNonNull(localFile, "localFile");

        FTPClient client = createClient(config);
        Path target = localFile.toAbsolutePath().normalize();
        Path parent = target.getParent();
        if (parent == null) throw new IOException("無法判斷目標目錄");

        Files.createDirectories(parent);
        Path temp = Files.createTempFile(parent, ".ftp-", ".part");
        boolean completed = false;

        try {
            connectAndLogin(client, config);
            configureSession(client, config);

            try (OutputStream output = Files.newOutputStream(temp)) {
                if (!client.retrieveFile(remoteFile, output)) {
                    throw ftpError(client, "RETR 下載失敗: " + remoteFile);
                }
            }

            // 若業務允許空檔，應改與遠端 SIZE 或 metadata 比對。
            if (Files.size(temp) == 0L) {
                throw new IOException("下載結果為 0 bytes: " + remoteFile);
            }

            moveIntoPlace(temp, target);
            completed = true;

            if (!client.logout()) {
                throw ftpError(client, "下載成功，但 LOGOUT 失敗");
            }
        } finally {
            if (!completed) Files.deleteIfExists(temp);
            disconnectQuietly(client);
        }
    }

    private FTPClient createClient(Config config) {
        FTPClient client;

        if (config.protocol() == Protocol.FTPS_EXPLICIT) {
            FTPSClient ftps = new FTPSClient(false); // Explicit FTPS
            ftps.setEndpointCheckingEnabled(true);
            client = ftps;
        } else {
            client = new FTPClient();
        }

        client.setConnectTimeout(
            Math.toIntExact(config.connectTimeout().toMillis()));
        client.setDefaultTimeout(
            Math.toIntExact(config.controlTimeout().toMillis()));
        client.setDataTimeout(config.dataTimeout());

        // connect 前設定。
        client.setControlEncoding(StandardCharsets.UTF_8);
        client.setAutodetectUTF8(true);

        if (config.enableProtocolLog()) {
            client.addProtocolCommandListener(
                new PrintCommandListener(
                    new PrintWriter(System.out, true),
                    true // suppressLogin
                )
            );
        }
        return client;
    }

    private void connectAndLogin(FTPClient client, Config config)
            throws IOException {
        client.connect(config.host(), config.port());
        if (!FTPReply.isPositiveCompletion(client.getReplyCode())) {
            throw ftpError(client, "FTP 伺服器拒絕控制連線");
        }
        if (!client.login(config.username(), config.password())) {
            throw ftpError(client, "FTP 登入失敗");
        }
    }

    private void configureSession(FTPClient client, Config config)
            throws IOException {
        if (client instanceof FTPSClient ftps) {
            ftps.execPBSZ(0);
            ftps.execPROT("P");
        }

        client.enterLocalPassiveMode();

        if (!client.setFileType(FTP.BINARY_FILE_TYPE)) {
            throw ftpError(client, "無法切換為 Binary Mode");
        }

        if (config.enableUtf8Option()) {
            int code = client.sendCommand("OPTS", "UTF8 ON");
            if (!FTPReply.isPositiveCompletion(code)) {
                throw ftpError(client, "伺服器不接受 OPTS UTF8 ON");
            }
        }
    }

    private static IOException ftpError(FTPClient client, String message) {
        return new IOException(
            message + "; replyCode=" + client.getReplyCode()
            + "; reply=" + client.getReplyString()
        );
    }

    private static void moveIntoPlace(Path source, Path target)
            throws IOException {
        try {
            Files.move(source, target,
                StandardCopyOption.ATOMIC_MOVE,
                StandardCopyOption.REPLACE_EXISTING);
        } catch (AtomicMoveNotSupportedException ex) {
            Files.move(source, target, StandardCopyOption.REPLACE_EXISTING);
        }
    }

    private static void disconnectQuietly(FTPClient client) {
        if (!client.isConnected()) return;
        try {
            client.disconnect();
        } catch (IOException ignored) {
            // 不蓋掉原始例外。
        }
    }

    public static void main(String[] args) throws IOException {
        Config config = new Config(
            "ftp.example.com",
            21,
            Protocol.FTPS_EXPLICIT,
            System.getenv("FTP_USERNAME"),
            System.getenv("FTP_PASSWORD"),
            true,
            true,
            Duration.ofSeconds(10),
            Duration.ofSeconds(15),
            Duration.ofSeconds(60)
        );

        new IisFtpDownloader().download(
            config,
            "/outbound/report.zip",
            Path.of("downloads/report.zip")
        );
    }
}
~~~

調整注意事項：

- 已確定明文 FTP 時才改成 Protocol.FTP；明文會暴露帳密與內容。
- 若伺服器不接受 OPTS UTF8 ON，先確認 FEAT 與 IIS 編碼政策，再關閉該選項。
- 業務允許空檔時，不能用 0 bytes 即失敗；應與遠端 SIZE 或 metadata 比對。
- setDataTimeout(Duration) 適用近期 Commons Net；舊版應依該版 API 使用對應 overload。
- 自訂 TrustStore 應由啟動參數或專用 SSLContext 注入，禁止 trust-all。

---

## 15. 文字版排查流程圖

~~~text
[開始：已知 Host、Port 21、帳密]
                |
                v
[TCP 21 可達？]
   | 否 ------------------> DNS / route / firewall / service binding
   | 是
   v
[收到 220 FTP banner？]
   | 否 ------------------> 連錯服務、proxy、implicit TLS、網路設備
   | 是
   v
[FEAT：記錄 AUTH TLS / UTF8 / PBSZ / PROT]
                |
                v
[優先測 Explicit FTPS：FTPSClient(false)]
   | TLS/PKIX/SAN 失敗 ----> 修憑證鏈、TrustStore、DNS/SAN；禁止降級
   | AUTH TLS 不支援 ------> 經核准後才測明文 FTP
   | 成功
   v
[LOGIN 成功（230）？]
   | 530 ------------------> 帳密 / 帳號格式 / Basic Auth /
   |                         Authorization Rules / NTFS / User Isolation
   | 534 ------------------> SSL policy、AUTH TLS、PBSZ/PROT
   | 是
   v
[PBSZ 0 + PROT P（FTPS）]
                |
                v
[Passive Mode + Binary Mode]
                |
                v
[LIST/MLSD 或 RETR 成功？]
   | 425/timeout ----------> Passive Port Range / Windows Firewall /
   |                         NAT / PASV external IP / cloud firewall / ALG
   | 426 ------------------> 傳輸中斷、timeout、網路設備 reset
   | 550 ------------------> 路徑 / 權限 / 虛擬目錄 /
   |                         User Isolation / 檔案不存在
   | 是
   v
[中文檔名正確？]
   | 否 ------------------> connect 前 control encoding、
   |                         FEAT UTF8、OPTS UTF8 ON、IIS 編碼
   | 是
   v
[檔案大小/雜湊正確？]
   | 否 ------------------> Binary Mode、傳輸未完成、半成品
   | 是
   v
[成功；保存明確協定，停止每次自動探測]
~~~

---

## 16. 分角色檢查清單

### Java 開發端

- [ ] Port 21 不直接假設明文 FTP。
- [ ] Explicit FTPS 使用 FTPSClient(false)。
- [ ] FTPS 啟用 endpoint checking。
- [ ] FTPS 登入後執行 PBSZ 0 與 PROT P。
- [ ] 每次 connect 後進入 Passive Mode。
- [ ] 每次 connect 後設定 Binary Mode。
- [ ] 控制編碼在 connect 前設定。
- [ ] OPTS UTF8 ON 的回應有被檢查。
- [ ] connect、control、data timeout 均有上限。
- [ ] 保留各階段 reply code 與 reply string。
- [ ] logging 遮蔽 PASS，不記錄密碼。
- [ ] retrieveFile() 的 boolean 有被檢查。
- [ ] retrieveFileStream() 後有 completePendingCommand()。
- [ ] 先下載到暫存檔，成功後才搬移。
- [ ] Client 不跨執行緒共用。
- [ ] 憑證錯誤不觸發明文降級。

### IIS 管理端

- [ ] FTP Site binding 是預期 IP／hostname／Port 21。
- [ ] FTP SSL Settings 憑證正確且未過期。
- [ ] 已確認 Allow、Require 或 Custom SSL 政策。
- [ ] Basic／Anonymous Authentication 符合需求。
- [ ] FTP Authorization Rules 允許該使用者 Read。
- [ ] NTFS ACL 允許讀取實體檔案。
- [ ] FTP User Isolation 與 home directory 正確。
- [ ] FTP Firewall Support 設定固定 Passive Port Range。
- [ ] NAT 後方設定正確 External IP。
- [ ] 變更後已重啟 ftpsvc。
- [ ] 已檢查 IIS log 與 Windows Event Viewer。

### 網路／資安端

- [ ] Client 到 Server TCP 21 可達。
- [ ] Client 到完整 Passive Port Range 可達。
- [ ] Windows Firewall 開放 21 與 Passive Port Range。
- [ ] 外部防火牆／NSG 開放相同範圍。
- [ ] NAT 對控制與資料 port 正確轉送。
- [ ] PASV 回覆 IP 是 Client 可達位址。
- [ ] FTP ALG／TLS inspection 不會破壞 FTPS。
- [ ] 憑證鏈、SAN、DNS 與組織 CA 正確。

### 驗收測試

- [ ] 收到 220，登入收到 230。
- [ ] 能執行 PWD、列目錄與下載。
- [ ] 小檔、大檔、中文與含空白檔名都成功。
- [ ] ZIP／PDF／圖片可正常開啟。
- [ ] 檔案大小或 SHA-256 與來源一致。
- [ ] 第二次與連續多次下載均成功。
- [ ] 錯誤帳密時得到不洩密的錯誤。
- [ ] 不受信任憑證會停止，且不降級成 FTP。

---

## 17. 提交給維運的最小診斷資料

~~~text
時間（含時區）：
Java Client 來源 IP：
FTP Host / Port：
設定協定：FTP 或 FTPS_EXPLICIT：
connect reply code/string：
FEAT 完整回應：
login reply code/string：
失敗指令：LIST / MLSD / RETR / completePendingCommand：
失敗 reply code/string：
Exception 類型、message、完整 stack trace：
PASV 回覆的 IP 與 port：
遠端路徑：
是否只有中文檔名失敗：
是否只有 FTPS 失敗：
是否已由 Client 主機測試 Passive port：
~~~

請刪除密碼、token、私鑰與不應外流的憑證資料。

---

## 18. 最短可行處理順序

1. 啟用遮蔽密碼的 protocol logging。
2. 確認收到 220，執行 FEAT。
3. Port 21 優先測 FTPSClient(false)。
4. TLS 驗證失敗時修 TrustStore、憑證鏈與 SAN；不降級。
5. FTPS 登入後執行 PBSZ 0、PROT P。
6. 執行 Passive Mode 與 Binary Mode。
7. IIS 設固定 Passive Port Range，所有防火牆與 NAT 同步開放。
8. 測 LIST/MLSD 與 RETR，依 425/426/550 分流。
9. 中文檔名另測 FEAT UTF8、控制通道 UTF-8 與 OPTS UTF8 ON。
10. 正式設定檔保存明確協定，停用每次探測與不安全 fallback。

將「控制通道」、「TLS／憑證」、「資料通道」、「檔案／權限」、「編碼」分開驗證，就能明確定位問題。

---

## 19. 官方參考資料

- [Apache Commons Net FTPClient API](https://commons.apache.org/proper/commons-net/apidocs/org/apache/commons/net/ftp/FTPClient.html)
- [Apache Commons Net FTPSClient API](https://commons.apache.org/proper/commons-net/apidocs/org/apache/commons/net/ftp/FTPSClient.html)
- [Apache Commons Net Maven Coordinates](https://commons.apache.org/proper/commons-net/dependency-info.html)
- [Microsoft：Default FTP over SSL Settings](https://learn.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/sitedefaults/ftpserver/security/ssl)
- [Microsoft：Configuring FTP Firewall Settings in IIS](https://learn.microsoft.com/en-us/iis/publish/using-the-ftp-service/configuring-ftp-firewall-settings-in-iis-7)
- [Microsoft：Using FTP Over SSL in IIS](https://learn.microsoft.com/en-us/iis/publish/using-the-ftp-service/using-ftp-over-ssl-in-iis-7)
- [Oracle：keytool Command](https://docs.oracle.com/en/java/javase/19/docs/specs/man/keytool.html)
- [RFC 959：File Transfer Protocol](https://www.rfc-editor.org/info/rfc959/)
- [RFC 2640：Internationalization of FTP](https://www.rfc-editor.org/info/rfc2640/)
