# V4 訪問控制

## 控制目標

授權是一個概念，即只允許那些被允許使用資源的人訪問資源。確保經過驗證的應用程式滿足以下高階要求：

* 訪問資源的人員持有有效憑據才能這樣做。
* 使用者與一組明確定義的角色和許可權相關聯。
* 角色和許可權元資料受到保護，不會被重放或篡改。

## 安全驗證要求

## V4.1 通用訪問控制設計

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **4.1.1** | 驗證應用程式是否在受信任的服務層上執行訪問控制規則，尤其是在有客戶端訪問控制並且可能被繞過的情況下。 | ✓ | ✓ | ✓ | 602 |
| **4.1.2** | 驗證訪問控制所使用的所有使用者和資料屬性以及策略資訊，不能被終端使用者操縱，除非得到特別授權。 | ✓ | ✓ | ✓ | 639 |
| **4.1.3** | 驗證是否存在最小許可權原則 —— 使用者應該只能訪問他們擁有特定授權的功能、資料檔案、URL、控制器、服務和其他資源。這意味著防止欺騙或特權提升。 ([C7](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 285 |
| **4.1.4** | [已刪除，與 4.1.3 重複] | | | | |
| **4.1.5** | 驗證訪問控制安全，在發生異常時是否失效。 ([C10](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 285 |

## V4.2 操作級訪問控制

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **4.2.1** | 驗證敏感資料和 API 的保護，防止針對建立、讀取、更新和刪除記錄的不安全直接物件引用（IDOR）攻擊，如建立或更新別人的記錄，檢視每個人的記錄或刪除所有記錄。 | ✓ | ✓ | ✓ | 639 |
| **4.2.2** | 驗證應用程式或框架是否實施了強大的反 CSRF 機制來保護經過身份驗證的功能，以及有效的反自動化或反 CSRF 保護無需身份驗證的功能。 | ✓ | ✓ | ✓ | 352 |

## V4.3 其他訪問控制注意事項

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **4.3.1** | 驗證管理介面使用適當的多因素認證，防止未經授權的使用。 | ✓ | ✓ | ✓ | 419 |
| **4.3.2** | 驗證目錄瀏覽被禁用，除非特意需要。此外，應用程式不應允許披露檔案或目錄元資料，例如 Thumbs.db、.DS_Store、.git 或.svn 資料夾。 | ✓ | ✓ | ✓ | 548 |
| **4.3.3** | 驗證應用程式對低價值的系統有額外的授權（如升級或自適應認證），對高價值的應用程式進行職責分離，以根據應用程式和過去的欺詐風險執行反欺詐控制。 | | ✓ | ✓ | 732 |

## 參考文獻

有關更多資訊，請參閱：

* [OWASP Testing Guide 4.0: Authorization](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/05-Authorization_Testing/README.html)
* [OWASP Cheat Sheet: Access Control](https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html)
* [OWASP CSRF Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html)
* [OWASP REST Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)