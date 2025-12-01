# Revolut Open Banking API – Requirements

## R1: Authentication & Security
- R1.1: The API must require valid authentication (OAuth token) for all protected endpoints.
- R1.2: Requests with missing or invalid tokens must return an appropriate error status (e.g. 401 Unauthorized).
- R1.3: Sensitive data must not be returned for unauthenticated or unauthorized requests.

## R2: Accounts
- R2.1: The system must allow the client to retrieve a list of all available accounts.
- R2.2: Each account in the list must contain at least: account id, name/description, currency and balance.
- R2.3: The client must be able to retrieve details for a single account by id.
- R2.4: Requests for a non‑existing account id must return an appropriate error (e.g. 404 Not Found).

## R3: Transactions
- R3.1: The system must allow the client to retrieve transactions for a given account.
- R3.2: It must be possible to filter transactions by date range (from / to).
- R3.3: The response must include basic information per transaction (amount, currency, date, description).
- R3.4: If the account has no transactions for the given period, the API must return an empty list without errors.

## R4: Payments (if available in sandbox)
- R4.1: The client must be able to initiate a domestic payment.
- R4.2: The API must validate mandatory fields (amount, currency, debtor, creditor, account identifiers).
- R4.3: Invalid or negative amounts must be rejected with a clear validation error.
- R4.4: The payment status must be returned (e.g. pending, completed, failed).

## R5: Error Handling & Stability
- R5.1: All endpoints must return meaningful HTTP status codes (2xx success, 4xx client errors, 5xx server errors).
- R5.2: Error responses must include an error code and human‑readable message.
- R5.3: The API must handle unexpected input gracefully without crashing.

