# Decentralized Identity Verification Contract

## Overview

The Decentralized Identity Verification Contract is a Clarity smart contract designed for the Stacks blockchain that provides a privacy-preserving identity verification system with customizable trust levels. This contract enables third-party identity providers to verify users while keeping personal data off-chain and only storing verification metadata on-chain.

**Version:** 1.0.0

## Key Features

- **Customizable Trust Levels**: Support for multiple trust levels (1-5) that can be assigned based on verification methods and identity providers
- **Privacy-Preserving**: Only stores verification hashes on-chain - actual personal data remains off-chain
- **Multiple Provider Support**: Can integrate with different identity verification providers, each with their own trust scores
- **Service-Specific Requirements**: Different applications can set their own verification requirements
- **KYC/AML Compliance**: Designed to support Know Your Customer (KYC) and Anti-Money Laundering (AML) compliance needs
- **Expiration Management**: Verifications include expiration timestamps for security

## Contract Structure

The contract maintains three primary data maps:

1. **identity-providers**: Stores information about trusted verification providers
   - Provider name
   - Trust score (1-5)
   - Active status

2. **user-identities**: Tracks user verification status
   - Verification status
   - Trust level
   - Provider ID
   - Verification hash (hash of verification data)
   - Verification and expiration timestamps

3. **verification-requirements**: Defines verification requirements for different services
   - Required trust level
   - Required providers
   - KYC/AML flags

## Main Functions

### For Contract Owners

- `register-user`: Allows users to register in the system
- `add-provider`: Adds trusted identity providers to the system
- `update-provider-status`: Updates a provider's active status
- `set-verification-requirements`: Sets verification standards for different services
- `transfer-ownership`: Transfers contract ownership to a new principal

### For Identity Providers

- `verify-user`: Records verification of a user's identity by a provider

### For Users and Services

- `check-verification`: Checks if a user meets requirements for a specific service
- `get-user-verification-status`: Gets the current verification status of a user
- `get-service-requirements`: Gets the verification requirements for a service
- `get-provider-info`: Gets information about an identity provider

## Privacy and Security Considerations

The contract is designed with privacy in mind:
- No personal data is stored on-chain
- Only verification hashes and metadata are recorded
- Verification has expiration dates for security
- Each service can set its own trust requirements

## Implementation Example

Here's an example of how to use this contract:

1. Contract owner adds trusted identity providers:
```
(contract-call? .identity-verification add-provider "gov-id-provider" "Government ID Verification" u4)
(contract-call? .identity-verification add-provider "social-id-provider" "Social Identity Verification" u2)
```

2. Users register in the system:
```
(contract-call? .identity-verification register-user)
```

3. Identity providers verify users:
```
(contract-call? .identity-verification verify-user 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM "gov-id-provider" 0x8a9c5031c2c2bb1d161c9d0a36be331661eaa7ae2724fd9a5a59421119be052c u43200)
```

4. Services set verification requirements:
```
(contract-call? .identity-verification set-verification-requirements "financial-app" u3 (list "gov-id-provider") true true)
```

5. Services check user verification:
```
(contract-call? .identity-verification check-verification 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM "financial-app")
```

## Installation and Deployment

### Prerequisites
- Clarinet 2.15 or newer
- Stacks blockchain CLI (optional for mainnet deployment)

### Local Development
1. Initialize a new Clarinet project:
```bash
clarinet new decentralized-identity
cd decentralized-identity
```

2. Add the contract to your project:
```bash
# Place the contract code in contracts/identity-verification.clar
```

3. Update Clarinet.toml to include the contract:
```toml
[contracts.identity-verification]
path = "contracts/identity-verification.clar"
depends_on = []
```

4. Test the contract:
```bash
clarinet check
clarinet test
```

### Deployment to Testnet
```bash
stacks-cli deploy identity-verification.clar --testnet --fee 10000
```

## Extending the Contract

This contract can be extended in several ways:
- Adding multi-signature requirements for verification
- Implementing reputation systems for identity providers
- Adding support for verifiable credentials
- Implementing revocation mechanisms
- Adding support for selective disclosure of verification attributes

## License

This contract is provided as open source software under the [MIT License](https://opensource.org/licenses/MIT).
