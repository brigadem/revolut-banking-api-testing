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
    
## TS011 – List accounts with pagination (if supported)
- **Related requirement**: R2.1
- **Precondition**: API supports pagination parameters (e.g. page, size / limit, offset).
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
  - Response contains an empty list or appropriate indication that no accounts exist on that page.

## TS013 – List accounts with invalid page parameter
- **Related requirement**: R5.1, R5.2
- **Steps**:
  1. Send GET /accounts?page=-1&size=10 (or non‑numeric value).
- **Expected result**:
  - API returns HTTP 4xx.
  - Response contains a validation error describing invalid pagination parameters.

## TS014 – Filter accounts by type (if supported)
- **Related requirement**: R2.1
- **Precondition**: API supports filtering by account type (e.g. current, savings).
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
  - Every account object in the response contains id, name/description, currency and balance fields.
