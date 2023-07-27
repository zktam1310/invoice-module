## Mongo Schema

### Users
|_id|`ObjectId`|
|-|-|
|fullName|`string`|
|email|`string`|
|phoneNo|`string`|
|address|`string`|
|walletAddresses|`IUserWalletAddress[]`|
|bankAccounts?|`IUserBankAccount[]`|
|organization?|`string`|
|updatedAt|`Date`|
|createdAt|`Date`|

#### IUserWalletAddress (Users.$.walletAddresses)
|address|`string`|
|-|-|
|coin|`'btc' \| 'eth' \| 'usdt' \| 'usdc'`|

#### IUserBankAccount (Users.$.bankAccounts)
|accountNumber|`string`|
|-|-|
|swiftCode|`string`|

### Customers
|_id|`ObjectId`|
|-|-|
|fullName|`string`|
|email|`string`|
|phoneNo|`string`|
|address|`string`|
|organization?|`string`|
|updatedAt|`Date`|
|createdAt|`Date`|

### Invoices
|_id|`ObjectId`|
|-|-|
|items|`IInvoiceItem[]`|
|subtotalAmount|`number`|
|taxCharges|`number`|
|otherCharges|`number`|
|totalAmount|`number`|
|paidAmount|`number`|
|status|`'issued' \| 'partial' \| 'paid \|'cancelled'`|
|metaData?|`{[field: string]: any}`|
|updatedAt|`Date`|
|createdAt|`Date`|


#### IInvoiceItem (Invoices.$.items)
|title|`string`|
|-|-|
|description|`string`|
|unitPrice|`number`|
|quantity|`number`|
|totalAmount|`number`|
|remarks?|`string`|

**remarks**:
* default currency in invoice would be `usdt`, value in other currencies will be computed upon payment made.

### Transactions
|_id|`ObjectId`|
|-|-|
|userId|`ObjectId`|
|customerId|`ObjectId`|
|invoiceId|`ObjectId`|
|amount|`number`|
|coin?|`'btc' \| 'eth' \| 'usdt' \| 'usdc'`|
|paymentMethod|`'crypto' \| 'fiat'`|
|fromWalletAddress?|`string`|
|toWalletAddress?|`string`|
|fromBankAcc?|`ITransactionBankAccount`|
|toBankAcc?|`ITransactionBankAccount`|
|referenceNo?|`string`|
|metaData?|`{[field: string]: any}`|
|paidAt|`Date`|
|createdAt|`Date`|

#### ITransactionBankAccount (Transactions.\$.fromWalletAddress & Transactions.\$.toWalletAddress)
|accountNo|`string`|
|-|-|
|swiftCode|`string`|

**remarks**:
* if `(paymentMethod === 'crypto)`
  * `coin` is required.
  * `fromWalletAddress` and `toWalletAddress` are required
* else if `(paymentMethod === 'fiat)`
  * `'usd'` will be the default currency
  * `fromWalletAddress` and `toWalletAddress` are required
