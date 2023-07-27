## Others

### Security measures
- As one of the industry standard, JWT token will be implemented for user authentication.
- System generated password will be implemented to strengthen the security of invoice access.
- Considering adding a feature for the user to reset the system generated password for the customer.
- pathDao will act as the middleman of holding the transactions' fund until the invoice is deemed fulfilled by both parties.

### Disputes
- It is further documented in [Technical](technical.md)

### Scaling Up
- The current proposed technical technologies and data structures are expected to work effectively for a very long time.
- However, these are the technologies which can be considered to adopt for a better perfomance (in a very long run):
  1. Fastify as backend framework, to facilitate more requests at the same time.
  2. Clickhouse as database, to facilitate more write throughput of data.