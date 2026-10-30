# Reflection — Lab 5: Static Code Analysis

**1. Which issues were the easiest to fix, and which were the hardest? Why?**

The easiest were the Flake8 style issues: the unused `math` import, the unused
`booking_fee` variable, `== None` → `is None`, and the missing final newline.
Each was a one-line change with an obvious fix.

The hardest was replacing `eval()`. Removing it meant changing the function's
interface: it now takes a numeric `discount_percent` instead of a code string,
so I also had to update the call in `main()` and add range validation, while
making sure the promo price was still 16.0. The mutable default argument was
also tricky to *understand*: the code looked fine, but the `[]` default is
created once and shared across every call, so order history leaked between
customers.

**2. Did the static analysis tools report any false positives? If so, give an example.**

Bandit's B311 warning about `random` is borderline. `random` is fine for things
like games or simulations, and Bandit flags it regardless of what it's used for.
In this case it was generating a booking PIN, which is security-sensitive, so I
still switched to `secrets`, but the tool can't tell the difference by itself.

More important was a false *negative*: none of the three tools flagged the
hardcoded `BOX_OFFICE_SECRET_PASS_2026` being printed to the console. Bandit
only detects hardcoded secrets in variables with names like `password`. I found
it by reading the code and fixed it by reading the key from an environment
variable. This showed me that static analysis supports manual review but
doesn't replace it.

**3. How would you integrate static analysis tools into your actual software development workflow?**

Locally, I would run Flake8 and Pylint through a pre-commit hook (a script that
runs automatically before every commit) so style and quality issues are caught
before code is saved. In a team, I would add all three tools to a CI pipeline
(e.g. GitHub Actions) that runs on every pull request and blocks merging if
Bandit finds a medium/high severity issue or the Pylint score drops below a
threshold like 9.5. I would also enable linting in my editor (VS Code) so
problems show up while typing.

**4. What tangible improvements did you observe in code quality, readability, or robustness after the fixes?**

- Pylint score went from 5.37/10 to 10.00/10; Bandit issues from 5 (2 High,
  1 Medium, 2 Low) to 0; Flake8 issues from 7 to 0.
- Security: no more arbitrary code execution (`eval`), shell injection
  (`os.system`), weak hashing (MD5), or exposed secrets.
- Robustness: the file is always closed thanks to `with`, cancelling a
  non-existent seat now reports an error instead of failing silently, and the
  shared-list bug is gone.
- Readability: every function has a docstring, names follow snake_case, and
  `check_seat_status` and `get_theater_summary` are shorter and clearer.
- The program's output stayed the same, so the fixes didn't break anything.