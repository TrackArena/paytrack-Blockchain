# Security Policy

## Reporting Vulnerabilities

**Do not open a public GitHub issue for security vulnerabilities**, especially in smart contract code. A public disclosure of a contract vulnerability before it is patched could result in loss of funds.

Please report security issues by emailing **security@paytrack.dev** with:

1. A description of the vulnerability
2. Steps to reproduce
3. The potential impact
4. Any suggested fixes if you have them

You will receive a response within 48 hours.

## Smart Contract Security

Smart contracts in this repository handle real financial assets on Stellar mainnet. Before any contract is deployed to mainnet:

- Internal code review by at least two contributors
- Unit test coverage for all execution paths
- Integration tests on Stellar testnet
- External security audit for contracts handling significant value

## Responsible Disclosure

We ask that you:
- Give us reasonable time to patch before any public disclosure
- Not exploit the vulnerability beyond what is needed to demonstrate it
- Not access or modify data belonging to other users

We commit to:
- Acknowledging your report within 48 hours
- Keeping you informed of progress
- Crediting you in the fix if you wish
