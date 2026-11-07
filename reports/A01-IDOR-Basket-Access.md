Vulnerability : Insecure Direct Object Reference.
OWASP category : A01 Broken Access Control(2025)
Affected Endpoint : /rest/basket/6(id)
HTTP Method : GET
Impact : Unauthorized access to another user's basket
evidence: Account A retrieves account B's basket.
remediation : Enforce server-side ownership checks.
