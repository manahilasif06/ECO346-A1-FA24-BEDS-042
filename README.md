# ECO346 Assignment 1: FA24-BEDS-042

**Variant 2: Sindh rural** (042 = 5 x 8 + 2). Data: PSLM 2019-20 person file, with `hhweight`, `prov_code` and `urban_rural` attached from the household file by `hhid`. All statistics are weighted by `hhweight`.

## Figures

| File | What it is |
|---|---|
| `figures/default.svg` | Question 4: R's default `ggplot(data, aes(age, literate)) + geom_point()` |
| `figures/distribution.svg` | Question 2: weighted histogram of annual education spending (log scale) |
| `figures/literacy_by_age.svg` | Question 3: weighted literacy by age band, women and men (the redesign of the default) |
| `figures/literacy_by_age_python.svg` | Question 5: the same figure drawn in Python |

![distribution](figures/distribution.svg)
![literacy](figures/literacy_by_age.svg)

## Question 2: the distribution

Students: 1,440. Weighted median Rs 4,500; unweighted median Rs 4,295. Weighted 10th percentile Rs 1,000; 90th percentile Rs 15,500; ratio 15.50. Excluded from the histogram (outside Rs 100 to Rs 1,000,000): 1.

Spending is concentrated in the low thousands of rupees with a long right tail: the middle 80% of students lie between Rs 1,000 and Rs 15,500, a 15.5-fold spread. The weighted median (Rs 4,500) is about 5% above the unweighted (Rs 4,295) because households sampled at higher rates, which carry small weights, spend somewhat less (median Rs 4,000 in the lowest-weight third). Weighting undoes that over-representation, but the gap is small.

## Question 3: the band table

Literacy sample: 7,171 persons aged 10-64 with non-missing `literate`. `n` is the unweighted count; rates are weighted percentages. No cell has fewer than 30 observations (the smallest is 77), so no hollow markers appear.

| Band | Women n | Women literate % | Men n | Men literate % | Gap (points) |
|---|---|---|---|---|---|
| 10-14 | 529 | 35.5 | 718 | 57.6 | 22.1 |
| 15-19 | 446 | 28.0 | 548 | 59.5 | 31.5 |
| 20-24 | 455 | 23.4 | 422 | 58.5 | 35.1 |
| 25-29 | 430 | 24.6 | 419 | 53.4 | 28.9 |
| 30-34 | 399 | 17.7 | 382 | 60.2 | 42.5 |
| 35-39 | 319 | 19.5 | 391 | 55.5 | 36.0 |
| 40-44 | 285 | 13.2 | 274 | 51.2 | 38.0 |
| 45-49 | 198 | 9.1 | 221 | 49.9 | 40.8 |
| 50-54 | 152 | 7.2 | 208 | 42.4 | 35.3 |
| 55-59 | 98 | 9.6 | 100 | 40.0 | 30.4 |
| 60-64 | 77 | 6.8 | 100 | 36.4 | 29.6 |

Women aged 15-19: 28.0%; men 15-19: 59.5%. Women aged 55-59: 9.6%; men 55-59: 40.0%. Largest gap: 30-34 (42.5 points).

Men are far more literate than women in every band, and the gap barely narrows with youth: 31.5 points at 15-19 against 30.4 at 55-59, peaking at 42.5 at 30-34. Women's literacy falls from 28% to under 10% across the age range. Because this is one cross-section, older bands are older birth cohorts, so the slope mixes age and generation. The figure cannot tell us why: scarce girls' schools (supply) and household decisions not to send girls (demand) produce the same picture.

## Question 4: the makeover

Three changes. (1) Conditional means: the default draws 7,171 binary points on two rows that overplot completely (Chapter 4, overplotting); I plot the weighted mean of literate in five-year bands, E[literate | age], read by position on a common scale (Chapter 1, Cleveland-McGill hierarchy). (2) Women and men are separate lines labelled at their ends with no legend (Gestalt proximity, data-ink). (3) hhweight makes the rates describe rural Sindh, not the sample (Chapter 3, whose distribution), and hollow markers flag thin cells; the title states the finding. The default shows what the redesign hides: individual observations at single years of age, so the spread inside each band; the redesign shows only means.

## Question 5: a second language (Python)

Women aged 15-19: 28.0% in R and 28.0% in Python. `tables/band_table_R.csv` and `tables/band_table_python.csv` agree to one decimal place; `scripts/04_second_language.py` prints the comparison.

ggplot2 pads a scale's limits by 5% on each side, so limits of 0-100 display as about -5 to 105; matplotlib's set_ylim(0, 100) adds no padding, so the Python figure looked tighter. I set ylim(-5, 105) and xlim(10, 70) explicitly to match R. I also built bands with floor(age/5)*5 in both languages rather than cut(), whose default closes intervals on the right.

## How to reproduce

Run every command from the repository root. No internet connection is needed; the scripts read the local copies in `data/`.

```
Rscript scripts/01_default.R
Rscript scripts/02_distribution.R
Rscript scripts/03_literacy.R
python scripts/04_second_language.py
```

R packages: ggplot2, svglite, dplyr. Python packages: pandas, numpy, matplotlib.
