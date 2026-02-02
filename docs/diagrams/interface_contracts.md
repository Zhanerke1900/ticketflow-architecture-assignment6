# Interface Contracts (Source)

> This file is a **source** version of the contract tables for your repo.
> You can keep the official tables in the PDF; this is here so the repo has a clear, editable copy.

## REST
| Method | Endpoint | Request | Response | Errors | Auth |
|---|---|---|---|---|---|
| POST | /auth/register | {email, password, name} | 201 {jwt, user} | 400, 409 | Public |
| POST | /auth/login | {email, password} | 200 {jwt, user} | 401 | Public |
| GET | /events | q, city, date, page | 200 [Event] | 400 | Optional |
| GET | /events/{id}/seats | — | 200 {seatMap, availability} | 404 | Optional |
| POST | /orders | {eventId, seatIds[]} | 201 {orderId, status} | 409, 422 | JWT |
| POST | /payments/{orderId}/init | {returnUrl} | 200 {checkoutUrl} | 404, 409 | JWT |
| POST | /payments/webhook | provider payload + signature | 200 OK | 401, 409 | Provider signature |
| GET | /tickets/{orderId} | — | 200 {ticketId, qrPayload} | 404, 409 | JWT |
| POST | /validate | {qrPayload} | 200 {valid, status} | 400, 409 | Staff JWT |

## Events
| Event | Topic | Producer | Payload (key fields) |
|---|---|---|---|
| OrderCreated | orders.created | Order | {orderId, userId, amount, seatIds[]} |
| PaymentConfirmed | payments.confirmed | Payment | {orderId, paymentId, paidAt} |
| PaymentFailed | payments.failed | Payment | {orderId, reasonCode} |
| TicketIssued | tickets.issued | Ticket | {ticketId, orderId, qrHash} |
| TicketValidated | tickets.validated | Validation | {ticketId, scannedAt, result} |
