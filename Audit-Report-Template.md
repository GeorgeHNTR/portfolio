# \<<project name\>> Security Review

A security review of the [\<<project name\>>](\<<project site or github link\>>) smart contract protocol was done by [Gogo](https://twitter.com/gogotheauditor). \
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

|               |                                                                                              |
| :------------ | :------------------------------------------------------------------------------------------- |
| Project Name  | \<<project name\>>                                                                           |
| Repository    | \<<provided repo by the project\>>                                                           |
| Commit hash   | [\<<provided commit hash by the project\>>](\<<link to the specific commit code to audit\>>) |
| Documentation | [\<<link\>>](\<<link to the docs provided by the project\>>)                                 |
| Methods       | Manual review                                                                                |
|               |

### Issues found

| Severity      |                                                     Count |
| :------------ | --------------------------------------------------------: |
| Critical risk |   \<<number of critical severity vulnerabilities found\>> |
| High risk     |       \<<number of high severity vulnerabilities found\>> |
| Medium risk   |     \<<number of medium severity vulnerabilities found\>> |
| Low risk      |        \<<number of low severity vulnerabilities found\>> |
| Informational | \<<number of informational issues or code improvements\>> |

### Scope

| File                                                                                                    | SLOC                                |
| :------------------------------------------------------------------------------------------------------ | :---------------------------------- |
| _Contracts (\<<number of contracts files in scope\>>)_                                                  |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| _Abstracts (\<<number of abstract contracts files in scope\>>)_                                         |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| _Interfaces (\<<number of interfaces files in scope\>>)_                                                |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| [\<<file relative path\>>](\<<link to file in the provided github repo for the specific commit hash\>>) | \<<source lines of code in file\>>  |
| _Total (\<<the total number of files in scope\>>)_                                                      | \<<total number of sloc in scope\>> |

# Findings

| ID                                         | Title                                    | Severity      |
| ------------------------------------------ | :--------------------------------------- | :------------ |
| [\<<index of critical severity finding\>>] | \<<title of critical severity finding\>> | Critical      |
| [\<<index of high severity finding\>>]     | \<<title of high severity finding\>>     | High          |
| [\<<index of medium severity finding\>>]   | \<<title of medium severity finding\>>   | Medium        |
| [\<<index of low severity finding\>>]      | \<<title of low severity finding\>>      | Low           |
| [\<<index of informational finding\>>]     | \<<title of informational finding\>>     | Informational |

## Critical severity

### [C-\<<index of critical severity finding\>>] \<<finding title\>>

#### **Context**

- [\<<file relative path\>>#\<<lines\>>](\<<link to file in the provided github repo for the specific commit hash\>>#\<<lines\>>)

#### **Description**

\<<description\>>

#### **Proof Of Concept**

```
<<executable exploit scenario test case>>
```

#### **Recommended Mitigation Steps**

\<<recommended way(s) to solve the vulnerability\>>

## High severity

### [H-\<<index of high severity finding\>>] \<<finding title\>>

#### **Context**

- [\<<file relative path\>>#\<<lines\>>](\<<link to file in the provided github repo for the specific commit hash\>>#\<<lines\>>)

#### **Description**

\<<description\>>

#### **Proof Of Concept**

```
<<executable exploit scenario test case>>
```

#### **Recommended Mitigation Steps**

\<<recommended way(s) to solve the vulnerability\>>

## Medium severity

### [M-\<<index of medium severity finding\>>] \<<finding title\>>

#### **Context**

- [\<<file relative path\>>#\<<lines\>>](\<<link to file in the provided github repo for the specific commit hash\>>#\<<lines\>>)

#### **Description**

\<<description\>>

#### **Recommended Mitigation Steps**

\<<recommended way(s) to solve the vulnerability\>>

## Low severity

### [L-\<<index of low severity finding\>>] \<<finding title\>>

#### **Context**

- [\<<file relative path\>>#\<<lines\>>](\<<link to file in the provided github repo for the specific commit hash\>>#\<<lines\>>)

#### **Description**

\<<description\>>

#### **Recommendations**

\<<recommended way(s) to solve the issue or improve code quality\>>

## Informational

### [I-\<<index of informational finding\>>] \<<finding title\>>

#### **Context**

- [\<<file relative path\>>#\<<lines\>>](\<<link to file in the provided github repo for the specific commit hash\>>#\<<lines\>>)

#### **Description**

\<<description\>>

#### **Recommendations**

\<<recommended way(s) to solve the issue or improve code quality\>>
