# Sahwa Corporation

Sahwa Corporation is a Web3 ecosystem focused on building a tokenized digital corporation with SAH as the core ecosystem asset.

The project combines Solana Token-2022 infrastructure, HOLD mechanics, digital citizenship, and future DAO-based governance.

## Current Development Status

The first technical milestone has been completed locally:

**SAH Token-2022 HOLD MVP — Local E2E Passed**

The current prototype includes:

- SAH Token-2022 mint architecture
- SPL Token-2022 Transfer Hook integration
- HOLD Oracle PDA storage
- Active HOLD transfer restriction
- Inactive HOLD bypass
- Expired HOLD bypass
- Citizenship-level transfer restriction
- Local end-to-end validation

## Core Architecture

```text
User Wallet
↓
Token-2022 Transfer
↓
Transfer Hook
↓
Hold Oracle PDA
↓
Check frozen_amount / citizenship_level
↓
Allow or reject transfer
```