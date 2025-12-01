# Test Scenarios – Revolut Open Banking API

## TS001 – List accounts with valid token
- **Related requirement**: R2.1, R1.1
- **Precondition**: Valid OAuth access token is available.
- **Steps**:
  1. Send GET /accounts with valid Authorization header.
- **Expected result**:
  - API returns HTTP 200.
  - Response body contains a non‑empty list of accounts.

## TS002 – List accounts with invalid token
- **Related requirement**: R1.2
- **Precondition**: Invalid or expired token is used.
- **Steps**:
  1. Send GET /accounts with invalid Authorization header.
- **Expected result**:
  - API returns HTTP 401 (or similar).
  - Response body contains an error message about authentication.

## TS003 – Get single account by valid id
- **Related requirement**: R2.2, R2.3
- **Precondition**: At least one account id is known from TS001.
- **Steps**:
  1. Send GET /accounts/{accountId} with existing account id.
- **Expected result**:
  - API returns HTTP 200.
  - Response body contains details: id, name/description, currency, balance.

## TS004 – Get single account with invalid id
- **Related requirement**: R2.4
- **Steps**:
  1. Send GET /accounts/{accountId} with a non‑existing id.
- **Expected result**:
  - API returns HTTP 404 (or similar).
  - Response body contains an error about account not found.

## TS005 – List transactions for account
- **Related requirement**: R3.1, R3.3
- **Precondition**: Valid account id is available.
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions.
- **Expected result**:
  - API returns HTTP 200.
  - Response contains a list of transactions with amount, currency, date, description.

## TS006 – List transactions with date range filter
- **Related requirement**: R3.2
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions?from=YYYY‑MM‑DD&to=YYYY‑MM‑DD.
- **Expected result**:
  - API returns HTTP 200.
  - Only transactions within the given date range are returned.

## TS007 – Transactions when no data
- **Related requirement**: R3.4
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions for a period with no movements.
- **Expected result**:
  - API returns HTTP 200.
  - Response contains an empty list, no error.

## TS008 – Initiate valid domestic payment (if supported)
- **Related requirement**: R4.1, R4.2
- **Steps**:
  1. Send POST /payments/domestic with all mandatory fields filled correctly.
- **Expected result**:
  - API returns HTTP 201 or 200.
  - Response contains created payment and status (e.g. pending or completed).

## TS009 – Initiate payment with invalid amount
- **Related requirement**: R4.3
- **Steps**:
  1. Send POST /payments/domestic with negative or zero amount.
- **Expected result**:
  - API returns HTTP 4xx.
  - Response contains validation error about amount.

## TS010 – General error handling
- **Related requirement**: R5.1, R5.2
- **Steps**:
  1. Trigger a request with clearly wrong input (e.g. wrong path or method).
- **Expected result**:
  - API returns appropriate HTTP 4xx/5xx.
  - Error body includes error code and human‑readable message.

## TS011 – List accounts with pagination
- **Related requirement**: R2.1
- **Precondition**: API supports pagination parameters.
- **Steps**:
  1. Send GET /accounts?page=1&size=10.
- **Expected result**:
  - API returns HTTP 200.
  - Response contains at most 10 accounts and pagination metadata (if defined).

## TS012 – List accounts with extreme page number
- **Related requirement**: R2.1, R5.1
- **Steps**:
  1. Send GET /accounts?page=9999&size=10.
- **Expected result**:
  - API returns HTTP 200 or 204.
  - Response contains an empty list or indication that no accounts exist on that page.

## TS013 – List accounts with invalid page parameter
- **Related requirement**: R5.1, R5.2
- **Steps**:
  1. Send GET /accounts?page=-1&size=10 (or non‑numeric value).
- **Expected result**:
  - API returns HTTP 4xx.
  - Response contains a validation error describing invalid pagination parameters.

## TS014 – Filter accounts by type
- **Related requirement**: R2.1
- **Precondition**: API supports filtering by account type.
- **Steps**:
  1. Send GET /accounts?type=current.
- **Expected result**:
  - API returns HTTP 200.
  - All returned accounts have type = current.

## TS015 – Verify mandatory fields in account response
- **Related requirement**: R2.2
- **Precondition**: At least one account exists.
- **Steps**:
  1. Send GET /accounts.
- **Expected result**:
  - Every account object contains id, name/description, currency and balance fields.

## TS016 – List transactions with invalid account id
- **Related requirement**: R3.1, R2.4
- **Steps**:
  1. Send GET /accounts/{invalidId}/transactions.
- **Expected result**:
  - API returns HTTP 404 (or similar).
  - Error indicates that the account does not exist.

## TS017 – Transactions with invalid date range
- **Related requirement**: R3.2, R5.2
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions?from=2025-12-31&to=2025-01-01.
- **Expected result**:
  - API returns HTTP 4xx.
  - Response contains a validation error about invalid date range.

## TS018 – Transactions with missing date format
- **Related requirement**: R3.2, R5.2
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions?from=2025/01/01.
- **Expected result**:
  - API returns HTTP 4xx.
  - Response describes invalid date format.

## TS019 – Transactions with large result set
- **Related requirement**: R3.1, R5.3
- **Precondition**: Account has many historical transactions.
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions without filters.
- **Expected result**:
  - API returns HTTP 200 within acceptable response time.
  - Response includes all or paginated transactions without timeout.

## TS020 – Transactions with pagination
- **Related requirement**: R3.1
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions?page=1&size=20.
- **Expected result**:
  - API returns HTTP 200.
  - At most 20 transactions are returned, plus pagination info if available.

## TS021 – Transactions filtered by amount (if supported)
- **Related requirement**: R3.3
- **Precondition**: API supports filtering or sorting by amount.
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions?minAmount=100.
- **Expected result**:
  - API returns HTTP 200.
  - All returned transactions have amount >= 100.

## TS022 – Transactions filtered by type (credit/debit)
- **Related requirement**: R3.3
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions?type=credit.
- **Expected result**:
  - API returns HTTP 200.
  - All transactions are of type credit.

## TS023 – Unauthorized transactions request
- **Related requirement**: R1.2
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions with missing Authorization header.
- **Expected result**:
  - API returns HTTP 401.
  - Response explains that authentication is required.

## TS024 – Transactions for closed or blocked account (if supported)
- **Related requirement**: R3.1, R5.1
- **Precondition**: Account status can be closed/blocked in sandbox.
- **Steps**:
  1. Send GET /accounts/{closedAccountId}/transactions.
- **Expected result**:
  - API returns appropriate HTTP status (e.g. 403 or 404).
  - Response explains that the account is not available.

## TS025 – Transactions response schema validation
- **Related requirement**: R3.3
- **Steps**:
  1. Send GET /accounts/{accountId}/transactions.
- **Expected result**:
  - Each transaction object contains required fields (amount, currency, date, description, id).

## TS026 – Create valid domestic payment
- **Related requirement**: R4.1, R4.2
- **Steps**:
  1. Send POST /payments/domestic with valid debtor, creditor, amount and currency.
- **Expected result**:
  - API returns HTTP 201 or 200.
  - Payment resource is created with status pending or completed.

## TS027 – Payment with missing mandatory field
- **Related requirement**: R4.2
- **Steps**:
  1. Send POST /payments/domestic without debtor account.
- **Expected result**:
  - API returns HTTP 4xx.
  - Response includes validation error for missing debtor account.

## TS028 – Payment with invalid currency
- **Related requirement**: R4.2, R4.3
- **Steps**:
  1. Send POST /payments/domestic with unsupported currency code.
- **Expected result**:
  - API returns HTTP 4xx.
  - Response states that currency is not supported.

## TS029 – Payment with insufficient funds (if simulated)
- **Related requirement**: R4.3, R5.3
- **Precondition**: Sandbox can simulate insufficient balance.
- **Steps**:
  1. Send POST /payments/domestic from low-balance account with high amount.
- **Expected result**:
  - API returns HTTP 4xx.
  - Response indicates insufficient funds.

## TS030 – Duplicate payment submission
- **Related requirement**: R4.1, R5.3
- **Steps**:
  1. Send POST /payments/domestic with same idempotency key or identical payload twice.
- **Expected result**:
  - Second request is either rejected as duplicate or returns same payment resource.

## TS031 – Retrieve payment status
- **Related requirement**: R4.4
- **Precondition**: A payment has been created.
- **Steps**:
  1. Send GET /payments/{paymentId}.
- **Expected result**:
  - API returns HTTP 200.
  - Response includes current payment status.

## TS032 – Cancel or reverse payment (if supported)
- **Related requirement**: R4.4, R5.1
- **Precondition**: Payment supports cancellation in given status.
- **Steps**:
  1. Send POST /payments/{paymentId}/cancel (or relevant endpoint).
- **Expected result**:
  - API returns HTTP 200 or 204.
  - Payment status changes accordingly (e.g. cancelled).

## TS033 – Payment with invalid debtor account
- **Related requirement**: R4.2
- **Steps**:
  1. Send POST /payments/domestic with non-existing debtor account id.
- **Expected result**:
  - API returns HTTP 4xx.
  - Response explains that debtor account is invalid.

## TS034 – Payment with invalid creditor details
- **Related requirement**: R4.2
- **Steps**:
  1. Send POST /payments/domestic with malformed creditor IBAN or account.
- **Expected result**:
  - API returns HTTP 4xx.
  - Response includes validation error for creditor details.

## TS035 – Payment rate limiting (if enforced)
- **Related requirement**: R5.3
- **Steps**:
  1. Send multiple POST /payments/domestic requests in a very short time.
- **Expected result**:
  - API either handles all requests or returns rate-limit error (e.g. HTTP 429) with appropriate message.

## TS036 – Unauthorized access to another user’s account (if scenario exists)
- **Related requirement**: R1.1, R1.3
- **Precondition**: Sandbox can simulate multiple users or consents.
- **Steps**:
  1. Use token for user A and try to access account of user B.
- **Expected result**:
  - API returns HTTP 403 or 404.
  - No sensitive data of user B is exposed.

## TS037 – Invalid HTTP method
- **Related requirement**: R5.1
- **Steps**:
  1. Send PUT /accounts instead of GET /accounts.
- **Expected result**:
  - API returns HTTP 405 Method Not Allowed or similar.
  - Response indicates that method is not supported.

## TS038 – Invalid endpoint path
- **Related requirement**: R5.1, R5.2
- **Steps**:
  1. Send GET /accountsss (typo in path).
- **Expected result**:
  - API returns HTTP 404.
  - Response indicates resource not found.

## TS039 – Large payload handling (payments)
- **Related requirement**: R5.3
- **Steps**:
  1. Send POST /payments/domestic with extremely long description field.
- **Expected result**:
  - API either truncates safely or returns validation error.
  - No crash or unexpected server error.

## TS040 – Basic performance check for critical endpoints
- **Related requirement**: R5.3
- **Steps**:
  1. Measure response time for GET /accounts and GET /accounts/{accountId}/transactions.
- **Expected result**:
  - Responses return within acceptable time (e.g. < 2 seconds in sandbox conditions).
