
## Known vulnerabilities

1. Insecure transport
2. User enumeration
3. Information disclosure
4. Authentication bypass
5. No input validation
6. SQL injection
8. Weak session token crypto
9. Poor session validation
10. Plaintext storage of secrets
11. Command injection
12. Regex denial of service
13. Cross Site Scripting
14. XML XXE and billion laughs
15. Missing security headers 

### Vulnerabilities per endpoint

| method | endpoint       | input               | vuln            
|--------|----------------|---------------------|-----------------
| GET    | /              | -                   | 15              
| GET    | /tokens        | -                   | 10              
| POST   | /tokens        | post                | 14              
| POST   | /tokens        | post:username       | 2, 6, 8, 13, 14 
| POST   | /tokens        | post:password       | 2, 6, 8, 13, 14 
| GET    | /user/{userid} | header:x-auth-token | 4, 6, 8, 9      
| GET    | /user/{userid} | get: userid         | 2, 6, 10, 12    
| POST   | /user          | header:x-auth-token | 4, 6, 8, 9      
| POST   | /user          | post:username       | 6, 9, 12, 13    
| POST   | /user          | post:username       | 6, 9, 13        
| GET    | /uptime{flag}  | -                   | 11, 13
| POST   | /widget        | header:x-auth-token | 4, 6, 8, 9      
| POST   | /widget        | post:widget         | 12              

