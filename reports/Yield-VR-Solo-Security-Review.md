# Yield VR Security Review

A security review of the [Yield Variable Rate](https://yieldprotocol.com/) smart contract protocol was done by [Gogo](https://twitter.com/gogotheauditor). \
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

|               |                                                                                                                                     |
| :------------ | :---------------------------------------------------------------------------------------------------------------------------------- |
| Project Name  | Yield Protocol                                                                                                                      |
| Repository    | [https://github.com/yieldprotocol/vault-v2](https://github.com/yieldprotocol/vault-v2)                                              |
| Commit hash   | [1d1602a06fda352f463b6f126c8a90e05e221541](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541) |
| Documentation | [https://docs.yieldprotocol.com/](https://docs.yieldprotocol.com/)                                                                  |
| Methods       | Manual review                                                                                                                       |
|               |

### Issues found

| Severity      | Count |
| :------------ | ----: |
| Critical risk |     0 |
| High risk     |     0 |
| Medium risk   |     3 |
| Low risk      |     2 |
| Informational |     3 |

### Scope

| File                                                                                                                                                                             | nSLOC |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---- |
| Contracts (6)                                                                                                                                                                    |
| [src/variable/VRCauldron.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRCauldron.sol)                               | 297   |
| [src/variable/VRLadle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRLadle.sol)                                     | 210   |
| [src/variable/VRRouter.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRRouter.sol)                                   | 18    |
| [src/variable/VRWitch.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRWitch.sol)                                     | 57    |
| [src/variable/VYToken.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VYToken.sol)                                     | 154   |
| [src/oracles/VariableInterestRateOracle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/oracles/VariableInterestRateOracle.sol) | 149   |
| Interfaces (2)                                                                                                                                                                   |
| [src/variable/interfaces/IVRCauldron.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/interfaces/IVRCauldron.sol)       | 22    |
| [src/variable/interfaces/IVRWitch.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/interfaces/IVRWitch.sol)             | 78    |
| Total (8)                                                                                                                                                                        | 985   |

# Findings

| ID     | Title                                                                       | Severity      | Resolution   |
| :----- | :-------------------------------------------------------------------------- | :------------ | :----------- |
| [M-01] | Deployer can destroy core proxy contracts implementations                   | Medium        | Fixed        |
| [M-02] | VRWitch `initialized` flag can accidentally be reset in sequential upgrades | Medium        | Fixed        |
| [M-03] | Lack of two-step vault ownership transfer process                           | Medium        | Acknowledged |
| [L-01] | Unreachable code                                                            | Low           | Fixed        |
| [L-02] | No upper bound for borrowing fee                                            | Low           | Acknowledged |
| [I-01] | Insufficient NatSpec comments                                               | Informational | Fixed        |
| [I-02] | Typographical issues                                                        | Informational | Fixed        |

## Medium severity

### [M-01] Deployer can destroy core proxy contracts implementations

#### **Context**

- [AccessControl.sol](https://github.com/yieldprotocol/yield-utils-v2/blob/ec066083c12d5e69ede580edf015d58a79d8488a/src/access/AccessControl.sol)
- [ERC1967Upgrade.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/proxy/ERC1967/ERC1967Upgrade.sol#L62)

#### **Description**

One difference between the smart contracts of the Fixed Rate Yield Protocol and the newly modified contracts that are in scope for this audit is the choice of making them UUPS proxies.

The UUPSUpgradeable contract from OpenZeppelin is inherited by the core smart contracts: VRCauldron, VRLadle, VRWitch, and VYToken. This contract allows authorized accounts to perform upgrades by changing the implementation contract address and allows to perform a `delegatecall` to the new implementation contract.

This opens the opportunity to authorized accounts to maliciously destroy the implementation contracts causing permanent DoS because the upgrade mechanism is also hold in the implementation contract.

VRCauldron, VRLadle, VRWitch and VYToken contracts also inherit from AccessControl, which can be found [here](https://github.com/yieldprotocol/yield-utils-v2/blob/ec066083c12d5e69ede580edf015d58a79d8488a/src/access/AccessControl.sol). \
Upon deployment of the core implementation contracts, the admin ("ROOT") role is given to the deployer (msg.sender) of these contracts:

    constructor () {
        _grantRole(ROOT, msg.sender);   // Grant ROOT to msg.sender
        _setRoleAdmin(LOCK, LOCK);      // Create the LOCK role by setting itself as its own admin, creating an independent role tree
    }

This creates a serious risk since the deployer has the power to permanently break the main smart contracts of the system by upgrading the implementation contracts to a malicious contract that includes the `selfdestruct` opcode.

**_NOTE:_** Although the protocol has marked this issue as a [known way](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/How_to_hack.md#non-revoking-of-root) to break the Yield Variable Rate protocol, it is still important to include it in the current report due to the serious impact in case of an eventual exploit.

#### **Recommended Mitigation Steps**

Remove the admin role ("ROOT") of the deployer by adding the following line in the constructors of VRCauldron, VRLadle, VRWitch and VYToken:

```solidity
    constructor() {
        // See https://medium.com/immunefi/wormhole-uninitialized-proxy-bugfix-review-90250c41a43a
        initialized = true; // Lock the implementation contract

+       _revokeRole(ROOT, msg.sender)
  }
```

#### **Resolution**

Fixed: The recommendation was implemented in VRCauldron, VRLadle, VRWitch, and VYToken.

### [M-02] VRWitch `initialized` flag can accidentally be reset in sequential upgrades

#### **Context**

- [src/variable/VRWitch.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRWitch.sol)
- [src/WitchBase.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/WitchBase.sol)

#### **Description**

When implementing upgradable contracts that inherit from other contracts it is important that there are storage gaps in case new storage variables are later added to the inherited contracts.

These contracts can pose a significant risk when updating a contract because they can shift the storage slots of all inherited contracts which in the case of VRWitch and WitchBase can silently reset the `initialized` flag if e.g. a new mapping is added to WitchBase.

#### **Recommended Mitigation Steps**

Add a storage gap to WitchBase.

#### **Resolution**

Fixed: A storage gap of 10 slots is added to WitchBase.

### [M-03] Lack of two-step vault ownership transfer

#### **Context**

- [src/variable/VRLadle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRLadle.sol#L283-L290)
- [src/variable/VRCauldron.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRCauldron.sol#L246-L266)

#### **Description**

A standard deviation from best practices. There is insufficient input validation when changing the owner of a given vault in VRLadle and VRCauldron. Passing the null address as the receiver or making a typo in the input address will lead to a user loosing access to performing any operations with their vaults and the funds that belong to them.

#### **Recommended Mitigation Steps**

Consider implementing a two-step ownership transfer process for the user vaults similar to [OpenZeppelin's approach](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol#L52-L56) or at least add a zero address check to prevent from other side effects.

#### **Resolution**

Acknowledged

## Low severity

### [L-01] Unreachable code

#### **Context**

- [src/variable/VRLadle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRLadle.sol#L122)
- [src/Ladle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/Ladle.sol#L115)
- [src/Ladle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/Ladle.sol#L131-L135)

#### **Description**

There are several places around the codebase where code is made unreachable and there were no tests for it:

```solidity
    function addJoin(bytes6 assetId, IJoin join) external auth {
        ...
        require(join.asset() == asset, "Mismatched asset and join");
        ...
        bool set = (join != IJoin(address(0))) ? true : false; // @audit if `join` was address(0), join.asset() would have already reverted
        _addToken(asset, set); // address(0) disables the token
        ...
    }

    function addPool(bytes6 seriesId, IPool pool)
        external
        auth
    {
        ...
        require (address(fyToken) == address(pool.fyToken()), "Mismatched pool fyToken and series");
        require (fyToken.underlying() == address(pool.base()), "Mismatched pool base and series");
        ...
        bool set = (pool != IPool(address(0))) ? true : false; // @audit if `pool` was address(0), pool.fyToken() would have already reverted
        _addToken(address(fyToken), set);       // address(0) disables the token
        _addToken(address(pool), set);          // address(0) disables the token
        _addIntegration(address(fyToken), set); // address(0) disables the integration
        _addIntegration(address(pool), set);    // address(0) disables the integration
        ...
    }
```

#### **Recommendations**

Use a separate boolean parameter to check whether to perform the above actions or not.

#### **Resolution**

Fixed: The unreachable code is removed.

### [L-02] No upper bound for borrowing fee

#### **Context**

- [src/variable/VRLadle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRLadle.sol#L129)

#### **Description**

There is no upper bound when setting the borrowing fee in VRLadle which can be abused by an authorized account to block the borrowing functionality.

#### **Recommendations**

Consider adding an input validation on borrowingFee preventing it from being set to a value greater than e.g. 1e18.

#### **Resolution**

Acknowledged

## Informational

### [I-01] Insufficient NatSpec comments

#### **Context**

- [src/variable/\*\*](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable)

#### **Description**

NatSpec comments are incomplete, only @dev and @notice tags are used, which harms code readability for auditors and other developers.

#### **Recommendations**

Follow the[code style guide](https://docs.soliditylang.org/en/v0.8.19/style-guide.html) from the solidity docs to improve code readability.

#### **Resolution**

Fixed

### [I-02] Typographical issues

#### **Context**

- [src/variable/VRCauldron.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRCauldron.sol#L369-L370)
- [src/variable/VRLadle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRLadle.sol#L341)
- [src/variable/VRLadle.sol](https://github.com/yieldprotocol/vault-v2/blob/1d1602a06fda352f463b6f126c8a90e05e221541/src/variable/VRLadle.sol#L348)

#### **Description**

There are a few typographical mistakes in the code:

VRCauldron.sol#L369-L370

"borrow" has been mistakenly replaced with "rate".

    /// @dev Add collateral and rate from vault, pull assets from and push rateed asset to user
    /// Or, repay to vault and remove collateral, pull rateed asset from and push assets to user

VRLadle.sol#L341 \
VRLadle.sol#L348

else can be used instead of the second inner ifs to save gas from performing both checks every time.

    if (ink < 0) ilkJoin.exit(to, uint128(-ink)); // @audit use `else` instead
    ...
    if (base > 0) baseJoin.exit(to, uint128(base)); // @audit use `else` instead

#### **Recommendations**

Correct the listed typos.

#### **Resolution**

Fixed
