# SAH Token-2022 Architecture

## Overview

SAH is the native utility token of the Sahwa ecosystem.

The token is built using the Solana SPL Token-2022 standard and serves as the foundation for ecosystem participation, HOLD mechanics, Digital Citizenship, and future DAO governance.

The architecture combines native Token-2022 functionality with custom on-chain validation logic implemented through Transfer Hooks and PDA-based state storage.

---

## Technology Stack

| Component | Technology |
|------------|------------|
| Blockchain | Solana |
| Framework | Anchor |
| Token Standard | SPL Token-2022 |
| Validation Layer | Transfer Hook |
| State Storage | PDA Accounts |
| HOLD Logic | Sahwa Hold Oracle |
| Governance Layer | DAO (Planned) |

---

## Core Components

### Token Mint

The SAH token is created as a Token-2022 mint.

The mint defines:

- Token supply
- Decimals
- Mint authority
- Transfer Hook extension

The mint serves as the primary asset of the Sahwa ecosystem.

---

### Token Accounts

Users interact with SAH through standard Token-2022 accounts.

Each account stores:

- Owner address
- Token balance
- Associated token metadata

Token accounts are validated during transfer operations.

---

### Transfer Hook

The Transfer Hook extension acts as the validation layer of the SAH ecosystem.

Every transfer request is routed through the hook before execution.

Responsibilities include:

- HOLD validation
- Citizenship validation
- Transfer restrictions
- Runtime compliance checks

---

### HOLD Oracle Integration

The Transfer Hook communicates with the Sahwa Hold Oracle.

Program ID:

```text
EkEa3SjvZk4D61tVtpcBXjdcoTt4A5TueKfSezwdDfnK
```

The Hold Oracle stores:

- frozen_amount
- unlock_timestamp
- citizenship_level
- nft_mint
- active status

This information is used during transfer validation.

---

## Runtime Flow

The following sequence occurs during every transfer:

```text
User
  │
  ▼
Token-2022 Transfer
  │
  ▼
Transfer Hook
  │
  ▼
Load UserHoldStatus PDA
  │
  ▼
Execute Validation Logic
  │
  ├── Reject
  │
  └── Approve
        │
        ▼
Token Transfer Executed
```

This ensures that validation occurs before token movement is finalized.

---

## Validation Logic

The current implementation validates multiple conditions.

### Active HOLD

Condition:

```text
is_active = true
current_time < unlock_timestamp
```

Result:

```text
Transfer Rejected
```

---

### Inactive HOLD

Condition:

```text
is_active = false
```

Result:

```text
Transfer Allowed
```

---

### Expired HOLD

Condition:

```text
current_time > unlock_timestamp
```

Result:

```text
Transfer Allowed
```

---

### Citizenship Restriction

Certain transfer operations may be restricted depending on Digital Citizenship requirements.

Result:

```text
Transfer Rejected
```

---

## Program Architecture

### Transfer Hook Program

Repository:

https://github.com/sahwa-sys/sahwa-transfer-hook

Program ID:

```text
z2nXKwowKgKcDYJFJUVZAUuz3p7fRFsjezRq8aLq78A
```

Responsibilities:

- Transfer validation
- Runtime execution
- Integration with Hold Oracle

---

### Hold Oracle Program

Repository:

https://github.com/sahwa-sys/sahwa-hold-oracle

Program ID:

```text
EkEa3SjvZk4D61tVtpcBXjdcoTt4A5TueKfSezwdDfnK
```

Responsibilities:

- HOLD storage
- Citizenship data storage
- Validation data provider

---

## Testing Status

Completed:

- Local deployment
- PDA initialization
- Transfer Hook integration
- HOLD validation
- Runtime validation
- End-to-End testing

Validation Results:

| Scenario | Result |
|-----------|-----------|
| Active HOLD | Reject |
| Inactive HOLD | Success |
| Expired HOLD | Success |
| Citizenship Restriction | Reject |

---

## Current Status

Status:

**SAH FINAL LOCAL E2E PASSED**

Development Phase:

**Local Testing Complete**

Next Phase:

**Devnet Deployment**

---

## Future Expansion

Planned future upgrades include:

- Digital Citizenship NFT integration
- DAO governance
- Voting mechanisms
- Advanced citizenship levels
- Reputation systems
- Ecosystem access controls

These features will be introduced incrementally as the Sahwa ecosystem evolves.