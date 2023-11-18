# An attacker can cancel each proposal before it is finalized

## Details

### Report ID

17577

### Target

https://bscscan.com/address/0x80531284f27d8b479aCA8dbA18fD6303B4bF1567#code

### Status

<img width="546" alt="image" src="https://user-images.githubusercontent.com/94860638/233742405-4ec86f0a-0651-4449-83d9-79d25454d383.png">

## Description

### Bug Description

There is insufficient validation on who and when can call the `cancelProposal` method on the `Dao` contract.

Currently, there are only two validation checks in place:

1.  The proposal is open (we want to cancel an opened proposal so this one is passed).
2.  The proposal is at least `cancelPeriod` seconds old.

The second check can be easily passed by the attacker because the `cancelPeriod` is the same as the `coolOffPeriod`, which is `864000` seconds (10 days). This matters because for finalizing a proposal, you will have to wait for 1) members to vote and 2) 10 days `coolOffPeriod`, while for canceling it, the attacker will only have to wait 10 days (`cancelPeriod`) since its creation.

### Impact

No proposals will be able to pass as each proposal in the `Dao` contract can be canceled (+ have the votes reset) by anyone before the proposal is finalized/completed.

### Risk Breakdown

Difficulty to Exploit: Easy \
Weakness: Any governance voting result manipulation \
CVSS2 Score: 5

### Recommendation

There are several ways to mitigate the issue I can think of at first glance:

1.  increase the `cancelPeriod` or decrease the `coolOffPeriod` so there is a time window in which the proposal can be finalized/completed without having the risk of being canceled
2.  do not allow proposals that are `finalising` to be canceled
3.  restrict the `cancelProposal` method on the `Dao` contract to only be callable by trusted entity (`onlyDAO` modifier or include some voting power/decision)

### References

https://bscscan.com/address/0x80531284f27d8b479aCA8dbA18fD6303B4bF1567?utm_source=immunefi#code#L669

## Proof of Concept

1.  Create a proposal at say timestamp 1678400000

- `currentProposal` = 11
- `mapPID_open[_currentProposal]` = true
- `mapPID_startTime[_currentProposal]` = block.timestamp // 1678400000

```solidity
598:    // If no existing open DAO proposal; register a new one
599:    function _checkProposal() internal operational isRunning returns(uint) {
600:        require(_RESERVE.globalFreeze() != true); // There must not be a global freeze in place
601:        uint _currentProposal = currentProposal; // Get the current proposal ID
602:        require(mapPID_open[_currentProposal] == false); // There must not be an existing open proposal
603:        _currentProposal += 1; // Increment to the new PID
604:        currentProposal = _currentProposal; // Set current proposal to the new count
605:        mapPID_open[_currentProposal] = true; // Set new proposal as open status
606:        mapPID_startTime[_currentProposal] = block.timestamp; // Set the start time of the proposal to now
607:        return _currentProposal;
608:    }
```

https://bscscan.com/address/0x80531284f27d8b479aCA8dbA18fD6303B4bF1567?utm_source=immunefi#code#L608

2.  Members call `voteProposal()` for the next say 3 days until there are enough votes

```solidity
852:    mapPIDAsset_votes[_currentProposal][votingAssets[i]] += unitsAdded; // Add user's votes for the current proposal
```

https://bscscan.com/address/0x80531284f27d8b479aCA8dbA18fD6303B4bF1567?utm_source=immunefi#code#L852

3.  Call `pollVotes()` to "push the proposal into 'finalising' status"

- `mapPID_finalising[_currentProposal]` = `true`
- `mapPID_coolOffTime[_currentProposal]` = `block.timestamp` // 1678400000 + 3 days in the current example
- and also `mapPID_open[_currentProposal]` is still `true`

```solidity
645:    // Poll vote weights and check if proposal is ready to go into finalisation stage
646:    function pollVotes() external operational {
647:        uint _currentProposal = currentProposal; // Get the current proposal ID
648:        require(mapPID_open[_currentProposal] == true); // Proposal must be open status
649:        bytes memory _type = bytes(mapPID_type[_currentProposal]); // Get the proposal type
650:        if(hasQuorum(_currentProposal) && mapPID_finalising[_currentProposal] == false){
651:            if(isEqual(_type, 'DAO') || isEqual(_type, 'UTILS') || isEqual(_type, 'RESERVE') || isEqual(_type, 'GET_SPARTA') || isEqual(_type, 'ROUTER') || isEqual(_type, 'LIST_BOND') || isEqual(_type, 'GRANT') || isEqual(_type, 'ADD_CURATED_POOL')){
652:                if(hasMajority(_currentProposal)){
653:                    _finalise(_currentProposal, _type); // Critical proposals require 'majority' consensus to enter finalization phase
654:                }
655:            } else {
656:                _finalise(_currentProposal, _type); // Other proposals require 'quorum' consensus to enter finalization phase
657:            }
658:        }
659:    }
```

https://bscscan.com/address/0x80531284f27d8b479aCA8dbA18fD6303B4bF1567?utm_source=immunefi#code#L646

4.  Now the DAO has to wait 10 days starting from `mapPID_coolOffTime[_currentProposal]` which means they will be able to finalize the proposal 13 days after the `startTime`

```solidity
686:    require((block.timestamp - mapPID_coolOffTime[_currentProposal]) > coolOffPeriod, "!cooloff"); // Must be past cooloff period
```

https://bscscan.com/address/0x80531284f27d8b479aCA8dbA18fD6303B4bF1567?utm_source=immunefi#code#L686

5.  The attacker has a 3-day time window to cancel the proposal, even if it is not intended to do so by the DAO members. The second check in `cancelProposal()` will pass on day 10, and the DAO cannot finalize the proposal before then, as they have to wait until day 13 as stated in 4.

```solidity
668:    // Attempt to cancel the open proposal
669:    function cancelProposal() operational external {
670:        uint _currentProposal = currentProposal; // Get the current proposal ID
671:        require(mapPID_open[_currentProposal], "!OPEN"); // Proposal must be open
672:        require(block.timestamp > (mapPID_startTime[_currentProposal] + cancelPeriod), "!days"); // Proposal must not be new
673:        address [] memory votingAssets =  _POOLFACTORY.getVaultAssets(); // Get array of vault-enabled pools
674:        for(uint i = 0; i < votingAssets.length; i++){
675:           mapPIDAsset_votes[_currentProposal][votingAssets[i]] = 0; // Reset votes to 0
676:        }
677:        mapPID_finalising[_currentProposal] = false; // Remove proposal from 'finalising' stage
678:        mapPID_open[_currentProposal] = false; // Close the proposal
679:        emit CancelProposal(msg.sender, _currentProposal);
680:    }
```

https://bscscan.com/address/0x80531284f27d8b479aCA8dbA18fD6303B4bF1567?utm_source=immunefi#code#L669
