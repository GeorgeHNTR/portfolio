---
title: \<\<project-name\>\> Security Review
date: \<\<current-date\>\>
header-includes:
  - \usepackage{titling}
  - \usepackage{graphicx}
lang: "en"
toc: true
toc-title: Table of Contents
toc-depth: 3
numbersections: true
...

\clearpage

# About \<\<researcher-name\>\>

\vspace{-0.25cm}

\<\<researcher-introduction\>\>

\vspace{-0.15cm}

# Disclaimer

\vspace{-0.25cm}

Audits are a time, resource and expertise bound effort where trained experts evaluate smart contracts using a combination of automated and manual techniques to find as many vulnerabilities as possible. Audits can show the presence of vulnerabilities **but not their absence**.

\vspace{-0.15cm}

# Risk classification

\vspace{-0.25cm}

| Severity           | Impact: High | Impact: Medium | Impact: Low |
| :----------------- | :----------: | :------------: | :---------: |
| Likelihood: High   |   Critical   |      High      |   Medium    |
| Likelihood: Medium |     High     |     Medium     |     Low     |
| Likelihood: Low    |    Medium    |      Low       |     Low     |

## Impact

\vspace{-0.25cm}

- **High** - leads to a significant loss of assets in the protocol or significantly harms a group of users.
- **Medium** - only a small amount of funds can be lost or a functionality of the protocol is affected.
- **Low** - any kind of unexpected behaviour that's not so critical.
  \vspace{-0.15cm}

## Likelihood

\vspace{-0.25cm}

- **High** - direct attack vector; the cost is relatively low to the amount of funds that can be lost.
- **Medium** - only conditionally incentivized attack vector, but still relatively likely.
- **Low** - too many or too unlikely assumptions; provides little or no incentive.
  \vspace{-0.15cm}

## Actions required by severity level

\vspace{-0.25cm}

- **Critical** - client **must** fix the issue.
- **High** - client **must** fix the issue.
- **Medium** - client **should** fix the issue.
- **Low** - client **could** fix the issue.
  \vspace{-0.15cm}

\clearpage

# Executive summary

\begin{center}
\textbf{Overview}
\end{center}

\begin{center}
\begin{tabular}{ | m{4cm} | m{10.1cm}| }
\hline
Project Name & <<project-name>> \\
\hline
Repository & <<project-repository>> \\
\hline
Commit hash & <<review-hash>> \\
\hline
Resolution & <<resolution-hash>> \\
\hline
Methods & Manual review \\
\hline
\end{tabular}
\end{center}

\begin{center}
\textbf{Scope}
\end{center}

\begin{center}
\begin{tabular}{ | m{4.6cm} | }
\hline
<<relative-path-1>> \\
\hline
<<relative-path-2>> \\
\hline
<<relative-path-3>> \\
\hline
\end{tabular}
\end{center}

\begin{center}
\textbf{Issues Found}
\end{center}

\begin{center}
\begin{tabular}{ | m{3cm} | m{0.4cm}| }
\hline
Governance risk & <<g>> \\
\hline
Critical risk & <<c>> \\
\hline
High risk & <<h>> \\
\hline
Medium risk & <<m>> \\
\hline
Low risk & <<l>> \\
\hline
Informational & <<i>> \\
\hline
\end{tabular}
\end{center}

\clearpage

# Findings

\vspace{-0.40cm}

## Governance risk

\vspace{-0.25cm}

### \<\<title-governance-1\>\>

\vspace{-0.25cm}

**Severity:** \textit{Governance risk}

**Context:** [\<\<relative-path-governance-1\>\>](\<\<link-governance-1\>\>)

**Description:** \<\<description-governance-1\>\>

**Recommendation:** \<\<recommendation-governance-1\>\>

**Resolution:** \<\<resolution-governance-1\>\>

\vspace{-0.40cm}

## Critical risk

\vspace{-0.25cm}

### \<\<title-critical-1\>\>

\vspace{-0.25cm}

**Severity:** \textit{Critical risk}

**Context:** [\<\<relative-path-critical-1\>\>](\<\<link-critical-1\>\>)

**Description:** \<\<description-critical-1\>\>

**Recommendation:** \<\<recommendation-critical-1\>\>

**Resolution:** \<\<resolution-critical-1\>\>

\vspace{-0.40cm}

## High risk

\vspace{-0.25cm}

### \<\<title-high-1\>\>

\vspace{-0.25cm}

**Severity:** \textit{High risk}

**Context:** [\<\<relative-path-high-1\>\>](\<\<link-high-1\>\>)

**Description:** \<\<description-high-1\>\>

**Recommendation:** \<\<recommendation-high-1\>\>

**Resolution:** \<\<resolution-high-1\>\>

\vspace{-0.40cm}

## Medium risk

\vspace{-0.25cm}

### \<\<title-medium-1\>\>

\vspace{-0.25cm}

**Severity:** \textit{Medium risk}

**Context:** [\<\<relative-path-medium-1\>\>](\<\<link-medium-1\>\>)

**Description:** \<\<description-medium-1\>\>

**Recommendation:** \<\<recommendation-medium-1\>\>

**Resolution:** \<\<resolution-medium-1\>\>

\vspace{-0.40cm}

## Low risk

\vspace{-0.25cm}

### \<\<title-low-1\>\>

\vspace{-0.25cm}

**Severity:** \textit{Low risk}

**Context:** [\<\<relative-path-low-1\>\>](\<\<link-low-1\>\>)

**Description:** \<\<description-low-1\>\>

**Recommendation:** \<\<recommendation-low-1\>\>

**Resolution:** \<\<resolution-low-1\>\>

\vspace{-0.40cm}

## Informational

\vspace{-0.25cm}

### \<\<title-informational-1\>\>

\vspace{-0.25cm}

**Severity:** \textit{Informational}

**Context:** [\<\<relative-path-informational-1\>\>](\<\<link-informational-1\>\>)

**Description:** \<\<description-informational-1\>\>

**Recommendation:** \<\<recommendation-informational-1\>\>

**Resolution:** \<\<resolution-informational-1\>\>
