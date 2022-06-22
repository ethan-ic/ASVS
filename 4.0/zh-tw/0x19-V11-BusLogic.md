# V11 業務邏輯

## 控制目標

確保經過驗證的應用程式滿足以下高階要求：

* 業務邏輯流程是序列的，按順序處理的，並且不能被繞過。
* 業務邏輯包括檢測和防止自動化攻擊，如連續的小額資金轉移，或一次新增上百萬個好友等。
* 高價值的業務邏輯流已經考慮了濫用情況和惡意行為者，並有防止欺騙、篡改、資訊披露和特權提升攻擊的保護措施。

## V11.1 業務邏輯安全

業務邏輯安全對每個應用程式來說都是非常獨特的，因此沒有通用的檢查表。業務邏輯安全必須設計成能夠抵禦可能的外部威脅 —— 它不能使用 Web 應用防火牆或安全通訊來新增。我們建議在設計衝刺（Design Sprint）期間使用威脅建模，例如使用 OWASP Cornucopia 或類似工具。

| # | 描述 | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **11.1.1** | 驗證應用程式僅按序列順序處理同一使用者的業務邏輯流，不會跳過步驟。 | ✓ | ✓ | ✓ | 841 |
| **11.1.2** | 驗證應用程式將只處理業務邏輯流，所有步驟都在現實的人工時間內處理，即事務不會提交得太快。 | ✓ | ✓ | ✓ | 799 |
| **11.1.3** | 驗證應用程式是否對特定的業務操作或交易有適當的限制，並在每個使用者的基礎上正確執行。 | ✓ | ✓ | ✓ | 770 |
| **11.1.4** | 驗證應用程式具有反自動化的控制手段，以防止過度呼叫，如大量資料洩露、業務邏輯請求、檔案上傳或拒絕服務攻擊。 | ✓ | ✓ | ✓ | 770 |
| **11.1.5** | 驗證應用程式是否具有業務邏輯限制或驗證，以防止可能的業務風險或威脅（使用威脅建模或類似方法識別）。 | ✓ | ✓ | ✓ | 841 |
| **11.1.6** | 驗證應用程式是否存在 TOCTOU（Time Of Check to Time Of Use）問題 或敏感操作的其他條件競爭問題。 | | ✓ | ✓ | 367 |
| **11.1.7** | 驗證應用程式是否從業務邏輯角度監控異常事件或活動。例如，嘗試執行無序的操作或普通使用者永遠不會嘗試的操作。 ([C9](https://owasp.org/www-project-proactive-controls/#div-numbering)) | | ✓ | ✓ | 754 |
| **11.1.8** | 驗證應用程式在檢測到自動化攻擊或異常活動時，具有可配置的警報。 | | ✓ | ✓ | 390 |

## 參考文獻

有關更多資訊，請參閱：

* [OWASP Web Security Testing Guide 4.1: Business Logic Testing](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/10-Business_Logic_Testing/README.html)
* 反自動化可以通過多種方式實現，包括使用 [OWASP AppSensor](https://github.com/jtmelton/appsensor) 和 [OWASP Automated Threats to Web Applications](https://owasp.org/www-project-automated-threats-to-web-applications/)
* [OWASP AppSensor](https://github.com/jtmelton/appsensor) 也可以幫助進行攻擊檢測和響應。
* [OWASP Cornucopia](https://owasp.org/www-project-cornucopia/)