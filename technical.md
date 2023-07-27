## Overview

### Tech Stacks
- Frontend: React.js
- Backend: Express, RESTful API
- Database: Mongo
- & other libs to make life easier (`lodash`, `crypto-js`, `jsonwebtoken`, etc.)

### Core
- User Authentication is not the point of this case study considering this invoice module will be implemented into an existing system.
- Three major modules for **Invoice Module**:
  - **Creation & Sharing**
  - **Payment**
  - **Monitoring**
  - **Dispute**
- Refer to the mongo schema [here](schema.md)

### Backend
  1. **Creation & Sharing**
     - `POST /invoice`: create `invoice`, `invoice.generatedPassword` will be generated and encrypted to allow the customer access the invoice.
     - `POST /invoice/id`: `password` is required to authenticate the customer for the specific `invoice` data.
  2. **Payment**
     - `POST /invoice/transaction`: update `invoice` and create `transaction`.
  3. **Monitoring**
     - `PATCH /invoice/id`:
       - primarily used for customer to acknowlegde the order/service has been fulfilled, thus releasing the fund to user.
       - update `invoice`.
       - The fund will be sent to the user wallet address/bank account on the server once the `invoice.fulfilledStatus` is updated to `'completed`', system will also update the `transaction.hasReleased`.
     - `GET /invoice`: retrieve all user's `invoices`
     - `GET /invoice/id`: retrieve single `invoice`
     - `GET /invoice/transaction`: retrieve all user's `transactions`
     - `GET /invoice/transaction/id`: retrieve single `transaction`
  4. **Dispute**
     - `POST /invoice/dispute`: create `dispute`, and update `invoice`.
     - `PATCH /invoice/dispute/id`: update `dispute`, and update `invoice`.
     - `GET /invoice/dispute/id`: fetch single `dispute` data.

### Frontend
  1. **Creation & Sharing**
     - A form for user to input the invoice data, send the data via request to `POST /invoice`.
     - Prepare a invoice sharing view which allows the customer to view the invoice, make payment, and acknowledge the fulfilling of order/service.
     - Invoice will be shared to customer via:
       - shareable url
       - email which redirects to the shareable url
       - `generatedPassword` acts as an authentication for the invoice.
  2. **Payment**
     - Customer is required to input the `password`, and able to view the invoice sharing view after authenticated, includes:
       - items
       - total amount
       - paid amount (will be updated if the customer pays in multiple terms)
     - Customer is allowed to click a button to make the payment:
       - Web3 libs suchs as `ether` and `bitcoinjs-lib` will be implemented at client side for crypto transactions.
       - The payment will be made to pathDao central wallet address or bank account depends on the selected payment method.
     - Frontend will make a API request to `POST /invoice/transaction` upon the success callback of crypto transaction, which updates the status of `invoice` and create `transaction`.
  3. **Monitoring**
     - User is able to view multiple invoices in table view via `GET /invoice`, or the details of a single invoice `GET /invoice/id`.
     - User is able to view multiple invoice's transactions in table view via `GET /invoice/transaction`, or the details of a single invoice's transaction `GET /invoice/transaction/id`.
     - Customer is able to acknowledge and approve the release of fund via clicking a button in the invoice sharing view, which make api request to `PATCH /invoice/id`.
  4. **Dispute**
     - Customer is able to initiate a dispute case before the invoice is partially/fully paid and before being marked fulfilled, by clicking a button in the invoice sharing view, which make api request to `POST /invoice/dispute`.
     - The process of dispute will however being handled by a customer support agent via emails.
     - `PATCH /invoice/dispute/id` is prepared however to be integrated into the customer support agent portal, to allow the agents update the dispute status.
     - User and Customer are able to monitor the status of the dispute case via `GET /invoice/dispute/id`.
