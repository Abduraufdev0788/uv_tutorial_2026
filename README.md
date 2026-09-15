# Ledger

A small income and spending tracker, used to learn [pytest](https://docs.pytest.org/).

The project is the **test suite**, not the app. The code stays small on purpose so
that every feature exists to force one new pytest concept.

## Features

1. **Add income** — amount, category, date
2. **Add spending** — same, stored as a negative amount
3. **Show balance** — income minus spending
4. **Total by category** — e.g. `{"food": -120, "salary": 3000}`
5. **Spending this month** — depends on today's date

That's it. One class, about 50 lines.

## What each feature teaches

| Feature | pytest concept |
| --- | --- |
| 1 — add income | plain `assert`, running `pytest -v` |
| 2 — add spending | `pytest.raises` (reject `0`, empty category) |
| 3 — balance | fixtures, then `pytest.approx` once floats break |
| 4 — total by category | `@pytest.mark.parametrize` |
| 5 — spending this month | `monkeypatch` the clock |

Step 3 is the one to build the lesson around: `0.1 + 0.2 != 0.3` failing in a test
about your own grocery budget is far more memorable than an abstract float example.
It leads straight into storing money as integer cents.

## Out of scope

Budgets, recurring bills, editing or deleting entries, multi-currency, charts, a CLI,
a database. Each adds code without adding a new pytest concept. If `ledger.py` passes
60 lines, the lesson has drifted.

Optional, only if there is time: CSV export and import, which is the natural home for
the `tmp_path` fixture.

## Layout

```
pyproject.toml
ledger.py
tests/
    test_ledger.py
```

## Running the tests

```sh
uv sync
uv run pytest
```

### A known trap

With the tests in `tests/`, `import ledger` fails — pytest puts the directory
containing the test file on `sys.path`, which is `tests/`, not the project root.
One line in `pyproject.toml` fixes it:

```toml
[tool.pytest.ini_options]
pythonpath = ["."]
```

Letting students hit this error on purpose is a good lesson in itself. It is the
moment `src/` layouts and editable installs start to make sense.
