# Security review report generator

A simple script that allows security researchers to convert their audit report files from MD format to more professional PDF.

## How to use

1. Make sure you have the following installed and set up:
 - [Pandoc](https://pandoc.org/)
 - [LaTeX](https://www.latex-project.org/)

2. Paste or write your report in [`report.md`](./report/report.md) following the template shown in [`template.md`](./template/template.md).

3. Run one of the following commands inside the [`/generator`](.) directory:
 - `npm start`
 - `pandoc "report.md" -o "report.pdf" --from markdown --template "./eisvogel.tex" --listings`
 
4. Enjoy your beautiful smart contract audit report located in [`report.pdf`](./report/report.pdf).

## Feature requests or issues

If you experience any problems using the generator, feel free to open a GitHub [issue](https://github.com/gogotheauditor/audits/issues/new), [PR](https://github.com/gogotheauditor/audits/pulls) or DM the author at [@gogotheauditor](https://twitter.com/gogotheauditor).