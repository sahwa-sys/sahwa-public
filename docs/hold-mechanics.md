# HOLD Mechanics

## Overview

HOLD is a core mechanism within the Sahwa ecosystem that allows users to voluntarily lock a portion of their SAH tokens for a predefined period of time.

Unlike traditional staking systems, HOLD is designed as a governance and participation mechanism rather than a yield-generating instrument.

The primary goals of HOLD are:

* Encourage long-term ecosystem participation
* Support Digital Citizenship requirements
* Enable governance participation
* Reduce short-term speculative behavior
* Establish commitment-based access to ecosystem privileges

⸻

## Core Concept

When a user activates HOLD, a specified amount of SAH tokens becomes restricted from transfer operations.

The restriction remains active until the HOLD period expires or the HOLD status is manually cleared according to ecosystem rules.

During an active HOLD period:

* Tokens remain in the user’s wallet
* Ownership is not transferred
* Tokens cannot be moved if they are covered by an active HOLD restriction
* The Transfer Hook validation layer enforces these restrictions

This architecture allows Sahwa to maintain user ownership while introducing protocol-level commitment mechanisms.

⸻

## User Hold Status Account

HOLD information is stored inside a dedicated PDA account called:

UserHoldStatus

Each user may have an associated status record containing:

Field	Description
user	Wallet address of the participant
citizenship_level	Current Digital Citizenship level
frozen_amount	Amount of SAH restricted by HOLD
unlock_timestamp	Timestamp when HOLD expires
nft_mint	Associated Digital Citizenship NFT
is_active	Current HOLD status

⸻

## Frozen Amount

The frozen_amount parameter defines how many SAH tokens are protected by HOLD restrictions.

Example:

User Balance:

10,000 SAH

Frozen Amount:

5,000 SAH

Result:

* First 5,000 SAH remain protected by HOLD
* Transfer validation checks whether the operation would violate HOLD restrictions
* Restricted tokens cannot be transferred while HOLD remains active

This mechanism creates measurable commitment without requiring token custody transfer.

⸻

## HOLD Lifecycle

Active HOLD

A HOLD becomes active when:

* A valid HOLD record exists
* is_active = true
* Current time is earlier than unlock_timestamp

Under these conditions:

* HOLD restrictions are enforced
* Protected balances remain locked
* Transfer Hook validation may reject transfers

Validation result:

Transfer Rejected

⸻

### Inactive HOLD

A HOLD is considered inactive when:

* is_active = false

Under these conditions:

* No transfer restrictions are applied
* User can freely transfer SAH

Validation result:

Transfer Allowed

⸻

### Expired HOLD

A HOLD is considered expired when:

Current Time > unlock_timestamp

Even if the record still exists, the HOLD period has ended.

Under these conditions:

* Restrictions are no longer enforced
* Protected balances become transferable
* User regains full token mobility

Validation result:

Transfer Allowed

⸻

## Runtime Validation Flow

Every SAH transfer passes through the Token-2022 Transfer Hook layer.

Validation sequence:

1. User initiates transfer.
2. Transfer Hook receives transfer request.
3. HOLD Oracle data is loaded.
4. UserHoldStatus PDA is evaluated.
5. HOLD state is determined.
6. Validation rules are executed.
7. Transfer is approved or rejected.

Simplified logic:

If HOLD is active:
    Reject transfer
If HOLD is inactive:
    Allow transfer
If HOLD is expired:
    Allow transfer

This validation occurs before token movement is finalized.

⸻

## Transfer Hook Integration

The HOLD system is enforced through Solana SPL Token-2022 Transfer Hooks.

This provides:

* Protocol-level enforcement
* Automatic validation
* Consistent runtime behavior
* Reduced reliance on off-chain checks

Transfer restrictions are verified directly during token movement.

⸻

## Relationship with Digital Citizenship

HOLD serves as a foundational component of the Digital Citizenship system.

Digital Citizenship may require users to maintain specific HOLD commitments.

Potential requirements include:

* Minimum HOLD balance
* Minimum HOLD duration
* Citizenship level thresholds
* Governance participation eligibility

This creates a direct relationship between commitment and ecosystem privileges.

⸻

## Ecosystem Benefits

The HOLD mechanism provides several advantages:

### Long-Term Alignment

Encourages participants to support ecosystem growth over longer time horizons.

### Governance Readiness

Creates a framework for future DAO participation and voting rights.

### Sybil Resistance

Increases the cost of creating disposable governance identities.

### Citizenship Qualification

Supports Digital Citizenship requirements and progression systems.

### Sustainable Growth

Aligns incentives between token holders and ecosystem development.

⸻

## Current Development Status

Implemented Components:

* HOLD Oracle Program
* UserHoldStatus PDA Storage
* SPL Token-2022 Integration
* Transfer Hook Validation
* Runtime HOLD Enforcement
* Local End-to-End Testing

Validation Scenarios Successfully Tested:

* Active HOLD → Reject
* Inactive HOLD → Success
* Expired HOLD → Success
* Citizenship Restriction → Reject

Development Status:

Local E2E Passed

Devnet Deployment Pending

⸻

## Future Expansion

Planned future enhancements may include:

* Multiple HOLD tiers
* Time-weighted citizenship levels
* DAO voting multipliers
* Reputation scoring
* Advanced governance privileges
* Ecosystem role qualification

These features will be introduced progressively as the Sahwa ecosystem evolves.