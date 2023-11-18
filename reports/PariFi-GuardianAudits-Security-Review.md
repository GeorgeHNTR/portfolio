![](https://hackmd.io/_uploads/Hyv4GdsYn.jpg)

**Audit Firm**: Guardian Defender

**Client Firm**: PariFi

**Prepared By:** [@gogotheauditor](https://twitter.com/gogotheauditor)

**Delivery Date:** July 21st, 2023

<br />

PariFi engaged Guardian Defender to review the security of its Smart Contract system. During the contest period, **1** auditor reviewed the source code in scope. All findings have been recorded in the following report.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.

# Project Overview

| Project Name | PariFi                                                                                                                                          |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| Language     | Solidity                                                                                                                                        |
| Codebase     | https://github.com/GuardianAudits/PariFiDefenderAudit                                                                                           |
| Commit       | [835c98b66fecfe072750c400ba3ac1b0f6a07ebd](https://github.com/GuardianAudits/PariFiDefenderAudit/tree/835c98b66fecfe072750c400ba3ac1b0f6a07ebd) |

| Delivery Date     | July 21st, 2023 |
| ----------------- | --------------- |
| Audit Methodology | Manual Review   |

| Vulnerability Level   | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
| --------------------- | ----- | ------- | -------- | ------------ | ------------------ | -------- |
| [Critical](#Critical) | 8     | 8       | 0        | 0            | 0                  | 0        |
| [High](#High)         | 9     | 9       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)     | 7     | 7       | 0        | 0            | 0                  | 0        |
| [Low](#Low)           | 8     | 8       | 0        | 0            | 0                  | 0        |

# Audit Scope & Methodology

## Scope

| ID  | File                    | SHA-1 Hash                               |
| --- | ----------------------- | ---------------------------------------- |
| DFC | src/DataFabric.sol      | 0c7dac9a47fbf5526afdb2fd81c43f9f777e22df |
| FMR | src/FeeManager.sol      | a32a44376b3884793b312f6239c3318185b7e9a8 |
| MVT | src/MarketVault.sol     | 0c101c6d286c18adf2a0016bef1174aebb3cbc4a |
| ODS | src/OrderDS.sol         | 5af392c2c678b7a1f5516bca240db986e7eb9bc0 |
| OMR | src/OrderManager.sol    | d6543e7289a1553568063542085b487d96bd0e3b |
| PFF | src/PariFiForwarder.sol | 977d0cc719633bdf3acccd458f092f10e16ad2ae |
| PFE     |  src/PriceFeed.sol | f4e25a29b3489c4ef29ab648b521c8fa6903e103 |
| RBA | src/RBAC.sol            | 3e20b89665c246b82030aadce9a495e8f6c4d222 |

## Methodology

The auditing process pays special attention to the following considerations:

- Testing the smart contracts against both common and uncommon attack vectors.
- Assessing the codebase to ensure compliance with current best practices and industry standards.
- Ensuring contract logic meets the specifications and intentions of the client.
- Cross-referencing contract structure and implementation against similar smart contracts produced by industry leaders.
- Thorough line-by-line manual review of the entire codebase by community auditors.

## Vulnerability Classifications

| Vulnerability Level   | Classification                                                                               |
| --------------------- | -------------------------------------------------------------------------------------------- |
| [Critical](#Critical) | Easily exploitable by anyone, causing loss/manipulation of assets or data.                   |
| [High](#High)         | Arduously exploitable by a subset of addresses, causing loss/manipulation of assets or data. |
| [Medium](#Medium)     | Inherent risk of future exploits that may or may not impact the smart contract execution.    |
| [Low](#Low)           | Minor deviation from best practices.                                                         |

# Findings & Resolutions

| ID           | Title                                                                                                             | Category            | Severity | Status  |
| ------------ | ----------------------------------------------------------------------------------------------------------------- | ------------------- | -------- | ------- |
| [C-01](#C01) | Liquidity providers can sandwich traders when taking profits to avoid losses.                                     | Sandwich attack     | Critical | Pending |
| [C-02](#C02) | Fees should be updated before any other accounting is done.                                                       | Logic error         | Critical | Pending |
| [C-03](#C03) | Accrued borrowing fees are incorrectly calculated.                                                                | Math                | Critical | Pending |
| [C-04](#C04) | Users will be liquidated even if they should not be.                                                              | Logic error         | Critical | Pending |
| [C-05](#C05) | The price deviation formula is flawed.                                                                            | Math                | Critical | Pending |
| [C-06](#C06) | The borrow rate calculation formula is flawed.                                                                    | Math                | Critical | Pending |
| [C-07](#C07) | Users can be gas-griefed by relayers by setting an extremely high tx.gasprice.                                    | Gas griefing        | Critical | Pending |
| [C-08](#C08) | Native tokens will be stuck in the PariFi forwarder if the transaction execution was not successful.              | Logic error         | Critical | Pending |
| [H-01](#H01) | Profitable positions that become undercollateralized after a fee is applied cannot be liquidated.                 | Logic error         | High     | Pending |
| [H-02](#H02) | The order creation deadline is incorrectly checked.                                                               | Logic error         | High     | Pending |
| [H-03](#H03) | Gas compensation for relayer does not consider the initial transaction gas cost.                                  | Logic error         | High     | Pending |
| [H-04](#H04) | Relayers can be gas-griefed.                                                                                      | Gas griefing        | High     | Pending |
| [H-05](#H05) | Users can be gas-griefed by admin by setting an extremely high gasPremium.                                        | Gas griefing        | High     | Pending |
| [H-06](#H06) | Cumulative fees should be updated before configuration values are changed by the admin.                           | Logic error         | High     | Pending |
| [H-07](#H07) | Market utilization can become more than 100%, breaking protocol invariants.                                       | Validation          | High     | Pending |
| [H-08](#H08) | Updating order manager address poses a risk of completely breaking the protocol functionality.                    | Logic error         | High     | Pending |
| [H-09](#H09) | An adversary can DoS orders creation in the order manager.                                                        | DoS                 | High     | Pending |
| [M-01](#M01) | The protocol admin can break core functionalities resulting in the loss of funds.                                 | Centralisation risk | Medium   | Pending |
| [M-02](#M02) | Transaction EIP712 object is missing a deadline property.                                                         | Validation          | Medium   | Pending |
| [M-03](#M03) | Missing check for minimal collateral when decreasing a position.                                                  | Validation          | Medium   | Pending |
| [M-04](#M04) | Missing check for minimal collateral when the opening fee is reduced from provided collateral on order creation.  | Validation          | Medium   | Pending |
| [M-05](#M05) | Price deviation is always calculated against the secondary price.                                                 | Logic error         | Medium   | Pending |
| [M-06](#M06) | A position can be liquidated even if it is not at a loss.                                                         | Logic error         | Medium   | Pending |
| [M-07](#M07) | Admin can update properties of a market that they must not be allowed to change.                                  | Validation          | Medium   | Pending |
| [L-01](#L01) | Using an old Solidity version with known vulnerabilities that may affect the protocol.                            | Compiler            | Low      | Pending |
| [L-02](#L02) | Leverage validation check on order creation can be passed when multiple orders for the same position are pending. | Validation          | Low      | Pending |
| [L-03](#L03) | Potential underflow when calculating negative price deviation.                                                    | Validation          | Low      | Pending |
| [L-04](#L04) | Fees should be deducted after profit is added.                                                                    | Logic error         | Low      | Pending |
| [L-05](#L05) | Function implementation is incorrect as per specification.                                                        | Logic error         | Low      | Pending |
| [L-06](#L06) | Allowing any account to withdraw funds on your behalf is generally a bad practice.                                | Logic error         | Low      | Pending |
| [L-07](#L07) | Leverage should be set to type(uint256).max when collateral is 0.                                                 | Logic error         | Low      | Pending |
| [L-08](#L08) | Overridden access control methods with no new logic added.                                                        | Logic error         | Low      | Pending |

## <a id="Critical"></a>Critical

### <a id="C01"></a> C-01 Liquidity providers can sandwich traders when taking profits to avoid losses.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L178-L183
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L249
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L81-L102

#### Description:

When a trader wants to close their position on profit, the profit is taken from the liquidity providers who have staked deposit/collateral tokens in the corresponding MarketVault through `withdrawUserProfits`. This will automatically decrease the value of liquidity providers' shares, since the MarketVault is a standard ERC4626 vault that uses its balance of the underlying `asset` to determine the shares' value.

The problem is that a large liquidity provider can build a script to monitor the mempool and withdraw their shares every time when the keeper calls `settleOrder` for closing a position that is on profit. That way, the liquidity provider can avoid almost any losses.

Similarly, a large liquidity provider can arbitrage by depositing right before borrowing fees are distributed and withdraw after that, effectively stealing from long-term liquidity providers' earnings.

#### Recommendation:

Consider allowing liquidity providers to withdraw their shares only after a certain lock period from their deposit timestamp.

#### Resolution:

### <a id="C02"></a> C-02 Fees should be updated before any other accounting is done.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L249-L271
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L125

#### Description:

Currently, the borrowing fees are updated at the end of every operation that changes the `totalLongs` or `totalShorts` through `DataFabric.updateMarketData`.

The problem is that `updateCumulativeFees` accounts for the fees that were accrued in the time between this call and the previous update:

[src/DataFabric.sol#L125](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L125)

```solidity
uint256 timeDelta = block.timestamp - feeLastUpdatedTimestamp;
```

However, `updateCumulativeFees` is called at the end of `updateMarketData`, after the `totalLongs` or `totalShorts` were updated. Therefore, the calculated value in `getBaseBorrowRatePerSecond` and `getDynamicBorrowRatePerSecond` will not be correct for the `timeDelta` as they depend on the values of `totalLongs` or `totalShorts` BUT for the passed time (`timeDelta`), which should not consider the update made in `updateMarketData`.

#### Recommendation:

Consider calling `updateCumulativeFees` at the beginning of `updateMarketData` instead of the end.

#### Resolution:

### <a id="C03"></a> C-03 Accrued borrowing fees are incorrectly calculated.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L218
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L689-L696
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L160-L170
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L124-L141

#### Description:

Every time a user modifies their position in the OrderManager, the `userPosition.lastCumulativeFee` is updated and set to the cumulative value that is being tracked in `DataFabric.updateCumulativeFees`. Then, to calculate the difference between the `lastCumulativeFee` and the current `baseFeeCumulative` value, the `getAccruedBorrowFeesInMarket` function is used, which has the following logic:

```solidity
function getAccruedBorrowFeesInMarket(bytes32 _positionId) public view returns (uint256 accruedBorrowFees) {
    Position memory userPosition = openPositions[_positionId];
    uint256 currFeeCumulative = dataFabric.getCurrentFeeCumulative(userPosition.marketId, userPosition.isLong);
    uint256 accruedFeesCumulative = _getDiff(currFeeCumulative, userPosition.lastCumulativeFee);

    accruedBorrowFees =
        accruedFeesCumulative.mulWadDown(userPosition.positionSize * (block.timestamp - userPosition.lastTimestamp));
}
```

The `dataFabric.getCurrentFeeCumulative` itself returns the sum of the values stored for the base cumulative borrow fee and the dynamic one. They are calculated in `updateCumulativeFees` in the following way:

```solidity
baseFeeCumulative[marketId] = baseFeeCumulative[marketId] + timeDelta * getBaseBorrowRatePerSecond(marketId);

if (totalLongs[marketId] > totalShorts[marketId]) {
    dynamicLongFeeCumulative[marketId] =
        dynamicLongFeeCumulative[marketId] + timeDelta * getDynamicBorrowRatePerSecond(marketId);
} else {
    dynamicShortFeeCumulative[marketId] =
        dynamicShortFeeCumulative[marketId] + timeDelta * getDynamicBorrowRatePerSecond(marketId);
}
```

The `baseFeeCumulative`, `dynamicLongFeeCumulative`, and `dynamicShortFeeCumulative` are updated using the `timeDelta`. Therefore, the `_getDiff(currFeeCumulative, userPosition.lastCumulativeFee)` represents the actual change for the time period between the last (for the user position) and current update.

However, `accruedBorrowFees` is calculated by multiplying the `accruedFeesCumulative` by the position size (and dividing by the appropriate precision). The code then multiplies this value once more by the time difference, which will return a completely wrong result because the value of `accruedFeesCumulative` already considers the time passed (it's not per second).

#### Recommendation:

Consider removing the ` * (block.timestamp - userPosition.lastTimestamp)` from the calculation of `accruedBorrowFees`.

#### Resolution:

### <a id="C04"></a> C-04 Users will be liquidated even if they should not be.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L410-L420

#### Description:

The `market.liquidationThreshold` is used to determine whether a position is undercollateralized or not. This [check](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L410) is made in `_liquidatePosition` to determine whether or not the user position can be liquidated.

However, even if the check does not pass, the user position will still be liquidated because the function does not revert the transaction:

```solidity
if (lossInCollateral > liquidationThreshold) {
    IERC20(market.depositToken).safeTransfer(feeManager, lossInCollateral);
    IFeeManager(feeManager).distributeFees(market.depositToken);
}

// Transfer any remaining collateral to the user
uint256 userBalance;
if (userPosition.collateralAmount > lossInCollateral) {
    userBalance = userPosition.collateralAmount - lossInCollateral;
    IERC20(market.depositToken).safeTransfer(userPosition.userAddress, userBalance);
}

dataFabric.updateMarketData(market.marketId, userPosition.positionSize, userPosition.isLong, false);

delete openPositions[_positionId];
emit PositionLiquidated(_positionId, userBalance);
```

#### Recommendation:

Consider reverting `if (lossInCollateral <= liquidationThreshold)` instead of continuing with the logic.

#### Resolution:

### <a id="C05"></a> C-05 The price deviation formula is flawed.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L191

#### Description:

Consider the following example:

1. `deviationCoeff[marketId]` = `40000`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/script/PariFiDeployment.s.sol#L72)
2. `utilizationBps` = `8000`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L237)
3. `deviationConst[marketId]` = `0`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/script/PariFiDeployment.s.sol#L73)
4. `deviationPoints` = `40000 * 8000 * 8000 + 0` = `2560000000000` = `2.560e12`
5. This is clearly wrong as the `deviationPoints` exceed `1e12`, which will cause an
   [underflow error](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L151-L156) in
   the order manager.

#### Recommendation:

Consider whether the problem is that `40000` used for `deviationCoeff` in the deployment script is causing the issue, and if this is the case, add an upper limit check in the contract. If the problem lies within the `getPriceDeviation` function, mitigate it to return an appropriate value.

#### Resolution:

### <a id="C06"></a> C-06 The borrow rate calculation formula is flawed.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L198-L202

#### Description:

Consider the following example:

1. `baseCoeff[marketId]` = `125`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/script/PariFiDeployment.s.sol#L75)
2. `utilizationBps` = `8000 * 100 = 800000`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L237)
3. `baseConst[marketId]` = `100`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/script/PariFiDeployment.s.sol#L76)
4. `baseBorrowRate` = `(1e18 * 125 * 800000 * 800000 + 100) / 1e12` = `80000000000000000000` = `80e18`
5. This seems wrong as the `baseBorrowRate` exceeds `1e18`, while `1e18` is the equivalent of a 100% base borrow rate
   per year. Also, the `baseConst` of `100` is completely ineffective since when divided by `1e12`, it will round down
   to `0`.

#### Recommendation:

Consider whether the problem lies within the values used in the deployment script. If the problem is within the `getBaseBorrowRatePerSecond` function, mitigate it so that it returns an appropriate value.

Also, due to the time constraints, I cannot execute a proper test on the `getDynamicBorrowRatePerSecond`, but I assume it may be affected by a similar issue.

#### Resolution:

### <a id="C07"></a> C-07 Users can be gas-griefed by relayers by setting an extremely high tx.gasprice.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L104

#### Description:

The gas compensation is calculated in the following way in PariFiForwarder:

```solidity
uint256 gasCostInToken = ((gasSent - gasleft()) * tx.gasprice * gasPremium) / MAX_FEE;
```

The transaction object that a user signs, however, contains only the following properties:

```solidity
struct Transaction {
    address fromAddress;
    address toAddress;
    uint256 txValue;
    uint256 minGas;
    uint256 userNonce;
    bytes txData;
}
```

The problem is that there's no maximum `tx.gasprice` value signed by the user, which means that relayers can specify as high a `tx.gasprice` as they want in order to incur a loss for the user. Having in mind that the `execute` function has no access control, this can be easily abused by an attacker, especially if `type(uint256).max` allowance is given to the PariFiForwarder contract by users.

#### Recommendation:

Consider adding a `maxTxGasPrice` property to the `Transaction` struct.

#### Resolution:

### <a id="C08"></a> C-08 Native tokens will be stuck in the PariFi forwarder if the transaction execution was not successful.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L185

#### Description:

If the transaction in `PariFiForwarder.execute` was not successful, the function will return a status of `false` (+ the returned data) instead of reverting.

The problem is that the native tokens sent with the transaction to the `transaction.toAddress` will remain locked in the `PariFiForwarder` forever, as they are not returned to the relayer.

#### Recommendation:

Consider returning the `msg.value` back to the relayer if the transaction was not successful.

#### Resolution:

---

## <a id="High"></a> High

### <a id="H01"></a> H-01 Profitable positions that become undercollateralized after a fee is applied cannot be liquidated.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L395

#### Description:

Currently, you cannot liquidate a position that is on profit.

However, it's possible that a position has a slight profit but a large amount of liquidation, closing, and borrowing fees to pay, which could make it undercollateralized when the fees are deducted from the balance and profit.

Such a position will not be able to get liquidated though, which breaks an important protocol invariant.

#### Recommendation:

Consider removing the `isProfit` check in `_liquidatePosition` and then deducting the fees from the collateral, and just after that check for the liquidation threshold. Also, considering this case was not handled in the smart contract, I assume it was also not handled in the code for the keeper script, so it may be necessary to implement the needed logic there as well.

#### Resolution:

### <a id="H02"></a> H-02 The order creation deadline is incorrectly checked.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L526

#### Description:

The order deadline is meant to prevent orders from being executed after a certain timestamp that the creator picks.

However, the deadline check is currently performed in the `createOrder` instead of `_settleOrder`, which makes the deadline completely useless and risks users' funds.

#### Recommendation:

Consider moving the check on line 526 to the `_settleOrder` function.

#### Resolution:

### <a id="H03"></a> H-03 Gas compensation for relayer does not consider the initial transaction gas cost.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L170

#### Description:

The starting gas amount is determined by simply calling `gasleft()` at the beginning of the `execute` function.

However, this amount is not the actual start gas value that should be used as there are 21000 gas required initially to send any transaction plus the gas paid for each byte of the calldata (4 gas per 0 byte, 8 gas per non-zero byte).

#### Recommendation:

Consider adding 21k gas + the gas spent for bytes to the `gasSent` on [line 170](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L170).

#### Resolution:

### <a id="H04"></a> H-04 Relayers can be gas-griefed.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L139
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L112

#### Description:

There are currently 2 ways in which relayers can be gas-griefed for transactions execution in the `PariFiForwarder`:

1. When a transaction is submitted, a user can front-run the call from the relayers so that they revoke the approval for
   the ERC20 token that should be used for compensation.
2. A user can simply execute the transaction themselves, which will then make the relayer transaction revert because the
   nonce has been incremented.

Both attack vectors will result in the relayers losing funds because of the gas spent (before the revert) that will not be compensated.

#### Recommendation:

The second case may be solved by compensating the relayer for the gas spent so far in the transaction execution, even if it's not successful. However, a fix for the first case is not trivial.

#### Resolution:

### <a id="H05"></a> H-05 Users can be gas-griefed by admin by setting an extremely high gasPremium.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L244-L251
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L104

#### Description:

Similar to issue C-07, users can be gas-griefed because of the `gasPremium` property that is not included in the `Transaction` struct and therefore may be changed without the user's knowledge.

#### Recommendation:

Consider adding a `maxGasPremium` property to the `Transaction` struct.

#### Resolution:

### <a id="H06"></a> H-06 Cumulative fees should be updated before configuration values are changed by the admin.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L281-L343

#### Description:

There are multiple configuration variables like `dynamicCoeff`, `baseCoeff`, `deviationCoeff`, etc. that can be updated by the protocol admin. All these variables are used to determine the borrowing fees calculated regularly in `updateCumulativeFees`.

The problem is that `updateCumulativeFees` is not called before changing the configuration values, which means that the new values would apply even for the time between the `feeLastUpdatedTimestamp` and the current `block.timestamp`, which is not desired and will result in wrong accounting.

#### Recommendation:

Consider calling `updateCumulativeFees` at the beginning of all admin setter functions in DataFabric.

#### Resolution:

### <a id="H07"></a> H-07 Market utilization can become more than 100%, breaking protocol invariants.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L237
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L284-L287

#### Description:

The market utilization is currently calculated in the following way:

```solidity
uint256 totalOi = totalLongs[marketId] + totalShorts[marketId];
utilization = (totalOi * PRECISION_MULTIPLIER) / (2 * maximumOi[marketId]);
```

The problem is that the `maximumOi` value can be changed by the owner at any time and no input validation is made in the
corresponding setter:

```solidity
function setMaximumOi(bytes32 marketId, uint256 maxOi) external onlyAdmin {
    maximumOi[marketId] = maxOi;
    emit MaxOiUpdated(marketId, maxOi);
}
```

The admin can either by mistake or on purpose set a `maximumOi` which is less than `(totalLongs[marketId] + totalShorts[marketId]) / 2`, which would result in a utilization rate of over 100%, which should not be possible.

#### Recommendation:

Consider adding the following check in `setMaximumOi`:

```solidity
if (maxOi * 2 > (totalLongs[marketId] + totalShorts[marketId])) revert();
```

#### Resolution:

### <a id="H08"></a> H-08 Updating order manager address poses a risk of completely breaking the protocol functionality.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L291-L295

#### Description:

Currently, there's a setter function in the DataFabric contract that allows the protocol admin to set a new `orderManager` address.

Such a function should not be present, as the OrderManager contract completely relies on the DataFabric contract to account for the borrowing fees. If the `orderManager` contract address is changed, when the OrderManager tries to call `updateMarketData`, the transaction will revert because of the `onlyOrderManager` access control modifier, which will cause a DoS for all functionalities in the OrderManager, making all funds in the contract stuck.

#### Recommendation:

Consider removing the aforementioned setter function.

#### Resolution:

### <a id="H09"></a> H-09 An adversary can DoS orders creation in the order manager.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderDS.sol#L31
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L521-L523

#### Description:

In OrderDS.sol, it is mentioned how an order id should be calculated:

```solidity
bytes32 orderId; // keccak256 hash of marketId, marketAddress, userAddress, positionDirection, and sequence
```

However, nowhere in the actual OrderManager contract, this is computed nor verified. Instead, it is simply passed by the order creator.

This opens up an attack vector where an adversary can simply front-run the creation of orders that they don't want to be created by passing the same `orderId` that the victim is about to use. Then, the order can either be closed by the attacker releasing almost no loss or just be kept in case the attacker makes use of it.

This can repeat as many times as the attacker wants with any order that is about to get created effectively causing a DoS.

#### Recommendation:

Consider computing the `orderId` in the OrderManager contract as is done with the `positionId`s.

#### Resolution:

---

## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 The protocol admin can break core functionalities resulting in the loss of funds.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L871-L874
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L848-L852
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L114-L130
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L211-L213

#### Description:

The protocol is currently highly centralized as it relies on the PariFi team multisig to change important configuration values, as well as it allows them to `pause` important functionalities that will prevent users from withdrawing their funds or closing their position in the right time. It is also possible that the admin sets a blacklisted USDC (or any other token used as collateral) addresses as fee receivers to also block certain core functionalities.

#### Recommendation:

Consider improving the input validation in admin setter functions. Consider removing the `whenNotPaused` modifier from withdrawal functions like `MarketVault.withdraw`, `MarketVault.redeem`, `OrderManager.cancelPendingOrder`.

#### Resolution:

### <a id="M02"></a> M-02 Transaction EIP712 object is missing a deadline property.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L22-L29

#### Description:

Deadlines are usually used for any kind of user signatures to validate that the user still intends to execute the transaction at the time of actual execution.

Such a property is missing in the `Transaction` struct in `PariFiForwarder`, so if a user's transaction initially failed or was in the mempool for too long or simply executed at an unfavorable time for them in the future, it could lead to unexpected behavior and potentially loss of funds.

#### Recommendation:

Consider adding a `deadline` property to the `Transaction` struct.

#### Resolution:

### <a id="M03"></a> M-03 Missing check for minimal collateral when decreasing a position.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L442

#### Description:

Each market can have a specific amount of minimal collateral that should be deposited:

```solidity
struct Market {
    ...
    uint256 minCollateral; // Min. amount of collateral to deposit
    ...
}
```

However, the check for it is only applied when an order is created. A position's collateral can fall below that level if a user simply decreases their position with the corresponding amount of collateral tokens, which makes the check in deposit ineffective.

#### Recommendation:

Consider applying the `minCollateral` when a user decreases their position. Furthermore, a user's position's collateral is decreased when fees are charged, so it might be necessary to add such check even when a position's collateral is increased in case the fees are higher than the increase.

#### Resolution:

### <a id="M04"></a> M-04 Missing check for minimal collateral when the opening fee is reduced from provided collateral on order creation.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L442

#### Description:

As explained in the previous issue M-03, there's a minimum collateral amount that has to be deposited to create an order.

However, the check for it is made at the beginning of `_createOrderNewPosition`:

```solidity
function _createOrderNewPosition(Order memory _order, Market memory market, address partner) internal {
    // Collateral checks
    if (_order.collateralAmount < market.minCollateral || _order.collateralAmount == 0) {
        revert LibError.InvalidCollateralAmount();
    }
```

This is a problem as the `_order.collateralAmount` is then decreased if there's an opening fee set:

[src/OrderManager.sol#L453-L457](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L453-L457)

```solidity
uint256 fees = (market.openingFee * _order.orderSize) / MAX_FEE;
uint256 feeInCollateral =
    priceFeed.convertMarketToTokenSecondary(market.marketId, fees, market.depositToken);

_order.collateralAmount = _order.collateralAmount - feeInCollateral;
```

Therefore, a user can still create an order with too low collateral when fees are applied.

#### Recommendation:

Consider executing the `minCollateral` check after the fees are deducted.

#### Resolution:

### <a id="M05"></a> M-05 Price deviation is always calculated against the secondary price.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L134-L137

#### Description:

Currently, the price deviation between the two oracles used is calculated in the following way:

```solidity
(uint256 primaryPrice,) = priceFeed.getMarketPricePrimary(_marketId);
(uint256 secondaryPrice,) = priceFeed.getMarketPriceSecondary(_marketId);

uint256 diffBps = (_getDiff(primaryPrice, secondaryPrice) * PRECISION_MULTIPLIER) / secondaryPrice;
if (diffBps > availableMarkets[_marketId].maxPriceDeviation) {
    revert LibError.PriceOutOfRange(secondaryPrice, primaryPrice);
}
```

Let's consider the following two examples:

1. `primaryPrice` is 100e18 and `secondaryPrice` is 50e18
2. `secondaryPrice` is 50e18 and `secondaryPrice` is 100e18

In the first case, the price deviation would be calculated as `50e18 * 10_000 / 50e18 = 10000`.

In the second case, the price deviation would be calculated as `50e18 * 10_000 / 100e18 = 5000`.

As can be seen, even though the reported prices were the same but just in different order, the deviation would be calculated differently just because the `secondaryPrice` is used as the divisor.

This does not seem to be alright and may result in incorrect validation if the `maxPriceDeviation` is set e.g. to 5000 (note that the values here are quire unrealistic but just to show the example better).

#### Recommendation:

Consider using either always the smaller or always the bigger price to keep results consistent.

#### Resolution:

### <a id="M06"></a> M-06 A position can be liquidated even if it is not at a loss.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L643-L663

#### Description:

When determining if a user's position should be liquidated or not, it is checked whether it's profitable or not considering the result of `getProfitOrLossInUsd`:

```solidity
if (userPosition.isLong) {
    if (_price > userPosition.avgPrice) {
        // User position is profitable
        profitOrLoss = (_price - userPosition.avgPrice);
        isProfit = true;
    } else {
        // User position is at loss
        profitOrLoss = (userPosition.avgPrice - _price);
        isProfit = false;
    }
} else {
    if (_price > userPosition.avgPrice) {
        // User position is at loss
        profitOrLoss = (_price - userPosition.avgPrice);
        isProfit = false;
    } else {
        // User position is profitable
        profitOrLoss = (userPosition.avgPrice - _price);
        isProfit = true;
    }
}
```

As you can see, if the price has increased, a long position is considered to be on profit, while a short position is considered on loss. However, when the price has neither increased nor decreased from the average price, the result would be different - for a long position, if the price stays the same, it'd be considered NOT `onProfit`, while a short position would be considered `onProfit` as it will go to the `else` block.

This means that the check on [line 395](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L395) will pass, allowing the keeper to liquidate the user's position causing a loss for the trader by both taking a liquidation fee + loss of missed opportunity since their position was closed when it shouldn't have to.

#### Recommendation:

Consider changing [line 654](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L654) to the following:

```solidity
if (_price >= userPosition.avgPrice) {
```

#### Resolution:

### <a id="M07"></a> M-07 Admin can update properties of a market that they must not be allowed to change.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L831-L844

#### Description:

The protocol admin is allowed to change existing market properties via the `OrderManager.updateExistingMarket` function. Input validation is made to ensure that important properties like the `marketAddress` and the `depositToken` cannot be changed.

However, input validation for other properties is missing, which allows the admin to damage the protocol significantly.

#### Recommendation:

Consider not allowing the protocol admin to change critical market properties like `marketId` or setting other properties to values that would break the protocol (e.g., `maxLeverage` to 0).

#### Resolution:

---

## <a id="Low"></a> Low

### <a id="L01"></a> L-01 Using an old Solidity version with known vulnerabilities that may affect the protocol.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src

#### Description:

The project is currently using Solidity compiler version 0.8.11, which has known vulnerabilities that can affect the project in a future state of development.

#### Recommendation:

Consider switching to version 0.8.19.

#### Resolution:

### <a id="L02"></a> L-02 Leverage validation check on order creation can be passed when multiple orders for the same position are pending.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L495

#### Description:

The `_validateLeverage` validation made when creating an order to increase an existing position is done "to avoid locking tokens in case the order cannot be executed."

However, this validation is not sufficient, as a user can have created more than one order for modifying an existing position (e.g., first decrease and then increase a position, both pending orders), and the changes from the previous orders would not be taken into consideration.

#### Recommendation:

Consider whether this could become an issue. Even if the leverage validation is not made in `_createOrderModifyPosition` though, it's not true that tokens would become locked because a user can simply cancel their order if it can't get executed.

#### Resolution:

### <a id="L03"></a> L-03 Potential underflow when calculating negative price deviation.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L156

#### Description:

As shown in a previous critical risk issue, on [line 156](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L156) in the OrderManager, the subtraction may result in an underflow exception, which will break the whole contract's functionality.

#### Recommendation:

Consider whether this is the intended behavior or setting the `updatedPrice` to 0 would be more appropriate.

#### Resolution:

### <a id="L04"></a> L-04 Fees should be deducted after profit is added.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L156

#### Description:

When a user closes their position at a profit, they are first charged a fee, and then the left collateral plus the profit is sent to them.

However, if the fees are greater than the user's position's collateral, the transaction will revert, not allowing the user to close their position. Instead, they'd have to create an order to increase the position collateral, wait for the order to be executed, and then try to close their position again.

This process is unnecessary and makes users suffer a slight loss of the additional opening fee that would be applied for the additional order.

#### Recommendation:

Consider taking the fees from the `userBalance` on [line 248](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L248). If the fees are greater than the `userPosition.collateralAmount`, the `pnlInCollateral` should be decreased by the appropriate amount as well.

#### Resolution:

### <a id="L05"></a> L-05 Function implementation is incorrect as per specification.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L817

#### Description:

A NatSpec comment for the `toggleMarketStatus` function states that the function should revert if it is called with the same status.

However, the NatSpec does not match the actual code since the `status` parameter has been removed, and the function cannot set the `market.isLive` to the value that it currently has:

```solidity
/// @notice Toggle the market state to/from active/inactive
/// @dev Reverts in case same status is passed as before
/// @param _marketId Market ID of the market
/// @param _status True if market is set to be active, false to pause the market
function toggleMarketStatus(bytes32 _marketId) external onlyAdmin {
    Market storage market = availableMarkets[_marketId];
    if (market.marketAddress == address(0)) revert LibError.ZeroAddress();
    market.isLive = !market.isLive;
    emit MarketStatusToggled(_marketId);
}
```

#### Recommendation:

Consider either implementing the behavior described in the NatSpec comment or remove the comments.

#### Resolution:

### <a id="L06"></a> L-06 Allowing any account to withdraw funds on your behalf is generally a bad practice.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L787-L796

#### Description:

The `claimPartnerFees` function allows anyone to claim the partner fees a given partner has accrued.

It is generally a bad practice to allow untrusted parties to execute actions on behalf of users.

#### Recommendation:

Consider allowing only partners to withdraw their own fees.

#### Resolution:

### <a id="L07"></a> L-07 Leverage should be set to type(uint256).max when collateral is 0.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L625

#### Description:

When calculating a position's leverage, in case the loss is more than the provided collateral, the leverage is calculated as follows:

```solidity
leverage = (sizeInCollateral * PRECISION_MULTIPLIER);
```

Such a leverage should never be allowed, and therefore should always be greater than the `maxLeverage` when comparing in `_validateLeverage`:

```solidity
if (leverage > maxLeverage) revert LibError.OverLeveraged(maxLeverage, leverage);
```

However, to make sure that such a scenario never occurs, it would be a better approach to set the `leverage` to `type(uint256).max` and also makes sure that `maxLeverage` can never be set that high.

#### Recommendation:

Consider setting the `leverage` to `type(uint256).max` and also revert if the admin tries to set the `maxLeverage` of a market that high.

#### Resolution:

### <a id="L08"></a> L-08 Overridden access control methods with no new logic added.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L57-L65

#### Description:

The `grantRole` and `revokeRole` methods in the RBAC contract were overridden from the base AccessControl contract, but no changes have been made.

#### Recommendation:

Consider whether it was intended to add additional logic to the aforementioned methods. If not, remove the unnecessary overriding.

#### Resolution:

---

## Disclaimer

> This report is not, nor should be considered, an “endorsement” or “disapproval” of any particular project or team.
> This report is not, nor should be considered, an indication of the economics or value of any “product” or “asset”
> created by any team or project that contracts Guardian Defender to perform a security assessment. This report does not
> provide any warranty or guarantee regarding the absolute bug-free nature of the technology analyzed, nor do they
> provide any indication of the technologies proprietors, business, business model or legal compliance.
>
> This report should not be used in any way to make decisions around investment or involvement with any particular
> project. This report in no way provides investment advice, nor should be leveraged as investment advice of any sort.
> This report represents an extensive assessing process intending to help our customers increase the quality of their
> code while reducing the high level of risk presented by cryptographic tokens and blockchain technology.
>
> Blockchain technology and cryptographic assets present a high level of ongoing risk. Guardian Defender’s position is
> that each company and individual are responsible for their own due diligence and continuous security. Guardian
> Defender’s goal is to help reduce the attack vectors and the high level of variance associated with utilizing new and
> consistently changing technologies, and in no way claims any guarantee of security or functionality of the technology
> we agree to analyze.
>
> The assessment services provided by Guardian Defender is subject to dependencies and under continuing development. You
> agree that your access and/or use, including but not limited to any services, reports, and materials, will be at your
> sole risk on an as-is, where-is, and as-available basis. Cryptographic tokens are emergent technologies and carry with
> them high levels of technical risk and uncertainty. The assessment reports could include false positives, false
> negatives, and other unpredictable results. The services may access, and depend upon, multiple layers of
> third-parties.
>
> Notice that smart contracts deployed on the blockchain are not resistant from internal/external exploit. Notice that
> active smart contract owner privileges constitute an elevated impact to any smart contract’s safety and security.
> Therefore, Guardian Defender does not guarantee the explicit security of the audited smart contract, regardless of the
> verdict.

<br/>

---

## About

Guardian Defender is a competitive platform for teams of practicing auditors guided by Guardian Audits.

To learn more, visit https://lab.guardianaudits.com

To view the Guardian Defender audit portfolio, visit https://github.com/GuardianAudits/DefenderAudits![]

---
