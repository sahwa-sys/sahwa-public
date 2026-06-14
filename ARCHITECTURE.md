Sahwa Architecture

Overview

Sahwa is being built as a Solana-based Web3 ecosystem powered by the SAH token.

The first infrastructure layer is based on Solana Token-2022 and introduces programmable transfer restrictions through SPL Transfer Hooks and HOLD mechanics.

The architecture consists of three primary components:

* sahwa-transfer-hook
* sahwa-hold-oracle
* sahwa-token-2022

Together these components enforce token freezing, digital citizenship validation, and future governance requirements directly at the token transfer layer.

⸻

High-Level Flow

User Wallet
↓
Token-2022 Transfer
↓
Transfer Hook
↓
Hold Oracle PDA
↓
Check frozen_amount
↓
Check citizenship_level
↓
Allow or reject transfer

⸻

Component 1 — sahwa-transfer-hook

Purpose:

Validate every SAH Token-2022 transfer.

Technology:

* Solana Program
* Anchor Framework
* SPL Transfer Hook Interface

Program ID:

z2nXKwowKgKcDYJFJUVZAUuz3p7fRFsjezRq8aLq78A

Responsibilities:

* Receive Transfer Hook execution requests
* Read Hold Oracle state
* Verify citizenship level
* Verify frozen balances
* Reject unauthorized transfers

Validation Logic:

if HOLD inactive:
    allow
if HOLD expired:
    allow
if citizenship_level == 0:
    reject
if source_balance < frozen_amount:
    reject
allow transfer

⸻

Component 2 — sahwa-hold-oracle

Purpose:

Store user HOLD information through Program Derived Accounts (PDAs).

Technology:

* Solana Program
* Anchor Framework

Program ID:

EkEa3SjvZk4D61tVtpcBXjdcoTt4A5TueKfSezwdDfnK

Main Account:

UserHoldStatus PDA

Structure:

pub struct UserHoldStatus {
    pub user: Pubkey,
    pub citizenship_level: u8,
    pub frozen_amount: u64,
    pub unlock_timestamp: i64,
    pub nft_mint: Pubkey,
    pub is_active: bool,
    pub bump: u8,
}

Supported Operations:

* initialize_config
* init_user_hold
* update_user_hold
* clear_user_hold
* set_nft_mint

⸻

Component 3 — sahwa-token-2022

Purpose:

Integration layer and deployment toolkit.

Technology:

* TypeScript
* Solana Web3.js
* SPL Token-2022

Responsibilities:

* Mint creation
* Account creation
* Token issuance
* Hook initialization
* HOLD configuration
* E2E validation

⸻

HOLD Mechanics

A user may voluntarily freeze part of their SAH balance.

Frozen tokens remain in the wallet but become unavailable for transfer.

Transfer restrictions are enforced through the Transfer Hook.

Benefits:

* Digital Citizenship eligibility
* Future DAO participation
* Future ecosystem privileges

⸻

Digital Citizenship Layer

Digital Citizenship is represented through on-chain status and future NFT credentials.

Current implementation:

citizenship_level > 0

Required for:

* Protected transfers
* Future DAO voting
* Governance participation
* Ecosystem privileges

⸻

Local Validation Status

Completed:

✓ Active HOLD rejection

✓ Inactive HOLD bypass

✓ Expired HOLD bypass

✓ Citizenship restriction

✓ Local end-to-end testing

Result:

SAH Token-2022 Devnet Launch Candidate v1

⸻

Next Stage

Devnet Deployment

Planned steps:

1. Deploy sahwa-transfer-hook
2. Deploy sahwa-hold-oracle
3. Create Devnet SAH mint
4. Initialize configurations
5. Create test users
6. Execute Devnet E2E validation
7. Prepare Testnet Demonstration Environment
