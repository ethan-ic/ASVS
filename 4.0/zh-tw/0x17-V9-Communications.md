# V9 通訊

## 控制目標

確保經過驗證的應用程式滿足以下進階要求：

* 要求 TLS 或強加密，與內容的敏感性無關。
* 遵循最新指南，包括：
  * 配置建議
  * 首選演算法和密碼
* 避免使用弱的或即將被廢棄的演算法和密碼，除非是最後的手段。
* 禁用已廢棄或已知不安全的演算法和密碼。

在這些要求範圍內：

* 瞭解業界對安全 TLS 配置的建議，因為它經常變化（往往是由於現有演算法和密碼的災難性破壞）。
* 使用最新版本的 TLS 配置審查工具，來配置首選順序和演算法選擇。
* 定期檢查你的配置，以確保安全通訊始終存在並有效。

## V9.1 客戶端通訊安全

確保所有客戶端訊息都通過加密網路傳送，使用 TLS 1.2 或更高版本。
使用最新的工具定期檢查客戶端配置。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **9.1.1** | 驗證所有客戶端連線都使用了 TLS，並且不會降級到不安全或未加密的通訊。 ([C8](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 319 |
| **9.1.2** | 使用最新的 TLS 測試工具，驗證是否只啟用了強密碼套件，並將最強的密碼套件設定為首選。 | ✓ | ✓ | ✓ | 326 |
| **9.1.3** | 驗證只啟用最新推薦版本的 TLS 協議，如 TLS 1.2 和 TLS 1.3。最新版本的 TLS 協議應該是首選項。 | ✓ | ✓ | ✓ | 326 |

## V9.2 伺服器通訊安全

伺服器通訊不僅僅是 HTTP。與其他系統的安全連線，例如監控系統、管理工具、遠端訪問和 ssh、中介軟體、資料庫、大型機、合作伙伴或外部源系統 —— 必須到位。所有這些都必須加密，以防止 “外面安全，裡面被輕易截獲”。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **9.2.1** | 驗證與伺服器的連線是否使用受信任的 TLS 證書。在使用內部生成或自簽名證書的情況下，必須將伺服器配置為只信任特定的內部 CA 和特定的自簽證書。所有其他的都應該被拒絕。 | | ✓ | ✓ | 295 |
| **9.2.2** | 確認所有入站和出站連線都使用了 TLS 等加密通訊，包括管理埠、監控、身份驗證、API 或 Web 服務呼叫、資料庫、雲、serverless、大型機、外部和合作夥伴的連線。伺服器不得回退到不安全或未加密的協議。 | | ✓ | ✓ | 319 |
| **9.2.3** | 驗證所有外部系統中與敏感資訊 / 功能相關的加密連線，均已通過身份驗證。 | | ✓ | ✓ | 287 |
| **9.2.4** | 驗證是否啟用並配置了正確的證書吊銷，例如線上證書狀態協議（OCSP）Stapling。 | | ✓ | ✓ | 299 |
| **9.2.5** | 驗證是否記錄了後端 TLS 連線失敗（的事件）。 | | | ✓ | 544 |

## 參考文獻

有關更多資訊，請參閱：

* [OWASP – TLS Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html)
* [OWASP - Pinning Guide](https://owasp.org/www-community/controls/Certificate_and_Public_Key_Pinning)
* 關於 “TLS 的批准模式 “ 的說明:
    * 在過去，ASVS 提到了美國標準 FIPS 140-2，但作為一個全球標準美國標準的應用可能充滿困難、矛盾或混亂。
    * 實現第 9.1 節的更好方法是審查指南，如 [Mozilla's Server Side TLS](https://wiki.mozilla.org/Security/Server_Side_TLS) or [generate known good configurations](https://mozilla.github.io/server-side-tls/ssl-config-generator/)，並使用已知最新的 TLS 評估工具來獲得所需的安全等級。