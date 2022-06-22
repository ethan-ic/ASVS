# V5 驗證、過濾和編碼

## 控制目標

最常見的 Web 應用程式安全漏洞，是在沒有任何輸出編碼的情況下，直接使用來自客戶端或環境的輸入（缺乏正確的驗證）。 這一弱點導致了 Web 應用程式中幾乎所有的重大漏洞，例如跨站點指令碼（XSS）、SQL 注入、直譯器注入、語言環境 / Unicode 攻擊、檔案系統攻擊和緩衝區溢位。

確保經過驗證的應用程式滿足以下進階要求：

* 輸入驗證和輸出編碼架構有一條約定的管道來防止注入攻擊。
* 輸入資料是強型別的，經過驗證，範圍或長度檢查，或者在最壞的情況下，經過消毒或過濾。
* 輸出結果根據資料的上下文進行編碼或轉義，儘可能地接近直譯器。

隨著現代網路應用架構的發展，輸出編碼比以往任何時候都更重要。在某些情況下很難提供健壯的輸入驗證，因此使用更安全的 API，如引數化查詢、自動轉義的模板框架或精心選擇的輸出編碼，對應用程式的安全性至關重要。

## V5.1 輸入驗證

正確實施的輸入驗證控制，使用白名單列表和強資料型別，可以消除 90% 以上的所有注入攻擊。長度和範圍檢查可以進一步減少這種情況。在應用程式架構、設計衝刺（Design Sprint）、編碼以及單元和整合測試期間，需要構建安全輸入驗證。 儘管其中許多專案在滲透測試中找不到，但不實施它們的結果通常可以在 V5.3 - 輸出編碼和注入預防要求 中找到。建議開發人員和安全程式碼審查人員將本小節作為基礎（正如所有專案都需要滿足 L1 那樣），以防止注入。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.1.1** | 驗證應用程式是否有 HTTP 引數汙染攻擊的防禦措施，特別是當應用程式框架沒有區分請求引數的來源（GET、POST、cookies、請求頭或環境變數）。 | ✓ | ✓ | ✓ | 235 |
| **5.1.2** | 驗證框架是否能防止批量引數分配攻擊，或者應用程式是否有對策來防止不安全的引數分配，如將欄位標記為私有等型別。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 915 |
| **5.1.3** | 驗證所有輸入（HTML 表單欄位、REST 請求、URL 引數、HTTP 請求頭、cookies、批處理檔案、RSS 源等）都使用 “白名單”（允許列表）。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 20 |
| **5.1.4** | 驗證結構化資料是強型別的，並根據定義的模式進行驗證，包括允許的字元、長度和模式（如信用卡號碼、電子郵件地址、電話號碼，或驗證兩個相關欄位是否合理，如檢查郊區和郵政編碼是否匹配）。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 20 |
| **5.1.5** | 驗證 URL 重定向和轉發的目標地址都在白名單中，或者在重定向到可能不受信任的內容時顯示警告。 | ✓ | ✓ | ✓ | 601 |

## V5.2 過濾和沙盒化

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.2.1** | 驗證所有來自 “所見即所得” 編輯器或類似的不受信任的 HTML 輸入，都已經通過 HTML 過濾庫或框架功能，進行了適當的淨化。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 116 |
| **5.2.2** | 驗證非結構化資料是否經過消毒處理，以執行安全措施，如允許的字符集和長度。 | ✓ | ✓ | ✓ | 138 |
| **5.2.3** | 驗證應用程式在傳遞給郵件系統之前，對使用者的輸入進行過濾，以防止 SMTP 或 IMAP 注入。 | ✓ | ✓ | ✓ | 147 |
| **5.2.4** | 驗證應用程式是否避免使用 eval () 或其他動態程式碼執行功能。在沒有其他選擇的情況下，任何被包含的使用者輸入必須在執行前進行過濾或沙箱處理。 | ✓ | ✓ | ✓ | 95 |
| **5.2.5** | 驗證應用程式是否對相關的使用者輸入進行過濾或沙箱處理，來防止模板注入攻擊。 | ✓ | ✓ | ✓ | 94 |
| **5.2.6** | 驗證應用程式是否通過驗證或淨化不受信任的資料或 HTTP 檔案元資料（如檔名和 URL 輸入欄位），並使用協議、域、路徑和埠的白名單，來防止 SSRF 攻擊。 | ✓ | ✓ | ✓ | 918 |
| **5.2.7** | 驗證應用程式是否過濾、禁用或沙盒處理了使用者提供的可擴充套件向量圖（SVG）指令碼內容，特別是與內聯指令碼產生的 XSS 有關的內容，以及外部物件。 | ✓ | ✓ | ✓ | 159 |
| **5.2.8** | 驗證應用程式是否對使用者提供的模板語言內容（指令碼或表示式，如 Markdown、CSS 或 XSL 樣式表、BBCode 或類似內容）進行過濾、禁用或沙盒處理。 | ✓ | ✓ | ✓ | 94 |

## V5.3 輸出編碼和預防注入

靠近或鄰近當前直譯器的輸出編碼，對應用程式的安全至關重要。通常情況下，輸出編碼並不持久化，而是用於在適當的輸出環境中使輸出安全，以便立即使用。不進行輸出編碼，將最終形成一個不安全、可注入的應用程式。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.3.1** | 驗證輸出編碼是否與所需的直譯器和環境相關。例如，根據 HTML 值、HTML 屬性、JavaScript、URL 引數、HTTP 頭、SMTP 等上下文的要求，使用專門的編碼器，特別是來自不可信任的輸入（如帶有 Unicode 或單引號的名字，如ねこ或 O'Hara）。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 116 |
| **5.3.2** | 驗證輸出編碼是否保留了使用者選擇的字符集和地域，從而使任何 Unicode 字元點都能得到有效和安全的處理。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 176 |
| **5.3.3** | 驗證上下文感知，最好是自動 —— 或者最差也是手動 —— 轉義輸出，以防止反射、儲存或基於 DOM 的 XSS。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 79 |
| **5.3.4** | 驗證資料選擇或資料庫查詢（如 SQL、HQL、ORM、NoSQL）是否使用引數化查詢、ORM、實體框架，或以其他方式防止資料庫注入攻擊。 ([C3](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 89 |
| **5.3.5** | 驗證在沒有引數化或更安全機制的情況下，使用特定上下文的輸出編碼來防止注入攻擊，例如使用 SQL 轉義來防止 SQL 注入。 ([C3, C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 89 |
| **5.3.6** | 驗證應用程式是否可以防止 JSON 注入攻擊、JSON eval 攻擊和 JavaScript 表示式評估。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 830 |
| **5.3.7** | 驗證應用程式可以防止 LDAP 注入漏洞，或者已經實施了特定的安全控制來防止 LDAP 注入。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 90 |
| **5.3.8** | 驗證應用程式是否能防止作業系統命令注入，以及作業系統呼叫是否使用引數化的作業系統查詢或使用上下文命令列輸出編碼。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 78 |
| **5.3.9** | 驗證應用程式是否能防止本地檔案包含（LFI）或遠端檔案包含（RFI）攻擊。 | ✓ | ✓ | ✓ | 829 |
| **5.3.10** | 驗證應用程式是否能防止 XPath 注入或 XML 注入攻擊。 ([C4](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 643 |

注意：使用引數化查詢或轉義 SQL 並不總是足夠的；表和列名、ORDER BY 等不能被轉義。若在這些欄位中轉義使用者提供的資料，會導致查詢失敗或 SQL 注入。

注意：SVG 格式在幾乎所有情況下都明確允許 ECMA 指令碼，所以可能無法完全阻止所有的 SVG XSS 向量。如果需要上傳 SVG，我們強烈建議將這些上傳的檔案作為 text/plain 提供，或者使用一個單獨的 “使用者提供內容” 域，以防止成功的 XSS 接管應用程式。

## V5.4 記憶體、字串和非託管程式碼

以下要求僅在應用程式使用系統語言或非託管程式碼時適用。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.4.1** | 驗證應用程式是否使用記憶體安全字串、更安全的記憶體複製和指標運算，以檢測或防止堆疊、緩衝區或堆溢位。 | | ✓ | ✓ | 120 |
| **5.4.2** | 驗證格式化字串不接受潛在的有害輸入，並且是常量。 | | ✓ | ✓ | 134 |
| **5.4.3** | 驗證運用了符號、範圍和輸入驗證技術來防止整數溢位。 | | ✓ | ✓ | 190 |

## V5.5 預防反序列化

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **5.5.1** | 驗證序列化物件是否使用完整性檢查或加密，以防止惡意物件的建立或資料篡改。 ([C5](https://owasp.org/www-project-proactive-controls/#div-numbering)) | ✓ | ✓ | ✓ | 502 |
| **5.5.2** | 驗證應用程式正確限制 XML 解析器，使其只使用最嚴格的配置，並確保禁用不安全的功能，如解析外部實體，以防止 XML 外部實體注入（XXE）攻擊。 | ✓ | ✓ | ✓ | 611 |
| **5.5.3** | 驗證自定義程式碼和第三方庫（如 JSON、XML 和 YAML 解析器）禁止或限制不受信任資料的反序列化。 | ✓ | ✓ | ✓ | 502 |
| **5.5.4** | 驗證在瀏覽器或基於 JavaScript 的後端解析 JSON 時，使用 JSON.parse 來解析 JSON 文件。不使用 eval () 來解析 JSON。 | ✓ | ✓ | ✓ | 95 |

## 參考文獻

有關更多資訊，請參閱：

* [OWASP Testing Guide 4.0: Input Validation Testing](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/README.html)
* [OWASP Cheat Sheet: Input Validation](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
* [OWASP Testing Guide 4.0: Testing for HTTP Parameter Pollution](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/07-Input_Validation_Testing/04-Testing_for_HTTP_Parameter_Pollution.html)
* [OWASP LDAP Injection Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LDAP_Injection_Prevention_Cheat_Sheet.html)
* [OWASP Testing Guide 4.0: Client Side Testing](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/11-Client_Side_Testing/)
* [OWASP Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
* [OWASP DOM Based Cross Site Scripting Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html)
* [OWASP Java Encoding Project](https://owasp.org/owasp-java-encoder/)
* [OWASP Mass Assignment Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Mass_Assignment_Cheat_Sheet.html)
* [DOMPurify - Client-side HTML Sanitization Library](https://github.com/cure53/DOMPurify)
* [XML External Entity (XXE) Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/XML_External_Entity_Prevention_Cheat_Sheet.html)

有關自動轉義的更多資訊，請參閱：

* [Reducing XSS by way of Automatic Context-Aware Escaping in Template Systems](https://googleonlinesecurity.blogspot.com/2009/03/reducing-xss-by-way-of-automatic.html)
* [AngularJS Strict Contextual Escaping](https://docs.angularjs.org/api/ng/service/$sce)
* [AngularJS ngBind](https://docs.angularjs.org/api/ng/directive/ngBind)
* [Angular Sanitization](https://angular.io/guide/security#sanitization-and-security-contexts)
* [Angular Security](https://angular.io/guide/security)
* [ReactJS Escaping](https://reactjs.org/docs/introducing-jsx.html#jsx-prevents-injection-attacks)
* [Improperly Controlled Modification of Dynamically-Determined Object Attributes](https://cwe.mitre.org/data/definitions/915.html)

有關反序列化的更多資訊，請參閱：

* [OWASP Deserialization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html)
* [OWASP Deserialization of Untrusted Data Guide](https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)