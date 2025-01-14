# V10 惡意程式碼

## 控制目標

確保程式碼滿足以下進階要求：

* 惡意活動得到安全和適當的處理，不會影響應用程式的其餘部分。
* 沒有定時炸彈或其他基於時間的攻擊。
* 不會向惡意或未經授權的目的地 “打電話回家”。
* 沒有後門、復活節彩蛋、Salami 攻擊、rootkit 或攻擊者可以控制的未授權程式碼。

發現惡意程式碼是否定的證明，這是不可能被充分驗證的。應盡最大努力，確保程式碼沒有固有的惡意程式碼或不需要的功能。

## V10.1 程式碼完整性

對惡意程式碼的最佳防禦是 “信任，但要驗證”。在許多司法管轄區，將未經授權或惡意的程式碼片段引入程式碼，通常是刑事犯罪。策略和過程應明確對惡意程式碼的制裁。

首席開發人員應該定期檢查程式碼簽入，特別是那些可能去訪問時間、I/O 或網路功能的程式碼簽入。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **10.1.1** | 驗證是否使用了程式碼分析工具，可以檢測潛在的惡意程式碼，如時間函式、不安全的檔案操作和網路連線。 | | | ✓ | 749 |

## V10.2 惡意程式碼搜尋

惡意程式碼極為罕見，難以檢測。手動逐行審查程式碼可以幫助尋找邏輯炸彈，但即使是最有經驗的程式碼審查員也很難找到惡意程式碼，哪怕事先知道它們的存在。

如果不能完全接觸到原始碼，包括第三方庫，就不可能遵守本節的規定。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **10.2.1** | 驗證應用程式的原始碼和第三方庫不包含未經授權的回連或資料收集功能。如果存在這樣的功能，在收集任何資料之前，要獲得使用者的操作許可。 | | ✓ | ✓ | 359 |
| **10.2.2** | 驗證應用程式不會對隱私相關的功能或感測器（例如聯絡人、攝像頭、麥克風或位置）要求不必要或過度的許可權。 | | ✓ | ✓ | 272 |
| **10.2.3** | 驗證應用程式的原始碼和第三方庫不包含後門，如硬編碼或額外的未記錄的賬戶或金鑰、程式碼混淆、未記錄的二進位制 blobs、rootkits 或反除錯、不安全的除錯特性，或其他過時、不安全或隱藏的功能，一旦被發現可能會被惡意使用。 | | | ✓ | 507 |
| **10.2.4** | 通過搜尋日期和時間相關函式，來驗證應用程式原始碼和第三方庫不包含時間炸彈。 | | | ✓ | 511 |
| **10.2.5** | 驗證應用程式原始碼和第三方庫不包含惡意程式碼，例如 salami 攻擊、邏輯繞過或邏輯炸彈。 | | | ✓ | 511 |
| **10.2.6** | 驗證應用程式的原始碼和第三方庫不包含復活節彩蛋或任何其他潛在的冗餘功能。 | | | ✓ | 507 |

## V10.3 應用程式完整性

應用程式被部署後，惡意程式碼仍然可以被插入。應用程式需要保護自己免受常見的攻擊，例如執行來自不受信任來源的未簽名程式碼或子域接管。

本節內容的實現，很可能是操作性和持續性的。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **10.3.1** | 驗證如果應用程式具有客戶端或伺服器自動更新功能，則應通過安全通道獲得更新，並進行數字簽名。更新程式碼必須在安裝或執行更新之前驗證更新的數字簽名。 | ✓ | ✓ | ✓ | 16 |
| **10.3.2** | 驗證應用程式是否採用了完整性保護，如程式碼簽名或子資源完整性。應用程式不得從不受信任的來源載入或執行程式碼，例如從不可信任的來源或網際網路載入模組、外掛、程式碼或庫。 | ✓ | ✓ | ✓ | 353 |
| **10.3.3** | 如果應用程式依賴 DNS 條目或 DNS 子域，例如過期的域名、過時的 DNS 指標或 CNAME、公共原始碼庫中過期的專案或臨時的雲 API 介面、serverless 功能或儲存桶（*autogen-bucket-id*.cloud.example.com）或類似情況，則驗證該應用程式是否具有防止子域接管的措施。保護措施可以包括確保定期檢查應用程式使用的 DNS 名稱是否過期或改變。 | ✓ | ✓ | ✓ | 350 |

## 參考文獻

有關更多資訊，請參閱：

* [Hostile Subdomain Takeover, Detectify Labs](https://labs.detectify.com/2014/10/21/hostile-subdomain-takeover-using-herokugithubdesk-more/)
* [Hijacking of abandoned subdomains part 2, Detectify Labs](https://labs.detectify.com/2014/12/08/hijacking-of-abandoned-subdomains-part-2/)