# Diagnosing Underpowered Studies and P-Hacking

## Objective
I examined how low statistical power, multiple comparisons, and extreme observations can affect the conclusions I draw from hypothesis tests.

## Methodology
- I ran Welch's t-test on a pilot study with 15 participants per group and calculated the observed standardized effect.
- I used the known population effect to calculate the pilot's power and the sample size needed for 80% power, then simulated a larger study.
- I applied the Benjamini-Hochberg (BH) correction to 20 subgroup tests.
- I simulated 1,000 sets of 20 tests with no treatment effects and compared the false-positive frequency with the theoretical probability.
- I implemented a BH function and checked its results against statsmodels and the notebook's supplied checks.
- I compared Welch's test, Yuen's trimmed t-test, and permutation tests of ordinary and trimmed means on heavy-tailed earnings data.
- I completed the optional challenge by simulating selective changes to borderline z-statistics.

## Key Findings

### Underpowered pilot study
My pilot had an observed Cohen's d of -0.146 and a p-value of 0.6925. However, the simulated population had a positive $800 treatment effect and a standard deviation of $3,000, giving a true standardized effect of about 0.267. The pilot had only 10.9% power to detect that effect, so failing to reject the null did not establish that the intervention was ineffective.

I needed 222 participants per group for 80% power. My rerun at that size produced a p-value of 0.0012 and rejected the null at the 5% level. Having 80% power still does not guarantee that every simulated study will detect the effect.

### Multiple comparisons
Subgroup 12 had the smallest raw p-value, 0.0180, but none of the 20 subgroups remained significant after BH correction. With 20 independent tests of true nulls, the theoretical chance of at least one false positive was 64.2%. My simulation produced at least one false positive in 65.8% of the 1,000 replications.

My BH function passed the supplied checks and matched statsmodels on the original subgroup results. With two small p-values planted, BH rejected 2 hypotheses, compared with 1 under Bonferroni correction.

### Heavy-tailed earnings
| Method | P-value | Decision at 5% |
|---|---:|---|
| Welch's t-test | 0.2650 | Fail to reject |
| Yuen's trimmed t-test | 0.0066 | Reject |
| Permutation test of means | 0.3496 | Fail to reject |
| Permutation test of trimmed means | 0.0056 | Reject |

I found that the tests split according to the statistic they compared. The two tests of ordinary means failed to reject, while the two tests of trimmed means rejected. Trimming reduced the influence of the extreme earners, but it also changed the quantity being compared; the trimmed mean is different from the ordinary mean.

### Optional challenge
In my simulation, p-hacking raised the share of significant results from 0.242 to 0.313, an increase of 29.2% relative to the original share. Moving borderline statistics upward created a dip below the significance threshold and a concentration above it. The simulation's random pushes did not make every borderline statistic cross the threshold.

## Files
- `lab_ch08_diagnostic.ipynb`: completed notebook with calculations, written diagnoses, checks, and charts.
- `src/power_utils.py`: the power-analysis utility supplied in the lab.
- `README.md`: my project description and findings.

## AI Use
Claude did not write any code for me, I only used it to write a project description for my data science lab.
