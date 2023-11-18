# Metalabel V1_1 Solo Security Review

A security review of the [Metalabel](https://www.metalabel.xyz/) smart contract protocol was done by [Gogo](https://twitter.com/gogotheauditor). \
This audit report includes all the vulnerabilities, issues and code improvements found during the security review.

## Disclaimer

"Audits are a time, resource and expertise bound effort where trained experts evaluate smart
contracts using a combination of automated and manual techniques to find as many vulnerabilities
as possible. Audits can show the presence of vulnerabilities **but not their absence**."

\- Secureum

## Risk classification

| Severity           | Impact: High | Impact: Medium | Impact: Low |
| :----------------- | :----------: | :------------: | :---------: |
| Likelihood: High   |   Critical   |      High      |   Medium    |
| Likelihood: Medium |     High     |     Medium     |     Low     |
| Likelihood: Low    |    Medium    |      Low       |     Low     |

### Impact

- **High** - leads to a significant material loss of assets in the protocol or significantly harms a group of users.
- **Medium** - only a small amount of funds can be lost (such as leakage of value) or a core functionality of the protocol is affected.
- **Low** - can lead to any kind of unexpected behaviour with some of the protocol's functionalities that's not so critical.

### Likelihood

- **High** - attack path is possible with reasonable assumptions that mimic on-chain conditions and the cost of the attack is relatively low to the amount of funds that can be stolen or lost.
- **Medium** - only conditionally incentivized attack vector, but still relatively likely.
- **Low** - has too many or too unlikely assumptions or requires a huge stake by the attacker with little or no incentive.

### Actions required by severity level

- **Critical** - client **must** fix the issue.
- **High** - client **must** fix the issue.
- **Medium** - client **should** fix the issue.
- **Low** - client **could** fix the issue.

## Executive summary

### Overview

|               |                                                                                                                                                   |
| :------------ | :------------------------------------------------------------------------------------------------------------------------------------------------ |
| Project Name  | Metalabel                                                                                                                                         |
| Repository    | https://github.com/metalabel/metalabel-contracts-v1_1                                                                                             |
| Commit hash   | [fb04291dfdf7114bbec12ef5ec30b4135eac4878](https://github.com/metalabel/metalabel-contracts-v1_1/commit/fb04291dfdf7114bbec12ef5ec30b4135eac4878) |
| Documentation | [link](https://metalabel.notion.site/Metalabel-Protocol-Walkthrough-V1-1-Addendum-3e18e13a1ccc48d68e777956a20279c6)                               |
| Methods       | Manual review                                                                                                                                     |
|               |

### Issues found

| Severity      | Count |
| :------------ | ----: |
| Critical risk |     0 |
| High risk     |     0 |
| Medium risk   |     5 |
| Low risk      |     7 |
| Informational |     6 |

### Scope

| File                                                                                                                                                                         | SLOC |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---: |
| _Contracts (5)_                                                                                                                                                              |
| [contracts/Memberships.sol](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Memberships.sol)                   |  261 |
| [contracts/MembershipsFactory.sol](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/MembershipsFactory.sol)     |   51 |
| [contracts/ControllerV1.sol](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/ControllerV1.sol)                 |  172 |
| [contracts/engines/DropEngineV2.sol](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol) |  299 |
| [contracts/RevenueModuleFactory.sol](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/RevenueModuleFactory.sol) |   80 |
| _Abstracts (0)_                                                                                                                                                              |
| _Interfaces (0)_                                                                                                                                                             |
| _Total (5)_                                                                                                                                                                  |  863 |

# Findings

## Medium severity

### [M-1] Revenue can be forwarded to the null address when price = 0 and revenueRecipient == address(0), but priceDecayPerDay > 0

#### **Context**

- [contracts/engines/DropEngineV2.sol#L284-L290](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L284-L290)
- [contracts/engines/DropEngineV2.sol#L340-L345](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L340-L345)

#### **Description**

If a drop price is configured to decay to `0` and at the same time `decayStopTimestamp` is > `block.timestamp` (e.g. user want the initial price to be 5 ETH and after 5 days to go down to `0`), the `revenueRecipient` is restricted to be `address(0)`. This will lead to transferring all the revenue generated from a given drop to the [null address](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L284-L290).

#### **Recommended Mitigation Steps**

Modify the input validation in [DropEngineV2.sol](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L340-L345) so users can configured sequences with price decaying to `0` and a `revenueRecipient != address(0)`. \
You can add the following check:

```diff
-       // Ensure that if a price is set, a recipient is set, and vice versa
-       if (
-           (dropData.price == 0) != (dropData.revenueRecipient == address(0))
-       ) {
-           revert InvalidPriceOrRecipient();
-       }

+       // Ensure that if a price or decaying price is set, a recipient is set, and vice versa
+       if (
+           (dropData.price == 0 && dropData.priceDecayPerDay == 0) !=
+           (dropData.revenueRecipient == address(0))
+       ) {
+           revert InvalidPriceOrRecipient();
+       }
```

### [M-2] Insufficient input validation - admin can transfer non-existent membership tokens from address(0) in `Memberships.sol`

- [contracts/Memberships.sol#L374-L381](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Memberships.sol#L374-L381)

#### **Description**

Admin should not be able to transfer membership from the null address since it will result in DoS of the `mint`-ing functionality when an `id >= minted` is passed to `Memberships.adminTransferFrom`.

#### **Recommended Mitigation Steps**

Add the following check in `Memberships.adminTransferFrom`:

```diff

        if (to == address(0)) revert InvalidTransfer();
+       if (from == address(0)) revert InvalidTransfer();
        if (balanceOf(to) != 0) revert InvalidTransfer();
        if (from != _tokenData[id].owner) revert InvalidTransfer();
```

### [M-3] Revenue recipient can cause DoS of the DropEngineV2.mint

#### **Context**

- [contracts/engines/DropEngineV2.sol#L284-L290](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L284-L290)

#### **Description**

If for some reason the `dropData.revenueRecipient` is set to an account that can not receive funds (e.g. a contract that doesn't have a payable `fallback` or `receive` function) nobody will be able to mint records from the given drop.

#### **Recommended Mitigation Steps**

Follow the [withdrawal pattern](https://docs.soliditylang.org/en/v0.8.18/common-patterns.html#withdrawal-from-contracts) for revenue collection. This is done for fee collection by the owner.

```diff
-       // Forward ETH to the revenue recipient
-       if (amountToForward > 0) {
-           (bool success, ) = drop.revenueRecipient.call{
-               value: amountToForward
-           }("");
-           if (!success) revert CouldNotTransferEth();
-       }

+       if (amountToForward > 0) {
+           revenueToReceive[drop.revenueRecipient] += amountToForward;
+       }
```

---

**NOTE**

The above will require changes to be made in the `DropEngineV2.transferFeesToOwner` function as well.

---

### [M-4] Single-step ownership transfer can potentially lead to loosing all primary sales fees in DropEngineV2

- [contracts/engines/DropEngineV2.sol](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol)

#### **Description**

Usually this would be considered a low severity issue, but in the case of the new `DropEngineV2` contract the owner is the only account that has access to the funds stored in the contract (the collected primary sales fees) so it is critical to ensure (on smart contracts level) it can't be mistakenly set to a non-existent ethereum account.

#### **Recommended Mitigation Steps**

Implement two-step ownership transfer for the `DropEngineV2` contract. Similar is done for the node ownership transfers in `NodeRegistry`.

### [M-5] Upper bound for `primarySaleFeeBps` is too high

#### **Context**

- [contracts/engines/DropEngineV2.sol#L186-L192](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L186-L192)

#### **Description**

Currently there's is a centralization issue with the privileged `owner` of `DropEngineV2`. As stated in a comment in `DropEngineV2.configureSequence`, you `don't allow the caller to control the primary sale fee`, instead it is only `set by the contract owner`. That means the `owner` decides how much to take from each mint order price. The owner can set the `primarySaleFeeBps` to 10000 (the max bps) and therefore take all of the revenue from each next drop leaving the `dropData.revenueRecipient` with nothing.

#### **Recommended Mitigation Steps**

Consider adding a constant (e.g. `MAX_PRIMARY_SALE_FEE_BPS`) that restricts the `owner` of the engine to set the primary sale fee to more than a given percent:

```diff
+   uint16 constant MAX_PRIMARY_SALE_FEE_BPS = 3000; // 30%

    /// @notice Set the primary sale fee for all drops configured on this
    /// engine. Only callable by owner
    function setPrimarySaleFeeBps(uint16 fee) external onlyOwner {
-       if (fee > 10000) revert InvalidPrimarySaleFee();
+       if (fee > MAX_PRIMARY_SALE_FEE_BPS) revert InvalidPrimarySaleFee();
        primarySaleFeeBps = fee;
        emit PrimarySaleFeeSet(fee);
    }
```

## Low severity

### [L-1] Insufficient input validation - `decayStopTimestamp` should be before `sealedAfterTimestamp`

- [contracts/engines/DropEngineV2.sol#L347-L353](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L347-L353)
- [contracts/Collection.sol#L212-L221](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Collection.sol#L212-L221)

#### **Description**

The `_sequence.sealedAfterTimestamp` and `_sequence.sealedBeforeTimestamp` variables in `Collection.configureSequence` define a timebound sequence. When a drop has a decaying price the decay stop timestamp should logically be before the end of the records minting window. Otherwise the `dropData.price` will never be reached in the given window because of the checks in [`Collection._validateSequence`](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Collection.sol#L290-L297).

#### **Recommended Mitigation Steps**

Verify that decayStopTimestamp is <= sealedAfterTimestamp in `Collection.configureSequence`.

### [L-2] External calls to 2 arbitrary addresses in `DropEngineV2.mint`

#### **Context**

- [contracts/engines/DropEngineV2.sol#L280](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L280)
- [contracts/engines/DropEngineV2.sol#L286-L288](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L286-L288)

#### **Description**

There are 2 external calls being made in the `mint` function of `DropEngineV2` and both of the are to arbitrary / user-controlled addresses. Although the CEI pattern is observed correctly in this function, it is also a good practice to implement the withdrawal pattern as well as to use a re-entrancy guard.

#### **Recommendations**

Consider using the `nonReentrant` modifier from [OZ's library](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol) in the `mint` function in `DropEngineV2`. \
Implement the withdrawal pattern for withdrawing the drop revenue.

### [L-3] The drop revenue recipient can steal gas from minters

#### **Context**

- [contracts/engines/DropEngineV2.sol#L286-L288](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L286-L288)

#### **Description**

The following external low-level call is made in `DropEngineV2.mint`:

```
        (bool success, ) = drop.revenueRecipient.call{
            value: amountToForward
        }("");
```

The whole transaction gas is forwarded to this call which means that the `drop.revenueRecipient` can use it for any purpose that it finds incentive for. Each minter will have to pay an arbitrary (eventually a huge) amount of gas for the logic that the `revenueRecipient` may implement if it is a smart contract account with a payable `receive` or `fallback` function.

#### **Recommendations**

Follow the [withdrawal pattern](https://docs.soliditylang.org/en/v0.8.18/common-patterns.html#withdrawal-from-contracts) for revenue collection. This is done for fee collection by the owner.

```diff
-       // Forward ETH to the revenue recipient
-       if (amountToForward > 0) {
-           (bool success, ) = drop.revenueRecipient.call{
-               value: amountToForward
-           }("");
-           if (!success) revert CouldNotTransferEth();
-       }

+       if (amountToForward > 0) {
+           revenueToReceive[drop.revenueRecipient] += amountToForward;
+       }
```

---

**NOTE**

The above will require changes to be made in the `DropEngineV2.transferFeesToOwner` function as well.

---

### [L-4] `primarySaleFeeBps` is overriden by the engine which might not be expected by users

#### **Context**

- [contracts/engines/DropEngineV2.sol#L367-L379](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L367-L379)

#### **Description**

The requirements in `DropEngineV2` are that the `owner` of the contract decides what the `primarySaleFeeBps` should be for each drop. This is enforced in the code in `DropEngineV2.configureSequence` by overriding the value that the user has passed for this parameter. It could be dangerous if the user is not aware of this behaviour.

#### **Recommendations**

As stated in a comment by the dev the better approach would be to `require the caller to pass in the current fee to assert the caller is aware of this behavior`

### [L-5] `Memberships.approve` should be not allowed as `Memberships.transferFrom` is not as well

#### **Context**

- [contracts/Memberships.sol#L363-L370](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Memberships.sol#L363-L370)

#### **Description**

ERC721 token transfers in `Memberships.sol` are not allowed by simply overriding the `transferFrom` function from solmate and reverting on each call. The same should be done for `Memberships.approve` since there is no point of approving tokens that can not be transferred later. This might prevent potential future problems.

#### **Recommendations**

Add the following code in `Memberships.sol`:

```diff
+   /// @notice Approval is not allowed on this token.
+   function approve(address, uint256) public pure override {
+       revert ApprovalNotAllowed();
+   }

+   /// @notice Approval is not allowed on this token.
+   function setApprovalForAll(address, bool) public pure override {
+       revert ApprovalNotAllowed();
+   }
```

### [L-6] The drop revenue recipient can mint infinite amount of records in a single transaction for free if primarySaleFeeBps = 0

#### **Context**

- [contracts/engines/DropEngineV2.sol#L284-L290](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L284-L290)

#### **Description**

The current design of minting records in a drop allows the revenue recipient to mint whatever amount of records they want practically for free. Since the `dropData.revenueRecipient` receives all funds for the sales (when `primarySaleFeeBps = 0`) they can simply mint again and again which will inflate the total supply of records. This may not be expected by users.

#### **Recommendations**

Not sure how this could be prevented or if it should be prevented at all, but I thought it is worth mentioning.

### [L-7] `priceDecayPerDay` can be extremely large compared to the constant `price`

#### **Context**

- [contracts/engines/DropEngineV2.sol#L347-L362](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L347-L362)

#### **Description**

Currently there is not relationship between the `dropData.priceDecayPerDay`, `dropData.decayStopTimestamp` and `dropData.price`. A user can set the price to decay for example from `100 ETH` to `0.00001 ETH` in just 1 day which doesn't seem to be a good UX design.

#### **Recommendations**

Consider implementing some restrictions for `priceDecayPerDay` and `decayStopTimestamp`.

## Informational

### [I-1] Inline-assembly can be used to make copying array cleaner

#### **Context**

- [contracts/RevenueModuleFactory.sol#L145-L153](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/RevenueModuleFactory.sol#L145-L153)

#### **Description**

Copying the `config.waterfallRecipients` to memory in RevenueModuleFactory.deployRevenueModule can be done in more efficient way using inline-assembly.

#### **Recommendations**

Consider the 2 following recommendations:

1. Saves ~2.5k gas

```
        address[] memory recipients;
        assembly {
            // Set recipients.length = config.waterfallRecipients.length + 1.
            mstore(recipients, add(calldataload(0x124), 1))
            // Copy all elements from calldata to the recipients memory array.
            calldatacopy(add(recipients, 0x20), 0x144, mul(calldataload(0x124), 0x20))
            // Add the split address at the end of the waterfall recipients array.
            mstore(add(add(recipients, 0x20), mul(calldataload(0x124), 0x20)), split)
        }
```

2. Saves ~2k gas

```
        address[] memory recipients = config.waterfallRecipients;
        assembly {
            // Add the split address at the end of the memory recipients array.
            mstore(add(add(recipients, 0x20), mul(mload(recipients), 0x20)), split)
            // Set recipients.length = config.waterfallRecipients.length + 1.
            mstore(recipients, add(mload(recipients), 1))
        }
```

### [I-2] Less expensive input validation should come first

#### **Context**

- [contracts/Memberships.sol#L328](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Memberships.sol#L328)

#### **Description**

When you have multiple input validation checks the less expensive ones should come before the more expensive so in case of a revert less gas is wasted. In `Memberships.mintMemberships` there are 2 checks in the for loop - whether the result from `MerkleProofLib.verify` is true and whether the `to` address has no membership tokens already. The `MerkleProofLib.verify` is expensive operation so the `balanceOf(mints[i].to) > 0` should happen before that.

#### **Recommendations**

Change code to:

```diff
+           // enforce at-most-one membership per address
+           if (balanceOf(mints[i].to) > 0) revert InvalidMint();
            bool isValid = MerkleProofLib.verify(
                mints[i].proof,
                membershipListRoot,
                keccak256(abi.encodePacked(mints[i].to, mints[i].sequenceId))
            );
-           // enforce at-most-one membership per address
-           if (!isValid || balanceOf(mints[i].to) > 0) revert InvalidMint();
+           if (!isValid) revert InvalidMint();
```

### [I-3] Use `string.concat` instead of `abi.encodePacked` since 0.8.12

#### **Context**

- [contracts/Memberships.sol#L420-L462](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Memberships.sol#L420-L462)
- [contracts/engines/DropEngineV2.sol#L397-L523](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L397-L523)
- [contracts/engines/DropEngine.sol#L266-L272](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngine.sol#L266-L272)

#### **Description**

Since solidity version 0.8.12 was released the new method `string.concat` is suggested to be used instead of `abi.encodePacked` for concatenating strings. This is done on multiple places around the current codebase especially in DropEngineV2.

#### **Recommendations**

Use `string.concat` instead of `abi.encodePacked` for concatenating strings.

### [I-4] Implement `batchMintRecords` in `Collection.sol` instead of iterating in `DropEngineV2.sol`

#### **Context**

- [contracts/engines/DropEngineV2.sol#L249-L261](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L249-L261)

#### **Description**

Right now, when someone wants to mint multiple records from a given drop they will call `DropEngineV2.mint` which iterates a `count` number of times and calls `collection.mintRecord` each time. In `Collection.mintRecord` we can see that on each time the function is called there is a verification procedure for the passed sequence and the `msg.sender` (the engine). This incurs overhead when multiple records are minted at once via `DropEngineV2.mint` because `_validateSequence` is called each time.

#### **Recommendations**

Consider adding a new function (e.g. `batchMintRecord`) in the `Collection.sol` contract that will mint multiple records at once and will `_validateSequence` only once.

---

**NOTE**

Only the checks on lines [286](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Collection.sol#L286) and [291-295](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Collection.sol#L291-L295) should be performed once per `batchMintRecord`. The [remaining supply check](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/Collection.sol#L300) should consider the minting amount (the `count`).

---

### [I-5] Timestamp variables should be of type `uint40` instead of `uint64`

#### **Context**

- [contracts/engines/DropEngineV2.sol#L59](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L59)

#### **Description**

The max value for variables of type `uint40` is 2\*\*40-1 or in the context of a timestamp this would be after the year of 36000. Currently variables that hold a timestamp value such as `sealedBeforeTimestamp`, `sealedAfterTimestamp` and `decayStopTimestamp` are of type `uint64`. Using `uint40` instead of `uint64` is more appropriate and might help with [structs packing](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/engines/DropEngineV2.sol#L60-L66).

#### **Recommendations**

Use `uint40` for `sealedBeforeTimestamp`, `sealedAfterTimestamp` and `decayStopTimestamp` instead of `uint64`.

### [I-6] A single struct can be reused for `CreateMembershipsConfig` and `CreateCollectionConfig`

#### **Context**

- [contracts/MembershipsFactory.sol#L42-L50](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/MembershipsFactory.sol#L42-L50)

#### **Description**

The `CreateMembershipsConfig` and `CreateCollectionConfig` structs are almost identical so a single struct can be used instead of 2 separate. Even the [comment](https://github.com/metalabel/metalabel-contracts-v1_1/blob/fb04291dfdf7114bbec12ef5ec30b4135eac4878/contracts/MembershipsFactory.sol#L42) for `CreateMembershipsConfig` is copied from `CreateCollectionConfig`.

#### **Recommendations**

Consider using a single struct like for example `ResourceConfig` instead of `CreateMembershipsConfig` and `CreateCollectionConfig`.
