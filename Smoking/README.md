# Maternal Smoking and Birth Weight

![Maternal Smoking](images/smoking.jpeg)

Maternal smoking has been shown to have harmful effects on the newborn, including reduced birth weight. This [dataset](smoking.tsv) is a subset of a much larger study by the Child Health and Development Study (CHDS) that was conducted over several years.

The columns included in the subsets described in the table below

| Column | Description |
| -- | -- |
| `id` | id number |
| `date` | birth date |
| `gestation` | gestation days |
| `weight` | birth weight in ounce (999 unknown) |
| `parity` | 0=first born |
| `mom.race` | mom race |
| `mom.age` | mom age in years |
| `mom.edu` | mom eduction 0=(<8), 1=(8-<12), 2=12, 3=12+trade, 4=12+some college, 5=16, 6, 7 trade (hs unclear), 9 = unknown |
| `mom.height` | mom height in inches |
| `mom.weight` | mom prepreganncy weight in pounds |
| `dad.race` | dad race |
| `dad.age` | dad age |
| `dad.edu` | dad eduction |
| `dad.height` | dad height |
| `dad.weight` | dad weight |
| `marital` | 1=married, 2-4=seperated, divorced, widowed, 5=never married |
| `income` | total income in 2500 increements 0=under 2500, 1=2500-4999, ...., 9=15000+, 98=unknown, 99=not asked |
| `smoke` | mom smoking
| `quit.time` | how long ago quit 0=never, 1=still, 2=during preggancy, 3=1 year, 4=2 years, 5=3 years, 6=4 years, 7=5-9 years, 8=10+ years, 9=quit and don't know when, 98=unknown |
| `cigs` | number of cigs smoked a day for past and current smokers 0=never, 1=1-4, 2=5-9, 3=10-14, 4=15-19, 5=20-29, 6=30-39, 7=40-60, 8=60+, 9=smoke but don't know, 98=unknown |
