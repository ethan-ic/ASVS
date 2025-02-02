# V6 儲存密碼學

## 控制目標

確保經過驗證的應用程式滿足以下進階要求：

* 所有加密模組以安全的方式失效，並且正確處理錯誤。
* 使用合適的隨機數發生器。
* 金鑰訪問被安全地管理。

## V6.1 資料分類

最重要的資產是由應用程式處理、儲存或傳輸的資料。始終執行隱私影響評估，對任何儲存資料的資料保護需求進行正確分類。

| # | 說明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.1.1** | 驗證受監管的私人資料在靜止狀態下是否被加密儲存，如個人身份資訊（PII）、敏感個人資訊或經評估可能受制於歐盟 GDPR 的資料。 | | ✓ | ✓ | 311 |
| **6.1.2** | 驗證受監管的健康資料在靜止狀態下是否被加密儲存，如醫療記錄、醫療裝置詳情或去匿名化的研究記錄。 | | ✓ | ✓ | 311 |
| **6.1.3** | 驗證受監管的金融資料在靜止狀態下是否被加密儲存，如金融賬戶、違約或信用記錄、稅務記錄、工資記錄、受益人或去匿名化的市場或研究記錄。 | | ✓ | ✓ | 311 |

## V6.2 演算法

密碼學的最新進展意味著以前安全的演算法和金鑰長度不再安全或足以保護資料。因此，應該可以改變演算法。

雖然這一部分不容易進行滲透測試，但開發人員應該把這一整節視為強制性的，即使在大多數專案中 L1 都沒有要求。

| # | 說明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.2.1** | 驗證所有的加密模組即使在故障時也是安全的，並且處理錯誤的方式不會使 Padding Oracle 攻擊得逞。 | ✓ | ✓ | ✓ | 310 |
| **6.2.2** | 驗證使用業界認可或政府批准的加密演算法、模式和庫，而不是自定義編碼的加密技術。 ([C8](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | 327 |
| **6.2.3** | 驗證加密初始化向量、密碼配置和分組模式是否使用最新建議進行安全配置。 | | ✓ | ✓ | 326 |
| **6.2.4** | 驗證隨機數、加密或雜湊演算法、金鑰長度、輪次、密碼或模式，可以在任何時候重新配置、升級或交換，以防止密碼中斷。 ([C8](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | 326 |
| **6.2.5** | 驗證不使用已知不安全的分組模式（如 ECB 等）、填充模式（如 PKCS#1 v1.5 等）、小塊大小的密碼（如 Triple-DES、Blowfish 等）和弱雜湊演算法（如 MD5、SHA1 等），除非需要向後相容。 | | ✓ | ✓ | 326 |
| **6.2.6** | 驗證隨機數、初始化向量和其他一次性使用的數字，不得與特定的加密金鑰使用超過一次。生成方法必須適合所使用的演算法。 | | ✓ | ✓ | 326 |
| **6.2.7** | 驗證加密資料是否通過簽名、認證的密碼模式或 HMAC 進行身份驗證，以確保密文不會被未經授權的一方更改。 | | | ✓ | 326 |
| **6.2.8** | 驗證所有的密碼操作都是恆定時間的，在比較、計算或返回中沒有 “短路” 操作，以避免資訊洩漏。 | | | ✓ | 385 |

## V6.3 隨機值

真正的偽隨機數生成（PRNG）很難實現。通常，如果過度使用，系統內良好的熵源不但很快耗盡，而且隨機性較小的源會導致可預測的金鑰和祕密。

| # | 說明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.3.1** | 驗證所有的隨機數、隨機檔名、隨機 GUID 和隨機字串，都是使用加密模組認可的加密安全隨機數生成器生成的，而這些隨機值旨在不被攻擊者猜測。 | | ✓ | ✓ | 338 |
| **6.3.2** | 驗證是否使用 GUID v4 演算法和加密安全偽隨機數生成器（CSPRNG）建立了隨機 GUID。使用其他偽隨機數生成器建立的 GUID 可能是可預測的。 | | ✓ | ✓ | 338 |
| **6.3.3** | 驗證應用程式即使在處於高負載下時也使用適當的熵建立隨機數，或者應用程式在這種情況下優雅地降級。 | | | ✓ | 338 |

## V6.4 金鑰管理

雖然這一部分不容易進行滲透測試，但開發人員應將整個部分視為強制性的，即使大多數專案中都缺少 L1 的要求。

| # | 說明 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.4.1** | 驗證祕密管理解決方案，如鑰匙庫，用於安全地建立、儲存、控制對祕密的訪問和銷燬。 ([C8](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | 798 |
| **6.4.2** | 驗證金鑰材料是否未暴露給應用程式，而是使用一個隔離的安全模組（如保險庫）進行加密操作。 ([C8](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | 320 |

## 參考文獻

有關更多資訊，請參閱：

* [OWASP Testing Guide 4.0: Testing for weak Cryptography](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/README.html)
* [OWASP Cheat Sheet: Cryptographic Storage](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
* [FIPS 140-2](https://csrc.nist.gov/publications/detail/fips/140/2/final)