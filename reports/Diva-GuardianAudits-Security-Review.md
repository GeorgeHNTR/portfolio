
__Audit Firm__: Solidity Lab

__Client Firm__: DIVA Protocol

__Prepared By__: [gogo](https://twitter.com/gogotheauditor), [kodyvim](https://twitter.com/kodyvim_), [Santipu_](https://twitter.com/MrCaesarDev), [zaskoh](https://twitter.com/0xzaskoh)

__Delivery Date:__ April 7th, 2023

<br />

__DIVA Protocol__ engaged Solidity Lab to review the security of its Smart Contract system. From __March 16th__ to __April 7th__, a team of __4__ auditors reviewed the source code in scope. All findings have been recorded in the following report.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | RaisinLabs                                                                                                                              |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| Language     | Solidity                                                                                                                                |
| Codebase     | https://github.com/GuardianAudits/DivaAudit                                                                                             |
| Commit       | [5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e](https://github.com/GuardianAudits/DivaAudit/commit/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e) |


| Delivery Date     | April 7th                      |
| ----------------- | ------------------------------ |
| Audit Methodology | Static Analysis, Manual Review |


| Vulnerability Level             | Total | Resolved | Declined | Acknowledged | Partially Resolved | Resolved |
| ------------------------------- | ----- | ------- | -------- | ------------ | ------------------ | -------- |
| [Critical](#Critical)           | 0     | 0       | 0        | 0            | 0                  | 0        |
| [High](#High)                   | 2     | 2       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)               | 4     | 4       | 0        | 0            | 0                  | 0        |
| [Low](#Low)                     | 11    | 11       | 0        | 0            | 0                  | 0        |
| [Informational](#Informational) | 11    | 11       | 0        | 0            | 0                  | 0        |


# Audit Scope & Methodology

## Scope

| File                                                               | nSLOC    | Complex. Score |
| ------------------------------------------------------------------ | -------- | -------------- |
| oracles/contracts/DIVAOracleTellor.sol                             | 480      | 344            |
| diva-contracts/contracts/DIVADevelopmentFund.sol                   | 145      | 123            |
| diva-contracts/contracts/DIVAOwnershipMain.sol                     | 128      | 86             |
| diva-contracts/contracts/DIVAOwnershipSecondary.sol                | 114      | 35             |
| diva-contracts/contracts/DIVAToken.sol                             | 12       | 5              |
| diva-contracts/contracts/Diamond.sol                               | 118      | 117            |
| diva-contracts/contracts/PermissionedPositionToken.sol             | 58       | 41             |
| diva-contracts/contracts/PositionToken.sol                         | 39       | 28             |
| diva-contracts/contracts/PositionTokenFactory.sol                  | 57       | 44             |
| diva-contracts/contracts/UsingTellor.sol                           | 14       | 6              |
| diva-contracts/contracts/facets/ClaimFacet.sol                     | 69       | 44             |
| diva-contracts/contracts/facets/DiamondCutFacet.sol                | 10       | 7              |
| diva-contracts/contracts/facets/DiamondLoupeFacet.sol              | 43       | 36             |
| diva-contracts/contracts/facets/EIP712AddFacet.sol                 | 47       | 22             |
| diva-contracts/contracts/facets/EIP712CancelFacet.sol              | 77       | 58             |
| diva-contracts/contracts/facets/EIP712CreateFacet.sol              | 119      | 33             |
| diva-contracts/contracts/facets/EIP712RemoveFacet.sol              | 47       | 22             |
| diva-contracts/contracts/facets/GetterFacet.sol                    | 135      | 79             |
| diva-contracts/contracts/facets/GovernanceFacet.sol                | 253      | 105            |
| diva-contracts/contracts/facets/LiquidityFacet.sol                 | 89       | 47             |
| diva-contracts/contracts/facets/PoolFacet.sol                      | 35       | 33             |
| diva-contracts/contracts/facets/SettlementFacet.sol                | 328      | 131            |
| diva-contracts/contracts/facets/TipFacet.sol                       | 42       | 29             |
| diva-contracts/contracts/libraries/LibDIVA.sol                     | 499      | 196            |
| diva-contracts/contracts/libraries/LibDIVAStorage.sol              | 86       | 16             |
| diva-contracts/contracts/libraries/LibDiamond.sol                  | 240      | 116            |
| diva-contracts/contracts/libraries/LibDiamondStorage.sol           | 26       | 6              |
| diva-contracts/contracts/libraries/LibEIP712.sol                   | 558      | 733            |
| diva-contracts/contracts/libraries/LibEIP712Storage.sol            | 17       | 6              |
| diva-contracts/contracts/libraries/LibOwnership.sol                | 21       | 8              |
| diva-contracts/contracts/libraries/SafeDecimalMath.sol             | 14       | 7              |
| diva-contracts/contracts/interfaces/IClaim.sol                     | 25       | 9              |
| diva-contracts/contracts/interfaces/IDIVADevelopmentFund.sol       | 19       | 30             |
| diva-contracts/contracts/interfaces/IDIVAOwnershipMain.sol         | 27       | 33             |
| diva-contracts/contracts/interfaces/IDIVAOwnershipSecondary.sol    | 10       | 11             |
| diva-contracts/contracts/interfaces/IDIVAOwnershipShared.sol       | 3        | 3              |
| diva-contracts/contracts/interfaces/IDiamondCut.sol                | 14       | 3              |
| diva-contracts/contracts/interfaces/IDiamondLoupe.sol              | 7        | 9              |
| diva-contracts/contracts/interfaces/IEIP712Add.sol                 | 15       | 5              |
| diva-contracts/contracts/interfaces/IEIP712Cancel.sol              | 5        | 13             |
| diva-contracts/contracts/interfaces/IEIP712Create.sol              | 15       | 5              |
| diva-contracts/contracts/interfaces/IEIP712Remove.sol              | 15       | 5              |
| diva-contracts/contracts/interfaces/IERC165.sol                    | 3        | 3              |
| diva-contracts/contracts/interfaces/IGetter.sol                    | 5        | 45             |
| diva-contracts/contracts/interfaces/IGovernance.sol                | 90       | 21             |
| diva-contracts/contracts/interfaces/ILiquidity.sol                 | 31       | 9              |
| diva-contracts/contracts/interfaces/IPermissionedPositionToken.sol | 4        | 15             |
| diva-contracts/contracts/interfaces/IPool.sol                      | 11       | 5              |
| diva-contracts/contracts/interfaces/IPositionToken.sol             | 4        | 13             |
| diva-contracts/contracts/interfaces/IPositionTokenFactory.sol      | 3        | 5              |
| diva-contracts/contracts/interfaces/ISettlement.sol                | 53       | 13             |
| diva-contracts/contracts/interfaces/ITellor.sol                    | 3        | 3              |
| diva-contracts/contracts/interfaces/ITip.sol                       | 14       | 5              |
| **Totals**                                                         | **4296** | **2826**       |

## Methodology

The auditing process pays special attention to the following considerations:
- Testing the smart contracts against both common and uncommon attack vectors.
- Assessing the codebase to ensure compliance with current best practices and industry standards.
- Ensuring contract logic meets the specifications and intentions of the client.
- Cross-referencing contract structure and implementation against similar smart contracts produced by industry leaders.
- Thorough line-by-line manual review of the entire codebase by community auditors.

## Vulnerability Classifications

| Vulnerability Level             | Classification                                                                                                  |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| [Critical](#Critical)           | Easily exploitable by anyone, causing loss/manipulation of assets or data.                                      |
| [High](#High)                   | Arduously exploitable by a subset of addresses, causing loss/manipulation of assets or data.                    |
| [Medium](#Medium)               | Inherent risk of future exploits that may or may not impact the smart contract execution.                       |
| [Low](#Low)                     | Minor deviation from best practices.                                                                            |
| [Informational](#Informational) | Suggestions or recommendations, but not necessarily indicating a vulnerability or immediate risk to the system. |

# Findings & Resolutions

| ID           | Title                                                                                            | Severity      | Status  |
| ------------ | ------------------------------------------------------------------------------------------------ | ------------- | ------- |
| [H-01](#H01) | Wrong implementation of EVMCall in DIVAOwnershipSecondary                                        | High          | Resolved |
| [H-02](#H02) | Funds could be stuck in DIVADevelopmentFund                                                      | High          | Resolved |
| [M-01](#M01) | Wrong protocol fee recipient when withdrawing liquidity                                          | Medium        | Resolved |
| [M-02](#M02) | PreviousFallbackDataProvider won't have incentive to provide accurate value                      | Medium        | Resolved |
| [M-03](#M03) | Fee-on-Transfer tokens used as collateral will make a pool undercollateralized                   | Medium        | Resolved |
| [M-04](#M04) | DoS in `_calcPayoffs` function when calculating big numbers                                      | Medium        | Resolved |
| [L-01](#L01) | Update openzeppelin NPM dependencies in package.json                                             | Low           | Resolved |
| [L-02](#L02) | Missing boundries for _maxDIVARewardUSD in DIVAOracleTellor                                      | Low           | Resolved |
| [L-03](#L03) | Fee-on-transfer tokens will get stuck in Development Fund                                        | Low           | Resolved |
| [L-04](#L04) | Wrong implementation of EIP-2535 in `LibDiamond` library                                         | Low           | Resolved |
| [L-05](#L05) | Missing important data in events                                                                 | Low           | Resolved |
| [L-06](#L06) | Don't allow setting owner to address(0) in `DIVAOwnershipSecondary`                              | Low           | Resolved |
| [L-07](#L07) | Add a minimum deposit amount in `DIVADevelopmentFund`                                            | Low           | Resolved |
| [L-08](#L08) | Missing possibility of removing deposits that are fully paid in DIVADevelopmentFund              | Low           | Resolved |
| [L-09](#L09) | DiamondCutFacet should close the Diamond after getting called                                    | Low           | Resolved |
| [L-10](#L10) | Neither the long nor the short token can be conditionally burned                                 | Low           | Resolved |
| [L-11](#L11) | Trapped ETH in the Diamond contract                                                              | Low           | Resolved |
| [I-01](#I01) | Missing validation on deployment of DIVAOracleTellor                                             | Informational | Resolved |
| [I-02](#I02) | Pragma version                                                                                   | Informational | Resolved |
| [I-03](#I03) | Use specific imports instead of just a global import in DIVAOracleTellor                         | Informational | Resolved |
| [I-04](#I04) | Useless `require` statement at `_diamondCut` function                                            | Informational | Resolved |
| [I-05](#I05) | Missing function to query for `_permissionedPositionTokenImplementation` in PositionTokenFactory | Informational | Resolved |
| [I-06](#I06) | Add missing variable checks in constructor                                                       | Informational | Resolved |
| [I-07](#I07) | Consider resetting values after a new Owner has claimed the ownership in DIVAOwnershipMain       | Informational | Resolved |
| [I-08](#I08) | Missing NatSpec @inheritdoc in implementations                                                   | Informational | Resolved |
| [I-09](#I09) | Change immutable to constant if a fixed value is used                                            | Informational | Resolved |
| [I-10](#I10) | Missing NatSpec in diva-contracts Interfaces                                                     | Informational | Resolved |
| [I-11](#I11) | Misleading Typo in comment                                                                       | Informational | Resolved |


## <a id="High"></a> High

### <a id="H01"></a> H-01 Wrong implementation of EVMCall in DIVAOwnershipSecondary
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipSecondary.sol#L113

#### Summary
The implementation in `DIVAOwnershipSecondary.getQueryDataAndId()` is generating a wrong `queryId` as the `queryData` is wrongly encoded.

#### Description
Tellor is using the [Tellor Data Specifications](https://github.com/tellor-io/dataSpecs#create-new-query-type) as the standard for how to structure a query.

The [EVMCall](https://github.com/tellor-io/dataSpecs/blob/main/types/EVMCall.md) is used to query and validate data from one chain on another. 
> The `EVMCall` query type allows users to bridge on-chain data from one EVM chain to another. Users can tip Tellor reporters to bridge balances, NFT ownership, and any other public data from one EVM blockchain to another, even if there is no bridge.

The `EVMCall` query type parameters consits of the following: `chainId` + `contractAddress` and `calldata`. The calldata is the **queryData** used in `msg.data` to call the contract on a specific chain.

The current implementation is using the right function signature `0xa18a186b` for the call, but as it's wrongly encoded, the `queryData` will be wrong and the contract on mainnet can't be called with the calldata here.

Current implementation:
```bash
➜ uint256 _mainChainId = 1;
➜ address _ownershipContractMainChain = 0xe8a4517C0380A5aC4164B8e4C7A480E27E47830B;

➜ abi.encode("EVMCall", abi.encode(_mainChainId, _ownershipContractMainChain, 0xa18a186b))
0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000745564d43616c6c0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000001000000000000000000000000e8a4517c0380a5ac4164b8e4c7a480e27e47830b00000000000000000000000000000000000000000000000000000000a18a186b

➜ keccak256(abi.encode("EVMCall", abi.encode(_mainChainId, _ownershipContractMainChain, 0xa18a186b)))
Type: bytes32
└ Data: 0xeb5360f9a90aa7690e0ac7e503d530c92aa954ae2e9b59f0db5fa6e1ed5a8888
```

Expected
```bash
➜ uint256 _mainChainId = 1;
➜ address _ownershipContractMainChain = 0xe8a4517C0380A5aC4164B8e4C7A480E27E47830B;

➜ abi.encode("EVMCall", abi.encode(_mainChainId, _ownershipContractMainChain, abi.encodeWithSignature("getCurrentOwner()")))
0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000745564d43616c6c0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000001000000000000000000000000e8a4517c0380a5ac4164b8e4c7a480e27e47830b00000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000004a18a186b00000000000000000000000000000000000000000000000000000000

➜ keccak256(abi.encode("EVMCall", abi.encode(_mainChainId, _ownershipContractMainChain, abi.encodeWithSignature("getCurrentOwner()"))))
Type: bytes32
└ Data: 0x039f2dda5530f8d2778820316be007a497783803a1c5525828ad3aecc92b0212
```

The correct queryId should be `0x039f2dda5530f8d2778820316be007a497783803a1c5525828ad3aecc92b0212` but in the current implementation it is `0xeb5360f9a90aa7690e0ac7e503d530c92aa954ae2e9b59f0db5fa6e1ed5a8888`.

As of the wrong `queryData`, automated systems and reporters or the [monitoring](https://docs.tellor.io/tellor/disputing-data/monitoring) can't validate the `EVMCall`.

They will call `DIVAOwnershipMain` with a msg.data of `00000000000000000000000000000000000000000000000000000000a18a186b` instead of `a18a186b00000000000000000000000000000000000000000000000000000000`.

This results in calling the function with signature `00000000` instead of `a18a186b`. As this function signature doesn't exist in DIVAOwnershipMain the call will revert and reporters and validators can't validate the value.

#### Recommendation
Follow the Tellor Data Specifications for the EVMCall and change the function to the following:

```solidity
        queryData = 
                abi.encode(
                    "EVMCall",
                    abi.encode(_mainChainId, _ownershipContractMainChain, abi.encodeWithSignature("getCurrentOwner()"))
                );
```




-----------------

### <a id="H02"></a> H-02 Funds could be stuck in DIVADevelopmentFund
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#LL43C54-L43C54
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L61

#### Summary
If a deposit  is done with _releasePeriodInSeconds = 0 all the tokens in that transaction will be stuck forever in the contract.

#### Description
In `DIVADevelopmentFund#deposit` when a user calls the deposit function with the `_releasePeriodInSeconds` set to zero, this would cause the token deposited following this call to be stuck in the contract forever. This is possible since the value of `_releasePeriodInSeconds` is not validated within the `deposit` function.
The reason for this behaviour is as a result of the check made in `DIVADevelopmentFund#withdraw`
```
if (_deposit.lastClaimedAt < _deposit.endTime) {//@audit-info trapping same block.timestamp event
    _claimableAmount += _claimableAmountForDeposit(_deposit);
    _deposit.lastClaimedAt = block.timestamp;
}
```
When `_releasePeriodInSeconds` is set to zero:
The `_deposits` array is updated with the following values:
```
_deposits.push(
            Deposit(
                _token,
                _amount,
                block.timestamp,
                block.timestamp + 0,
                block.timestamp
            )
        );
```
when a user(owner) tries to withdraw such deposit.
 _deposit.endTime equals to `block.timestamp + 0`, which means _deposit.lastClaimedAt == _deposit.endTime == _deposit.startTime. This means the condition (`if (_deposit.lastClaimedAt < _deposit.endTime)`) would never be met therefore no call would be made to `_claimableAmountForDeposit`. And the `_claimableAmount` for such deposit would not be accounted for.

For a scenario where a user supplies a very high value for the `_releasePeriodInSeconds` owner has to wait for that specified amount of time to be able to withdraw the token or for a reasonable _claimable amount to be calculated since all _claimableAmount would round to zero prior to the _deposit.endTime(or closer to that time). 

Note: tokens deposited with the `deposit` function can not be withdrawn using the `withdrawDirectDeposit` meaning if a vesting deposit can not be withdrawn using the `withdraw` function then the funds associated with such deposit are stuck forever. 

This issue is rated high since a user can consciously set `_releasePeriodInSeconds` to zero in an intention for the funds to be readily available for the DIVAdevelopment without the knowledge that the fund would be stuck forever.

#### Proof of Concept
see coded POC [here](https://gist.github.com/zaskoh/296a826902b2e9ceb0d2a8d7d5adcd57)

#### Recommendation

* check if the value of `_releasePeriodInSeconds` is zero and send the deposit directly to the contract, or change `<` to `<=` to the invariant on the withdraw function loop.

* The deposit should also check for reasonably maximum _releasePeriodInSeconds  like <= 30 years (as 30-year period was mentioned in the docs) to avoid funds getting stuck due to an unreasonable long _releasePeriodInSeconds


-----------------



## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 Wrong protocol fee recipient when withdrawing liquidity
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L783


#### Summary
When withdrawing liquidity, the wrong fee recipient is used to allocate the protocol fees.

#### Description
In `LibDIVA` library, the `_removeLiquidityLib` function allocates the protocol fees to the wrong address. 

The current treasury address is determined by the following function:
```solidity
function _getCurrentTreasury(LibDIVAStorage.GovernanceStorage storage _gs)
    internal
    view
    returns (address)
{
    // Return the new treasury address if `block.timestamp` is at or past
    // the activation time, else return the current treasury address
    return
        block.timestamp < _gs.startTimeTreasury
            ? _gs.previousTreasury
            : _gs.treasury;
}
```
This means that if `startTimeTreasury` is bigger than `block.timestamp`, the treasury address is `preaviousTreasury` instead of `treasury`. Having established this, when the `_removeLiquidityLib` function allocates the protocol fees to the treasury address, it should call `getCurrentTreasury` to know what is the correct treasury address at that moment. Instead of that, is directly allocates the fees to `treasury`.

```solidity
// Allocate protocol fee to DIVA treasury. Fee is held within this
// contract and can be claimed via `claimFee` function.
// `collateralBalance` is reduced inside `_allocateFeeClaim`.
_allocateFeeClaim(
    _removeLiquidityParams.poolId,
    _pool,
    LibDIVAStorage._governanceStorage().treasury, // @audit-issue it should call `_getCurrentTreasury`
    _protocolFee
);
```

This means that when the owner changes the treasury address, it will start to receive fees from the withdrawal of liquidity at that exact time when in theory it should wait 2 days to update the treasury address. 

#### Recommendation
Change the affected code to call `_getCurrentTreasury` function to get the current treasury address instead of directly allocating the fees to `LibDIVAStorage._governanceStorage().treasury`.

```solidity

address _treasury = _getCurrentTreasury();

// Allocate protocol fee to DIVA treasury. Fee is held within this
// contract and can be claimed via `claimFee` function.
// `collateralBalance` is reduced inside `_allocateFeeClaim`.
_allocateFeeClaim(
    _removeLiquidityParams.poolId,
    _pool,
    _treasury, // @audit-ok
    _protocolFee
);
```




### <a id="M02"></a> M-02 PreviousFallbackDataProvider won't have incentive to provide accurate value
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/facets/SettlementFacet.sol#L487

#### Summary
The `previousFallbackDataProvider` would have to submit value without a settlement fee.

#### Description
When there is change in the `fallbackDataProvider`, the new `fallbackDataProvider` has to wait for `startTimeFallbackDataProvider` to be able to submit a value but yet receives the SettlementFee within that period(60-day delay), meanwhile the `previousFallbackDataProvider` has to submit value as a result of the if condition [#L480](https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/facets/SettlementFacet.sol#L480).
This is wrong since the `previousFallbackDataProvider` won't have incentive to provide an accurate value.  
```
if (msg.sender != LibDIVA._getCurrentFallbackDataProvider(_gs))
                    revert NotFallbackDataProvider();

_confirmFinalReferenceValue(
                    _poolId,
                    _pool,
                    _treasury,
                    _gs.fallbackDataProvider,//@audit-issue - new fallbackDataProvider receives settlement fee
                    _finalReferenceValue,
                    _gs
                );
```

#### Recommendation
change [SettlementFacet.sol#L487](https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/facets/SettlementFacet.sol#L487) to:
```
_confirmFinalReferenceValue(
                    _poolId,
                    _pool,
                    _treasury,
                    msg.sender, //@audit-ok
                    _finalReferenceValue,
                    _gs
                );
```




### <a id="M03"></a> M-03 Fee-on-Transfer tokens used as collateral will make a pool undercollateralized
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L475-L490

#### Summary
Using fee-on-transfer tokens as collateral for a pool will make a pool undercollateralized thus not making users of that pool whole. 

#### Description
Fee-on-transfer tokens are those who apply a fee every time a transfer occurs. Actually there're some recognised tokens that use that mechanism (`STA`, `PAXG`) and some do not currently charge a fee but may do so in the future (e.g. `USDT`, `USDC`).

By using this kind of tokens as collateral for a pool, when transferring the tokens from the users to the pool, the contract will account for the whole amount of tokens but in reality there'll be less than accounted due to the fees. 

The documentation states that rebasable tokens shouldn't be used as collateral but it doesn't mention FoT tokens. 

Docs: 

![image](https://user-images.githubusercontent.com/90318659/227597356-1c5e4019-3e60-4e32-b837-11588959d4c4.png)

#### Recommendation
Describe explicitly in the documentation if fee-on-transfer tokens are allowed as collateral or implement a mechanism in the code to support this kind of tokens. 

Even if fee-on-transfer tokens are not supported, it's a best practice to change the code to check if the amount of `collateralToken` received is the same as expected before minting the S/L tokens. With this check, it's not possible to create a new pool or add liquidity to a pool if the fee-on-transfer was just activated. 




### <a id="M04"></a> M-04 DoS in `_calcPayoffs` function when calculating big numbers
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L421-L430

#### Summary
The `_calcPayoffs` function reverts everytime is called with `_finalReferenceValue` >= `2e59` and `cap` > `_finalReferenceValue`.

#### Description
In `LibDIVA` library, the `_calcPayoffs` function calculates the final payout for the short and long token holders deResolved of the `finalReferenceValue` provided by the data provider. This function is called from `_setPayoutAmount`, that is called by `_confirmFinalReferenceValue` that is the function called when the data provider sets the final reference value. 

The issue is that when this function is called for a pool with a `finalReferenceValue` >= `2e59` and `cap` > `finalReferenceValue`, the function will always revert causing a DoS. 

**Example**
A group of people want to create a pool that needs a cap value of `1e60` or more because it needs a lot of precision (could be something science-related). In this case, when the expiry time of that pool arrives and the `finalReferenceValue` is close to `cap` the function will revert everytime leaving the users without their fair payout. 

If this scenario happens, they have 2 options:
 1. Wait for the submission period and review period to end and set the `inflection` as the final reference value.
 2. Wait for the submission period to end and the `fallbackDataProvider` can submit a `finalReferenceValue` that don't revert. 

Both options leaves the users receiving an unfair payout because the protocol didn't function as promised with large values. 

#### POC
https://gist.github.com/santipu03/ad81055d2698382fbf812315ef548ccb

#### Recommendation
The protocol has 2 options:
 - Before creating a pool, set a limit for the `cap` value so that this situation cannot happen. 
 - Warn users in the docuentation or website about the danger of setting large numbers in the pool. 


-----------------


## <a id="Low"></a> Low

### <a id="L01"></a> L-01 Update openzeppelin NPM dependencies in package.json
https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/package.json#L56
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/package.json#L106-L107

#### Description:
For oracles you currently use `^4.4.0` and for diva-contracts `^4.7.3` and `^4.8.0-rc.1`. 
You should consider updating to the newest version `^4.8.2`

https://security.snyk.io/package/npm/@openzeppelin%2Fcontracts

Even if the currently used packages and implementations may not be directly vulnarable to one of the listed vulnerabilities, you should consider updating it.

#### Recommendation:
Update package.json to use `^4.8.2` for `@openzeppelin/contracts` and `@openzeppelin/contracts-upgradeable`


### <a id="L02"></a> L-02 Missing boundries for _maxDIVARewardUSD in DIVAOracleTellor
https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/contracts/DIVAOracleTellor.sol#L63
https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/contracts/DIVAOracleTellor.sol#L241

#### Description:
Currently setting `_maxDIVARewardUSD` is not checked for any upper or lower values in the `constructor` and in `updateMaxDIVARewardUSD`

If 0 is set for `_maxDIVARewardUSD` there will be no reward for the reporter and the DIVARewardRecipient will get all rewards instead. 

```solidity
uint256 _currentMaxDIVARewardUSD = _getCurrentMaxDIVARewardUSD();
if (divaRewardClaimUSD > _currentMaxDIVARewardUSD) {
    // if _formattedCollateralToUSDRate = 0, then divaRewardClaimUSD = 0 in
    // which case it will go into the else part, hence division by zero
    // is not a problem
    divaRewardToReporter =
        _currentMaxDIVARewardUSD.divideDecimal(
            _formattedCollateralToUSDRate
        ) /
        _SCALING; // integer with collateral token decimals
```

#### Recommendation:
Add upper and lower boundries to check the settings value in the `constructor` and in `updateMaxDIVARewardUSD`.

### <a id="L03"></a> L-03 Fee-on-transfer tokens will get stuck in Development Fund
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L58-L72

#### Description
In `DIVADevelopmentFund` contract, the `deposit` function doesn't support fee-on-transfer tokens because it doesn't check the balance before and after. The documentation doesn't mention that this kind of tokens aren't supported so if a user wants to contribute to the Development Funds by sending some tokens that implement fee-on-transfer (this could be the case with `USDC` and `USDT` in the future) this will result in some tokens stuck in the contract. 

However, if this case happens the tokens could be recovered by sending directly to the contract the tokens that are missing due to the fee. 

#### Recommendation 
The documentation should explicitly state if the contract support fee-on-transfer tokens or the contract should implement the logic to handle this tokens.

### <a id="L04"></a> L-04 Wrong implementation of EIP-2535 in `LibDiamond` library
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDiamond.sol#L266-L268

#### Description
In `LibDiamond` library, the `_initializeDiamondCut` function reverts if the address `_init` is zero and `_calldata` is not empty.

This is a wrong implementation of the official EIP-2535. As the official EIP page states:

*"If the `_init` value is address(0) then `_calldata` execution is skipped. In this case `_calldata` can contain 0 bytes or custom information."*

https://eips.ethereum.org/EIPS/eip-2535#executing-_calldata


#### Recommendation 
In order to implement the official EIP-2535 standard you should remove the check of `_calldata.length > 0` at line 266 and just return here early.

### <a id="L05"></a> L-05 Missing important data in events
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L55
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L71
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipMain.sol#L97
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipMain.sol#L169

#### Description
In some of your events you should consider adding an additional param for better off-chain analysis.

#### Recommendation 
Consider changing the event and add the suggested data for emitting.

```solidity
File: contracts/DIVADevelopmentFund.sol
55:         emit Deposited(msg.sender, _depositIndex); // @audit-info L consider adding token address for event
71:         emit Deposited(msg.sender, _depositIndex); // @audit-info L consider adding token address for event

File: contracts/DIVAOwnershipMain.sol
97:         emit Staked(_candidate, _amount); // @audit-info consider adding msg.sender in Stake event for more clarity
169:         emit Unstaked(_candidate, _amount); // @audit-info consider adding msg.sender in Unstaked event for more clarity
```

### <a id="L06"></a> L-06 Don't allow setting owner to address(0) in `DIVAOwnershipSecondary`
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipSecondary.sol#L84

#### Description
Currently it's possible that the owner of a secondary chain get's updated to address(0).

#### Recommendation 
Check the value of the address that Tellor sets that it is not address(0) before updating the owner.

```solidity
File: contracts/DIVAOwnershipSecondary.sol
81:         address _formattedOwner = abi.decode(_valueRetrieved, (address));
82:         
83:         // Update owner to the owner returned by the Tellor protocol
84:         _owner = _formattedOwner; // @audit-info add address(0) != check before setting the new owner
```

### <a id="L07"></a> L-07 Add a minimum deposit amount in `DIVADevelopmentFund`
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L51
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L65
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L211

#### Description
Currently it's possible to create deposits with a value of 0 in DIVADevelopmentFund. An attacker could flood the _deposits array with 0 value Deposits and the protocol team needs to make a closer investigation to analyze which Deposits realy hold any value and are good for a withdraw.

#### Recommendation 
In the function `_addNewDeposit` add a minimum check before adding a new Deposit to the _deposits array.

### <a id="L08"></a> L-08 Missing possibility of removing deposits that are fully paid in DIVADevelopmentFund
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L17
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/interfaces/IDIVADevelopmentFund.sol#L15-L21

#### Description
Currently it's not possible to mark or filter out _deposits that are fully paid in `DIVADevelopmentFund`. This will lead to an only growing array for `_deposits` that can't be filtered.

#### Recommendation 
Consider adding a possibility of removing deposits that are fully paid in DIVADevelopmentFund to shrink the size of the deposits array overtime.


### <a id="L09"></a> L-09 DiamondCutFacet should close the Diamond after getting called
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/facets/DiamondCutFacet.sol#L22

#### Description
As the protocol should be immutable it is best to close the upgradeability as soon as the functions are added in `DiamondCutFacet.diamondCut`.  
Currently in the deployment scripts the Diamond contract is only closed on the main-chain but not for the secondary chains.

#### Recommendation 
Automatically close the Diamond proxy after the first call to `DiamondCutFacet.diamondCut` inside that function.

-----------------


### <a id="L10"></a> L-10 Neither the long nor the short token can be conditionally burned
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L546
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L550

#### Summary
Neither long nor short token can be conditionally burn since `ERC20#_mint` would revert on zero address.

#### Description
The documentation under [createcontingentpool](https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/DOCUMENTATION.md#createcontingentpool) states that for `longRecipient`  or `shortRecipient` a user can choose the zero address to enable conditional burn use cases.

> Address that shall receive the long position token. Zero address is a valid input to enable conditional burn use cases.

However, Any call to [ERC20#_mint](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol#L251) with zero address as the recipient would revert, there is no viable means to conditionally burn one side of the token since they cannot be transferred to address(0) as well.
This leads to discrepancy between the documentation and it's implementation.

#### Recommendation
* Overwrite _mint() and allow minting to address(0)


### <a id="L11"></a> L-11 Trapped ETH in the Diamond contract
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/Diamond.sol#L191

#### Summary
The `receive` function in the Diamond contract can lead to trapped ETH because there's no way to withdraw it from the contract. 

#### Description
In the `Diamond` contract, there's a `receive` function that allows the direct transfer of ETH to the contract. 

If a user transfer ETH directly to the contract by accident, it will be stuck there forever because there's no function in the contract that allows to withdraw ETH from there. Given that the `Diamond` contract will remove the upgradability just after deployment, all the ETH sent to the contract by accident will be trapped forever.  

#### Recommendation
Remove the `receive` function from the `Diamond` contract. 

```diff
-receive() external payable {}
```

## <a id="Informational"></a> Informational

### <a id="I01"></a> I-01 Missing validation on deployment of DIVAOracleTellor
https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/contracts/DIVAOracleTellor.sol#L63-L64
https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/contracts/UsingTellor.sol#L20

#### Description:
The deployment for the `DIVAOracleTellor` contract is missing important validations for immutable variables.
If not set correctly, a redeployment is needed.

Additionally you should also check that `_excessDIVARewardRecipient` is not address(0) in the constructor, or all rewards will be transfered to the zero address until changed via function `updateExcessDIVARewardRecipient`

#### Recommendation:
Check that the address is not address(0) for the following three variables.

```solidity
File: oracles/contracts/DIVAOracleTellor.sol
62:         _excessDIVARewardRecipient = excessDIVARewardRecipient_;
64:         _diva = IDIVA(diva_);

File: oracles/contracts/UsingTellor.sol
20:         tellor = ITellor(_tellor);
```

### <a id="I02"></a> I-02 Pragma version
https://github.com/GuardianAudits/DivaAudit/tree/main/diva-contracts/contracts
https://github.com/GuardianAudits/DivaAudit/tree/main/oracles/contracts

#### Description:
For oracles you currently use `0.8.9` and for diva-contracts you use `0.8.19`.

Releases https://github.com/ethereum/solidity/blob/develop/Changelog.md

`0.8.19` is a bit too recent and `0.8.9` is a bit old.

#### Recommendation:
You should consider updating both to a more battle tested version like `0.8.17` regarding the last fixes.

https://docs.soliditylang.org/en/v0.8.19/bugs.html

### <a id="I03"></a> I-03 Use specific imports instead of just a global import in DIVAOracleTellor
https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/contracts/DIVAOracleTellor.sol#L4-L11
https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/contracts/UsingTellor.sol#L4

#### Description:
For a better developer experience it's better to use specific imports instead of just a global import. This helps to have a better overview what is realy needed and helps to have a clearer view of the contract.

#### Recommendation:
Use the following pattern for your imports.

```solidity
import {contract1 , contract2} from "filename.sol";
```

### <a id="I04"></a> I-04 Useless `require` statement at `_diamondCut` function
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDiamond.sol#L49-L51

#### Description:

In `LibDiamond` library, the `_diamondCut` function executes a `require` statement that's useless because it's also executed in the functions that are called next. 

In line 49, there's a check that ensures that `functionSelectors.length == 0`. After this, only three functions can called (`_addFunctions`, `_replaceFunctions` and `_removeFunctions`) and the three of them do the same check. 

#### Recommendation:
Remove the `require` statement at line 49 because the same check will be executed later in the next functions. 

### <a id="I05"></a> I-05 Missing function to query for `_permissionedPositionTokenImplementation` in PositionTokenFactory
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/PositionTokenFactory.sol

#### Description:

The `PositionTokenFactory` contract is missing a function to query the `_permissionedPositionTokenImplementation`. As the contract currently has a function to query the `_positionTokenImplementation` it is suggested to also give this possibility for the `_permissionedPositionTokenImplementation`.

#### Recommendation:
Implement a getter function like you have for `_positionTokenImplementation` for `_permissionedPositionTokenImplementation`.


### <a id="I06"></a> I-06 Add missing variable checks in constructor 
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipSecondary.sol#L48-L50
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/Diamond.sol#L66
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipMain.sol#L67-L68
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L37

#### Description:
Currently you miss some important variable checks in the constructor. You should consider adding some of the validations to mitigate a potential broken deployment and a needed redeployment.

#### Recommendation:

```solidity
File: contracts/DIVAOwnershipSecondary.sol
47:     ) payable UsingTellor(_tellorAddress) { // @audit-info add address(0) != checks to mitigate a redployment

File: contracts/Diamond.sol
66:         address _diamondCutFacet, // @audit-info add also for _diamondCutFacet address(0) check like for the other addresses in the constructor

File: contracts/DIVAOwnershipMain.sol
63:     constructor(
64:         address _initialOwner,
65:         IERC20Metadata divaToken_
66:     ) payable { // @audit-info add address(0) != checks to mitigate a redployment
67:         _owner = _initialOwner;
68:         _divaToken = divaToken_;
69:     }

File: contracts/DIVADevelopmentFund.sol
36:     constructor(IDIVAOwnershipShared divaOwnership_) payable {
37:         _divaOwnership = divaOwnership_; // @audit-info add address(0) != checks to mitigate a redployment
38:     }
```


### <a id="I07"></a> I-07 Consider resetting values after a new Owner has claimed the ownership in DIVAOwnershipMain
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipMain.sol#L143

#### Description:
Currently after setting the new onwer via `submitOwnershipClaim` in `DIVAOwnershipMain` the following variables could be resettet to their default values.

- _showdownPeriodEnd
- _submitOwnershipClaimPeriodEnd
- _cooldownPeriodEnd

This will make it easier off-chain to know if there is currently a vote ongoing without any calculations as when this 3 variables are zero, there is no election cycle running.

#### Recommendation:
Update `_showdownPeriodEnd`, `_submitOwnershipClaimPeriodEnd` and `_cooldownPeriodEnd` to **zero** after setting the new owner.


### <a id="I08"></a> I-08 Missing NatSpec @inheritdoc in implementations
https://github.com/GuardianAudits/DivaAudit/tree/main/diva-contracts/contracts
https://github.com/GuardianAudits/DivaAudit/tree/main/oracles/contracts

#### Description:
For a better user and deceloper experience you should consider adding the NatSpec @inheritdoc for all the functions implementing an Interface definition.

#### Recommendation:
Add for all functions that override the interface declaration the @inheritdoc NatSpec.

Example
```solidity
File: oracles/contracts/DIVAOracleTellor.sol
128:     /// @inheritdoc IDIVAOracleTellor
129:     function claimReward(
130:         uint256 _poolId,
131:         address[] calldata _tippingTokens,
132:         bool _claimDIVAReward
```

### <a id="I09"></a> I-09 Change immutable to constant if a fixed value is used
https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/contracts/DIVAOracleTellor.sol#L61

#### Description:
In `DIVAOracleTellor` you have defined an immutable variable `_challengeable` that is set with a fixed value of `false`

```solidity
File: oracles/contracts/DIVAOracleTellor.sol
33:     bool private immutable _challengeable;
```

#### Recommendation:

Change to 

```solidity
File: oracles/contracts/DIVAOracleTellor.sol
33:     bool private constant _challengeable = false;
```
and remove the setting in the constructor.

### <a id="I10"></a> I-10 Missing NatSpec in diva-contracts Interfaces 
https://github.com/GuardianAudits/DivaAudit/tree/main/diva-contracts/contracts/interfaces

#### Description:
Your code is very well documented. Still, some functions in your interfaces are missing a @return / @param / ... that you can add.

```solidity
File: contracts/interfaces/IDIVADevelopmentFund.sol
92:     function getDepositsLength() external view returns (uint256); // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVADevelopmentFund.sol
98:     function getDivaOwnership() external view returns (IDIVAOwnershipShared); // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVADevelopmentFund.sol
107:         returns (Deposit memory);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVADevelopmentFund.sol
123:     ) external view returns (uint256[] memory);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVADevelopmentFund.sol
133:         returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVADevelopmentFund.sol
143:         returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
114:         returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
123:         returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
128:     function getTimestampLastStake(address _user)  // @audit-info I missing @param NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
131:         returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
136:     function getShowdownPeriodEnd() external view returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
141:     function getSubmitOwnershipClaimPeriodEnd() external view returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
146:     function getCooldownPeriodEnd() external view returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
151:     function getDIVAToken() external view returns (address);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
156:     function getShowdownPeriod() external pure returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
162:     function getSubmitOwnershipClaimPeriod() external pure returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
169:     function getCooldownPeriod() external pure returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipMain.sol
174:     function getMinStakingPeriod() external pure returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipSecondary.sol
39:     function getOwnershipContractMainChain() external view returns (address);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipSecondary.sol
44:     function getMainChainId() external view returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipSecondary.sol
57:     function getQueryDataAndId() external view returns (bytes memory, bytes32);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IDIVAOwnershipShared.sol
11:     function getCurrentOwner() external view returns (address);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IGetter.sol
115:     function getFeesHistoryLength() external view returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IGetter.sol
124:         returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IGetter.sol
169:     function getClaim(address _collateralToken, address _recipient)  // @audit-info I wrong @param NatSpec order for first and second address

File: contracts/interfaces/IPermissionedPositionToken.sol
45:     function poolId() external view returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IPermissionedPositionToken.sol
51:     function owner() external view returns (address);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IPermissionedPositionToken.sol
56:     function permissionedERC721Token() external view returns (address);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IPool.sol
77:         returns (uint256[] memory);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IPositionToken.sol
61:     function poolId() external view returns (uint256);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/IPositionToken.sol
67:     function owner() external view returns (address);  // @audit-info I missing @return NatSpec description

File: contracts/interfaces/ITellor.sol
5:     function getDataBefore(bytes32 _queryId, uint256 _timestamp)  // @audit-info I missing NatSpec
```

#### Recommendation:

Add the missing NatSpec mentioned in the comments.


### <a id="I11"></a> I-11 Misleading Typo in comment
https://github.com/GuardianAudits/DivaAudit/tree/main/diva-contracts/contracts/interfaces

#### Description:
Currently you have a wrong time in the comment that doesn't match the documentation and implementation.

#### Recommendation:
Change comment from `days` to **hours**
```solidity
File: contracts/DIVAOwnershipSecondary.sol
73:         // Check that value is not older than 36 days
```

-----------------

## Disclaimer
> 
> This report is not, nor should be considered, an “endorsement” or “disapproval” of any particular project or team. This report is not, nor should be considered, an indication of the economics or value of any “product” or “asset” created by any team or project that contracts Solidity Lab to perform a security assessment. This report does not provide any warranty or guarantee regarding the absolute bug-free nature of the technology analyzed, nor do they provide any indication of the technologies proprietors, business, business model or legal compliance.
> 
> This report should not be used in any way to make decisions around investment or involvement with any particular project. This report in no way provides investment advice, nor should be leveraged as investment advice of any sort. This report represents an extensive assessing process intending to help our customers increase the quality of their code while reducing the high level of risk presented by cryptographic tokens and blockchain technology.
> 
> Blockchain technology and cryptographic assets present a high level of ongoing risk. Solidity Lab’s position is that each company and individual are responsible for their own due diligence and continuous security. Solidity Lab’s goal is to help reduce the attack vectors and the high level of variance associated with utilizing new and consistently changing technologies, and in no way claims any guarantee of security or functionality of the technology we agree to analyze.
> 
> The assessment services provided by Solidity Lab is subject to dependencies and under continuing
> development. You agree that your access and/or use, including but not limited to any services, reports, and materials, will be at your sole risk on an as-is, where-is, and as-available basis. Cryptographic tokens are emergent technologies and carry with them high levels of technical risk and uncertainty. The assessment reports could include false positives, false negatives, and other unpredictable results. The services may access, and depend upon, multiple layers of third-parties.
> 
> Notice that smart contracts deployed on the blockchain are not resistant from internal/external exploit. Notice that active smart contract owner privileges constitute an elevated impact to any smart contract’s safety and security. Therefore, Solidity Lab does not guarantee the explicit security of the audited smart contract, regardless of the verdict.

<br/>

____

## About

Solidity Lab is a community of practicing auditors guided by Guardian Audits.

To learn more, visit https://lab.guardianaudits.com

To view the Solidity Lab audit portfolio, visit https://github.com/GuardianAudits/LabAudits