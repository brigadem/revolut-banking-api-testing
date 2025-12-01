# Test Execution Status – Revolut Open Banking API

| Test ID | Endpoint        | Status | Notes                          |
|--------|-----------------|--------|--------------------------------|
| TS001  | GET /accounts   | FAIL   | Received 403 Forbidden in Postman (auth/consent not completed yet). |
> Execution note: First run returned 403 Forbidden in sandbox (likely missing access token / consent). Will re-test after auth flow is configured.

