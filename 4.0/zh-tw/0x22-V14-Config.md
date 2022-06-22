# V14 配置

## 控制目標

確保經過驗證的應用程式具有：

* 一個安全的、可重複的、可自動化的構建環境。
* 加固第三方庫、依賴和配置管理，使應用不包括過時的或不安全的元件。

應用程式開箱即用配置應該是安全的，可以放在網際網路上，這意味著安全的開箱配置。

## V14.1 構建和部署

構建管道是可重複安全性的基礎——每次發現不安全的東西時，都可以在原始碼、構建或部署指令碼中解決它，並自動進行測試。我們強烈鼓勵使用自動化的構建管道來執行安全和依賴檢查，這些檢查會警告或破壞構建，以防止已知的安全問題被部署到生產環境中。不定期執行的手動步驟會直接導致可避免的安全錯誤。

隨著行業向DevSecOps模式的轉變，必須確保部署和配置的持續可用性和完整性，以實現“已知良好”的狀態。在過去，如果一個系統被入侵，需要幾天到幾個月的時間來證明沒有進一步的入侵發生。今天，隨著軟體定義的基礎設施、零停機時間的快速A/B部署和自動化容器構建的出現，自動和持續地構建、加固和部署一個“已知良好”的替代品來替代任何被入侵的系統，是有可能的。

如果傳統模式仍然存在，那麼就必須採取手動步驟來加固和備份該配置，以便及時用高完整性的、未受損害的系統快速替換受損害的系統。

遵守本節的規定要求一個自動化的構建系統，以及對構建和部署指令碼的訪問。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **14.1.1** | 驗證應用程式的構建和部署過程是以安全和可重複的方式進行的，如 CI / CD 自動化、自動配置管理和自動部署指令碼。 | | ✓ | ✓ | |
| **14.1.2** | 驗證編譯器標誌的配置是否配置為啟用所有可用的緩衝區溢位保護和警告，包括堆疊隨機化、資料執行保護，並在發現不安全的指標、記憶體、格式字串、整數或字串操作時中斷構建。 | | ✓ | ✓ | 120 |
| **14.1.3** | 驗證伺服器配置是否按照應用程式伺服器和所使用框架的建議進行了加固。 | | ✓ | ✓ | 16 |
| **14.1.4** | 驗證應用程式、配置和所有依賴項是否可以使用自動部署指令碼重新部署、在合理的時間內根據記錄和測試的執行手冊構建，或者及時從備份中恢復。 | | ✓ | ✓ | |
| **14.1.5** | 驗證授權管理員可以驗證所有安全相關配置的完整性，以發現篡改行為。 | | | ✓ | |

## V14.2 依賴

依賴管理，對於任何型別應用程式的安全執行都至關重要。未能及時更新過時的或不安全的依賴，是迄今為止最大和最昂貴攻擊的根本原因。

注意：在 L1 ，14.2.1 的合規性與客戶端和其他庫、元件的觀察或檢測有關，而不是更準確的構建時靜態程式碼分析或依賴分析。這些更準確的技術可根據需要在訪談中發現。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **14.2.1** | 驗證所有元件都是最新的，最好是在構建或編譯時使用依賴檢查工具。 ([C2](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 1026 |
| **14.2.2** | 驗證所有不需要的功能、文件、示例應用程式和配置均已被刪除。 | ✓ | ✓ | ✓ | 1002 |
| **14.2.3** | 應用資產，例如JavaScript庫、CSS或網頁字型，如果被託管在外部的內容分發網路（CDN）或供應商，則驗證使用子資源完整性（SRI）來驗證該資產的完整性。 | ✓ | ✓ | ✓ | 829 |
| **14.2.4** | 驗證第三方元件來自預先定義的、可信的和持續維護的資源庫。 ([C2](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | 829 |
| **14.2.5** | 驗證是否維護了正在使用中的所有第三方庫的軟體材料清單（SBOM）。 ([C2](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | |
| **14.2.6** | 驗證通過沙盒或封裝第三方庫來減少攻擊面，只將必需的行為暴露在應用程式中。 ([C2](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | 265 |

## V14.3 意外安全洩露

應加強生產配置以防止常見攻擊，例如除錯控制檯，提高跨站點指令碼（XSS）和遠端檔案包含（RFI）攻擊的門檻，並消除瑣碎的資訊發現“漏洞”，這是許多滲透測試報告中不受歡迎的標誌。 其中許多問題很少被評為重大風險，但它們可跟其他漏洞聯絡在一起。如果這些問題在預設情況下不存在，那就提高了大多數攻擊的門檻。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **14.3.1** | [已刪除，與 7.4.1 重複] | | | | |
| **14.3.2** | 驗證Web或應用伺服器和應用框架的除錯模式在生產中是否被禁用，以消除除錯功能、開發人員控制檯和非預期的安全披露。 | ✓ | ✓ | ✓ | 497 |
| **14.3.3** | 驗證HTTP標頭或HTTP響應的任何部分不暴露系統元件的詳細版本資訊。 | ✓ | ✓ | ✓ | 200 |

## V14.4 HTTP 安全標頭

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **14.4.1** | 驗證每個HTTP響應都包含一個 Content-Type 頭。如果內容型別是 text/* 、 /+xml 和 application/xml ，還要指定一個安全的字符集（如UTF-8，ISO-8859-1）。內容必須與提供的Content-Type頭相匹配。 | ✓ | ✓ | ✓ | 173 |
| **14.4.2** | 驗證所有 API 響應是否包含 Content-Disposition: attachment; filename="api.json" 標頭（或內容型別的其他適當檔名）。 | ✓ | ✓ | ✓ | 116 |
| **14.4.3** | 驗證內容安全策略（CSP）響應標頭是否到位，有助於減輕對 HTML、DOM、JSON 和 JavaScript 注入漏洞等 XSS 攻擊的影響。 | ✓ | ✓ | ✓ | 1021 |
| **14.4.4** | 驗證所有響應是否包含 X-Content-Type-Options: nosniff 標頭。 | ✓ | ✓ | ✓ | 116 |
| **14.4.5** | 驗證所有響應和所有子域中是否包含 Strict-Transport-Security 標頭，例如 Strict-Transport-Security: max-age=15724800; includeSubdomains。 | ✓ | ✓ | ✓ | 523 |
| **14.4.6** | 驗證是否包含合適的 Referrer-Policy 標頭，以避免通過 Referer 標頭將 URL 中的敏感資訊暴露給不受信任的各方。 | ✓ | ✓ | ✓ | 116 |
| **14.4.7** | 驗證網路應用程式的內容在預設情況下不能被嵌入第三方網站，只有在必要時，才允許使用合適的Content-Security-Policy: frame-ancestors和X-Frame-Options響應頭嵌入確切的資源。 | ✓ | ✓ | ✓ | 1021 |

## V14.5 HTTP 請求頭驗證

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **14.5.1** | 驗證應用伺服器只接受應用/API使用的HTTP方法，包括預檢請求的OPTIONS，並對使應用上下文無效的請求進行記錄/警告。 | ✓ | ✓ | ✓ | 749 |
| **14.5.2** | 驗證提供的 Origin 標頭是否不用於身份驗證或訪問控制決策，因為 Origin 標頭很容易被攻擊者更改。 | ✓ | ✓ | ✓ | 346 |
| **14.5.3** | 驗證跨域資源共享（CORS）的 Access-Control-Allow-Origin 標頭是否使用受信任域和子域的嚴格白名單匹配。並且不支援'null'源。 | ✓ | ✓ | ✓ | 346 |
| **14.5.4** | 驗證由受信任的代理或 SSO 裝置新增的 HTTP 標頭（例如bearer令牌）是否已通過應用程式的身份驗證。 | | ✓ | ✓ | 306 |

## 參考文獻

有關更多資訊，請參閱：

* [OWASP Web Security Testing Guide 4.1: Testing for HTTP Verb Tampering](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/03-Testing_for_HTTP_Verb_Tampering.html)
* 將 Content-Disposition 新增到 API 響應，有助於防止許多基於客戶端和伺服器之間的MIME型別誤解的攻擊，並且“filename”選項特別有助於防止 [Reflected File Download attacks.](https://www.blackhat.com/docs/eu-14/materials/eu-14-Hafif-Reflected-File-Download-A-New-Web-Attack-Vector.pdf)
* [Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
* [Exploiting CORS misconfiguration for BitCoins and Bounties](https://portswigger.net/blog/exploiting-cors-misconfigurations-for-bitcoins-and-bounties)
* [OWASP Web Security Testing Guide 4.1: Configuration and Deployment Management Testing](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/README.html)
* [Sandboxing third party components](https://cheatsheetseries.owasp.org/cheatsheets/Third_Party_Javascript_Management_Cheat_Sheet.html#sandboxing-content)
