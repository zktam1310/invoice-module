## Mongo Schema

### Users
|_id|`ObjectId`|
|-|-|
|fullName|`string`|
|email|`string`|
|phoneNo|`string`|
|address|`string`|
|organization?|`string`|
|updatedAt|`Date`|
|createdAt|`Date`|

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
|items|`array`|
|subtotalAmount|`number`|
|taxCharges|`number`|
|otherCharges|`number`|
|totalAmount|`number`|
|paidAmount|`number`|
|status|`'issued' \| 'partial' \| 'paid \|'cancelled'`|
|metaData?|`{[field: string]: any}`|
|updatedAt|`Date`|
|createdAt|`Date`|


#### Invoices.$.items
|title|`string`|
|-|-|
|description|`string`|
|unitPrice|`number`|
|quantity|`number`|
|totalAmount|`number`|
|remarks?|`string`|

*default currency in invoice would be `usdt`, value in other currencies will be computed upon payment made.

### Transactions
|_id|ObjectId|
|-|-|
|userId|`ObjectId`|
|customerId|`ObjectId`|
|invoiceId|`ObjectId`|
|amount|`number`|
|coin?|`'btc' \| 'eth' \| 'usdt' \| 'usdc'`|
|paymentMethod|`'crypto' \| 'fiat'`|
|referenceNo?|`string`|
|metaData?|`{[field: string]: any}`|
|paidAt|`Date`|
|createdAt|`Date`|

*`coin` only required for `'crypto'` payment, `'usd'` will be the default for `'fiat'` payment.