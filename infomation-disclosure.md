# Infomation Disclosure in Wordpress Error Response — CWE-765

**Status:** `Duplicate`
**Severity (if rated):** High (8.6)
**Date Found:** 2026-11-02
**Date Resolved/Closed:** N/A
**Program:** Redacted

---

## Summary

An information disclosure vulnerability in the target's WordPress error handling exposed backend API credentials (UUID) to material non-public infomation (MNPI).

---

## Methodology

- **Recon approach:** Editing requests with unexpected input to trigger errors, in this case I injected a NoSQL query into a url parameter.
- **Testing methodology used:** OWASP Testing Guide, Testing for Error Handling -> https://wstg.owasp.org/v4.2/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling/
- **Tools:** Burp Suite

---

## Technical Detail

1. Collected a bunch of requests with burpsuite
2. Analyzed interesting input fields that cause changes in the response
3. Inserted payload based on presumed context that the variable is used to query a database.

---

## Impact

Attacker get's access to resources not released to the public yet.

---

## Outcome

- Closed as Informative — real vulnerability, but proof-of-impact was limited by program scope rules.

---

## What I'd Do Differently / Lessons Learned

- Next time I'll figure out a way to get more information using the UUID and check if there are other UUID's that lead to more serious leaks.

__Lesson: If it doesn't leak serious company data it is useless to the company.__

---

## Disclosure Note

*Target identity redacted per responsible disclosure norms. Vulnerability class and methodology shared for educational purposes. No real user data was accessed or exposed in the course of this research.*
