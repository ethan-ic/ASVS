# V5: Validation, Sanitization and Encoding Verification Requirements

## Control Objective

The most common web application security weakness is the failure to properly validate input coming from the client or the environment before using it. This weakness leads to almost all of the significant vulnerabilities in web applications, such as Cross-Site Scripting (XSS), SQL injection, interpreter injection, locale/Unicode attacks, file system attacks, and buffer overflows.

Ensure that a verified application satisfies the following high-level requirements:

* Input validation and output encoding architecture have an agreed pipeline to prevent injection attacks.
* Input data is strongly typed, validated, range or length checked, or at worst, sanitized or filtered.
* Output data is encoded as per the context of the data as close to the interpreter as possible.

## 5.1 Input Validation Reqirements

| # | Description | L1 | L2 | L3 | Since |
| --- | --- | --- | --- | -- | -- |
| 5.1.1 | Verify that server-side input validation failures result in request rejection and are logged. | ✓ | ✓ | ✓ | 1.0 |
| 5.1.2 | Verify that input validation is enforced on the server-side. | ✓ | ✓ | ✓ | 1.0 |
| 5.1.3 | Verify that the application uses a centralized input validation control mechanism. | ✓ | ✓ | ✓ | 1.0 |
| 5.1.4 | Verify that all untrusted HTML input from WYSIWYG editors or similar is properly sanitized with an HTML sanitizer library or framework feature.  | ✓ | ✓ | ✓ | 3.0 |
| 5.1.5 | Verify that the application has defenses against HTTP parameter pollution attacks, particularly if the application framework makes no distinction about the source of request parameters (GET, POST, cookies, headers, or environment variables). |  | ✓ | ✓ | 2.0 |
| 5.1.6 | Verify that all input data is validated, not only HTML form fields but all sources of input such as REST calls, query parameters, HTTP headers, cookies, batch files, or RSS feeds; using positive validation (whitelisting), then lesser forms of validation such as greylisting (eliminating known bad strings), or rejecting bad inputs (blacklisting). |  | ✓ | ✓ | 3.0 |
| 5.1.7 | Verify that structured data is strongly typed and validated against a defined schema including allowed characters, length and pattern (e.g. credit card numbers or telephone, or validating that two related fields are reasonable, such as validating suburbs and zip or postcodes match).  |  | ✓ | ✓ | 3.0 |
| 5.1.8 | Verify that unstructured data is sanitized to enforce safety measures such as allowed characters and length, and characters potentially harmful in given context should be escaped (e.g. names with Unicode or apostrophes, such as ねこ or O'Hara). |  | ✓ | ✓ | 3.0 |

## 5.2 Output Encoding Requirements

| # | Description | L1 | L2 | L3 | Since |
| --- | --- | --- | --- | -- | -- |
| 5.2.1 | Verify that output encoding occurs close to or by the interpreter for which it is intended. | ✓ | ✓ | ✓ | 4.0 |
| 5.2.2 | Verify that output encoding is contextually relevant for the interpreter and context required. For example, using encoders specifically for HTML values, attributes, HTTP headers, JSON, SMTP, as the case requires. | ✓ | ✓ | ✓ | 4.0 |
| 5.2.3 | Verify that output encoding preserves the user's chosen character set and locale, such that any Unicode character point is valid and safely handled. | ✓ | ✓ | ✓ | 4.0 |
| 5.2.4 | Verify that context-aware, preferably automated - or at worst, manual - output escaping protects against reflected, stored, and DOM based XSS. | ✓ | ✓ | ✓ | 4.0 |
| 5.2.5 | Verify that any user-supplied data included in the browser's DOM or web views protects against JavaScript code execution and XSS attacks. | ✓ | ✓ | ✓ | 4.0 |
| 5.2.6 | Verify that where parameterized or safer mechanisms are not present, that context-specific output encoding is used to protect against injection attacks, such as the use of SQL escaping to protect against SQL injection. | ✓ | ✓ | ✓ | 4.0 |

Note that escaping SQL is not always sufficient; table and column names, ORDER BY and so on, cannot be escaped. The inclusion of escaped user-supplied data in these fields results in failed queries or SQL injection.

## 5.3 Injection Prevention Requirements

| # | Description | L1 | L2 | L3 | Since |
| --- | --- | --- | --- | -- | -- |
| 5.3.1 | Verify that data selection or database queries (e.g. SQL, HQL, ORM, NoSQL) use parameterized queries, ORMs, entity frameworks, or otherwise protected from database injection attacks. | ✓ | ✓ | ✓ | 4.0 |
| 5.3.2 | Verify that LDAP queries are not susceptible to LDAP Injection by using either parameterized LDAP queries or the use of contextual LDAP output encoding. | ✓ | ✓ | ✓ | 2.0 |
| 5.3.3 | Verify that operating system calls are not susceptible to OS command injection by using either parameterized OS queries or the use of contextual command line output encoding. | ✓ | ✓ | ✓ | 2.0 |
| 5.3.4 | Verify that file and path handling is not susceptible to Local File Inclusion (LFI) or Remote File Inclusion (RFI) attacks. | ✓ | ✓ | ✓ | 3.0 |
| 5.3.5 | Verify that XML parsing and queries are not susceptible to XPath injection or XML injection attacks. | ✓ | ✓ | ✓ | 2.0 |
| 5.3.6 | Verify that where potentially untrusted data is copied one DOM context to another, the transfer uses safe JavaScript methods, such as using innerText or JQuery .val to ensure the application is not susceptible to DOM Cross-Site Scripting (XSS) attacks. | ✓ | ✓ | ✓ | 4.0 |
| 5.3.7 | Verify that the application protects against SMTP or IMAP injection. | ✓ | ✓ | ✓ | 4.0 |
| 5.3.8 | Verify that the application protects against eval(), template injection, or remote code execution attacks. | ✓ | ✓ | ✓ | 4.0 |
| 5.3.9 | Verify that frameworks protect against mass parameter assignment attacks, or that the application has countermeasures to protect against unsafe parameter assignment, such as marking fields private or similar. |  | ✓ | ✓ | 4.0 |
| 5.3.10 | Verify that if the application uses a systems language or unmanaged code, it uses memory-safe string, safer memory copy and pointer arithmetic to detect or prevent stack, buffer, or heap overflows. |  | ✓ | ✓ | 4.0 |
| 5.3.11 | Verify that if the application uses a systems language or unmanaged code, format strings do not take potentially hostile input, and are constant. |  | ✓ | ✓ | 4.0 |
| 5.3.12 | Verify that if the application uses a systems language or unmanaged code, sign, range, and input validation techniques are used to prevent integer overflows. |  | ✓ | ✓ | 4.0 |
| 5.3.13 | Verify that if the application uses a systems language or unmanaged code, compiler flags are configured to enable all available buffer overflow protections and warnings, including stack randomization, data execution prevention, and to break the build if an unsafe pointer, memory, format string, integer, or string operations are found. |  |  | ✓ | 4.0 |

## 5.4 Deserialization Prevention Requirements

| # | Description | L1 | L2 | L3 | Since |
| --- | --- | --- | --- | -- | -- |
| 5.4.1 | Verify when parsing JSON in browsers or JavaScript-based backends, that JSON.parse is used to parse the JSON document. Do not use eval() to parse JSON. |  | ✓ | ✓ | 3.0 |
| 5.4.2 | Verify that untrusted data, particularly filenames and URL input fields, use positive input validation and output encoding methods to protect against Server-Side Request Forgery (SSRF) vulnerabilities. | ✓ | ✓ | ✓ | 4.0 |
| 5.4.3 | Verify that the application correctly restricts XML parsers to only use the most restrictive configuration possible and to ensure that unsafe features such as resolving external entities are disabled.  | ✓ | ✓ | ✓ | 4.0
| 5.4.4 | Verify that deserialization of untrusted data is avoided or is protected in both custom code and third-party libraries (such as JSON, XML and YAML parsers).  | ✓ | ✓ | ✓ | 4.0

## References

For more information, see also:

* [OWASP Testing Guide 4.0: Input Validation Testing](https://www.owasp.org/index.php/Testing_for_Input_Validation)
* [OWASP Cheat Sheet: Input Validation](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet)
* [OWASP Testing Guide 4.0: Testing for HTTP Parameter Pollution](https://www.owasp.org/index.php/Testing_for_HTTP_Parameter_pollution_%28OTG-INPVAL-004%29)
* [OWASP LDAP Injection Cheat Sheet ](https://www.owasp.org/index.php/LDAP_Injection_Prevention_Cheat_Sheet)
* [OWASP Testing Guide 4.0: Client Side Testing ](https://www.owasp.org/index.php/Client_Side_Testing)
* [OWASP Cross Site Scripting Prevention Cheat Sheet ](https://www.owasp.org/index.php/XSS_%28Cross_Site_Scripting%29_Prevention_Cheat_Sheet)
* [OWASP DOM Based Cross Site Scripting Prevention Cheat Sheet ](https://www.owasp.org/index.php/DOM_based_XSS_Prevention_Cheat_Sheet)
* [OWASP Java Encoding Project](https://www.owasp.org/index.php/OWASP_Java_Encoder_Project)
* [OWASP Mass Binding Prevention Cheat Sheet](https://www.owasp.org/index.php/Mass_Assignment_Cheat_Sheet)

For more information on auto-escaping, please see:

* [Reducing XSS by way of Automatic Context-Aware Escaping in Template Systems](http://googleonlinesecurity.blogspot.com/2009/03/reducing-xss-by-way-of-automatic.html)
* [AngularJS Strict Contextual Escaping](https://docs.angularjs.org/api/ng/service/$sce)
* [AngularJS ngBind](https://docs.angularjs.org/api/ng/directive/ngBind)
* [Angular  Sanitzation](https://angular.io/guide/security#sanitization-and-security-contexts)
* [Angular Template Security](https://angular.io/guide/template-syntax#content-security)
* [ReactJS Escaping](https://reactjs.org/docs/introducing-jsx.html#jsx-prevents-injection-attacks)
* [Improperly Controlled Modification of Dynamically-Determined Object Attributes](https://cwe.mitre.org/data/definitions/915.html)