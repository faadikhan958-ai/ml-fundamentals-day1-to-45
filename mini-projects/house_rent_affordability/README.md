# House Rent Affordability

**Ticket:** A property website wants to tell users whether a home is affordable *relative to their income*, not just by the rent amount.

## Idea
A common rule of thumb says rent should not exceed about 30% of income.

- `rent_to_income = monthly_rent / monthly_income`
- `affordable = 1` if `rent_to_income <= 0.30`, otherwise `0`

No model is needed: the engineered ratio *is* the business rule. It shows that a well-chosen feature can solve a problem without machine learning.

## Sanity-check examples
| Income | Rent | rent_to_income | Affordable? |
|---|---|---|---|
| 15,000 | 4,000 | 0.27 | Yes |
| 45,000 | 24,000 | 0.53 | No |
| 8,000 | 20,000 | 2.50 | No |

If the output disagrees with this table, the ratio is probably inverted (income divided by rent instead of rent divided by income).

## Versions
- v1: computes the ratio and the `affordable` flag with pandas
- v2: loops over the rows and prints a readable verdict for each

## Limitations
- The 30% rule is a rough guideline; the right threshold varies by city and household.
- Tiny hand-made dataset: it illustrates the idea, not a statistical result.
