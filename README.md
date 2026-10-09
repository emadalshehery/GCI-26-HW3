# Homework 3: Visualizing Data Using Matplotlib

Pareto (decile) analysis on the [MyAnimeList dataset](https://github.com/Hernan4444/MyAnimeList-Database) using pandas and Matplotlib.

**Deadline:** Thu, Oct 22, 2026 (11:00 UTC), submitted via Omnicampus.

## Task

Implement `homework(anime_data, metric_column, n)`, which:

1. Ranks anime by `metric_column` in descending order.
2. Splits them into `n` equal-sized groups with `pd.qcut()` on the rank (`method='first'`, so ties don't break grouping).
3. Computes each group's share of the total of `metric_column`.
4. Sorts the shares in descending order and labels them `Group 1` ... `Group n`.

Returns a `pd.Series` whose values sum to 1.0.

| Argument | Type | Description |
|---|---|---|
| `anime_data` | `pd.DataFrame` | Preprocessed anime dataset |
| `metric_column` | `str` | Column to analyze, e.g. `'Completed'`, `'Members'` |
| `n` | `int` | Number of groups |

## Solution

```python
def homework(anime_data, metric_column, n):
    values = anime_data[metric_column]
    rank = values.rank(method='first', ascending=False)
    group = pd.qcut(rank, q=n, labels=False)

    result = values.groupby(group).sum() / values.sum()
    result = result.sort_values(ascending=False)
    result.index = [f"Group {i}" for i in range(1, n + 1)]
    return result
```

## Usage

```python
result = homework(anime_data, 'Completed', 5)
print(result)
```

```
Group 1    0.94XXXX
Group 2    0.04XXXX
Group 3    0.009XXX
Group 4    0.001XXX
Group 5    0.0004XX
dtype: float64
```

## Test Cases

| Case | Input | Expected |
|---|---|---|
| 1 | Full dataset, `'Completed'`, `n=10` | 10 groups, descending, sum ≈ 1.0 |
| 2 | `[40, 30, 20, 10]`, `n=2` | `0.7`, `0.3` |
| 3 | `[60, 50, 40, 30, 20, 10]`, `n=3` | `0.5238`, `0.3333`, `0.1429` |

## Notebook Structure

1. **Preparation** – imports, dataset download, preprocessing (do not modify).
2. **Your Answer** – the `homework()` function.
3. **Test Your Answer** – the three test cases above.
4. **Optional visualization** – Pareto chart: bars for per-group proportion, line for cumulative proportion, with an 80% reference line.

## Submission Notes

- Submit only the `homework()` function (from `def` through `return`).
- No `import` statements, downloads, or data-loading code inside the function.
- Re-run the function cell after any edit.
- Your latest submission is the final grade: 3.0 on time, max 2.0 if late.

## Requirements

`numpy`, `pandas`, `matplotlib`, `requests`
