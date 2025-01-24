# Bitcoin DAO Smart Contract

A comprehensive Decentralized Autonomous Organization (DAO) implementation for Bitcoin with advanced governance features, built using Clarity smart contracts.

## Overview

This smart contract implements a full-featured DAO system that enables:

- Democratic governance through proposal creation and voting
- Treasury management with investment tracking
- Delegation system for voting power
- Emergency controls and administrative functions
- Returns distribution mechanism
- Flexible governance parameters
- Quorum-based decision making

## Contract Architecture

### Core Components

1. **Governance System**

   - Proposal creation and management
   - Voting mechanism
   - Quorum and majority thresholds
   - Timelock periods for execution

2. **Treasury Management**

   - Balance tracking
   - Investment returns distribution
   - Claim mechanism for returns

3. **Delegation System**

   - Vote delegation
   - Time-bound delegations
   - Voting power tracking

4. **Emergency Controls**
   - Emergency state management
   - Admin system
   - Safety mechanisms

### Data Structures

#### Members

```clarity
{
    voting-power: uint,
    joined-block: uint,
    total-contributed: uint,
    last-withdrawal: uint
}
```

#### Proposals

```clarity
{
    id: uint,
    proposer: principal,
    title: (string-ascii 100),
    description: (string-utf8 1000),
    amount: uint,
    target: principal,
    start-block: uint,
    end-block: uint,
    yes-votes: uint,
    no-votes: uint,
    status: (string-ascii 20),
    executed: bool
}
```

#### Return Pools

```clarity
{
    total-amount: uint,
    distributed-amount: uint,
    distribution-start: uint,
    distribution-end: uint,
    claims: (list 200 principal)
}
```

## Governance Parameters

| Parameter           | Description                  | Default Value            |
| ------------------- | ---------------------------- | ------------------------ |
| proposal-fee        | Fee to create a proposal     | 0.1 STX                  |
| min-proposal-amount | Minimum amount for proposals | 1 STX                    |
| max-proposal-amount | Maximum amount for proposals | 1000 STX                 |
| voting-delay        | Blocks before voting starts  | 100 blocks               |
| voting-period       | Duration of voting period    | 144 blocks (~1 day)      |
| timelock-period     | Delay before execution       | 72 blocks (~12 hours)    |
| quorum-threshold    | Required participation       | 50% (500 basis points)   |
| super-majority      | Required majority            | 66.7% (667 basis points) |

## Public Functions

### Proposal Management

#### `create-proposal`

Creates a new governance proposal.

```clarity
(create-proposal
    (title (string-ascii 100))
    (description (string-utf8 1000))
    (amount uint)
    (target principal))
```

### Delegation System

#### `delegate-votes`

Delegates voting power to another member.

```clarity
(delegate-votes
    (delegate-to principal)
    (amount uint)
    (expiry uint))
```

### Returns Distribution

#### `create-return-pool`

Creates a pool for distributing returns.

```clarity
(create-return-pool
    (proposal-id uint)
    (total-amount uint))
```

#### `claim-returns`

Claims returns from a distribution pool.

```clarity
(claim-returns
    (proposal-id uint))
```

### Emergency Controls

#### `set-emergency-state`

Sets the emergency state of the DAO.

```clarity
(set-emergency-state
    (state bool))
```

#### `add-emergency-admin`

Adds an emergency administrator.

```clarity
(add-emergency-admin
    (admin principal))
```

## Read-Only Functions

- `get-member-info`: Retrieves member information
- `get-proposal-by-id`: Gets proposal details
- `get-vote`: Retrieves vote information
- `get-delegation`: Gets delegation details
- `get-return-pool`: Retrieves return pool information
- `get-dao-parameters`: Gets current DAO parameters
- `get-treasury-balance`: Retrieves treasury balance

## Error Codes

| Code | Description         |
| ---- | ------------------- |
| u100 | Not authorized      |
| u101 | Already voted       |
| u102 | Proposal expired    |
| u103 | Insufficient funds  |
| u104 | Invalid amount      |
| u105 | Proposal not active |
| u106 | Quorum not reached  |
| u110 | No delegate         |
| u111 | Invalid delegate    |
| u112 | Emergency active    |
| u113 | Not emergency       |
| u114 | Invalid parameter   |
| u115 | No returns          |

## Security Features

1. **Access Control**

   - Role-based permissions
   - Emergency admin system
   - Delegation validation

2. **Parameter Validation**

   - Amount bounds checking
   - Timelock periods
   - Quorum requirements

3. **Safety Mechanisms**
   - Emergency state
   - Proposal thresholds
   - Treasury protection

## Best Practices for Integration

1. **Proposal Creation**

   - Provide clear titles and descriptions
   - Set appropriate amounts
   - Consider voting periods

2. **Voting**

   - Check proposal status
   - Verify voting power
   - Monitor quorum progress

3. **Returns Distribution**
   - Validate pool creation
   - Track claim periods
   - Monitor distribution progress

## Development and Testing

To interact with this contract:

1. Deploy the contract to a Bitcoin testnet
2. Initialize DAO parameters
3. Add initial members
4. Test proposal creation and voting
5. Verify return distribution
