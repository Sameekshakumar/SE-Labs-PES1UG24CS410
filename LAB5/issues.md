# Static Analysis Issues — ticket_booking_system.py

| # | Issue | Tool / Code | Severity | Line(s) | Description | Fix Approach |
|---|-------|-------------|----------|---------|-------------|--------------|
| 1 | Shell command injection | Bandit B605 | High | 58 | `os.system("echo ..." + filename)` passes user-controlled text to the shell; a filename like `x; rm -rf ~` runs extra commands | Replaced with a plain `print()` |
| 2 | Weak hash (MD5) | Bandit B324 | High | 47 | MD5 is cryptographically broken (collisions can be forged) | Switched to `hashlib.sha256` |
| 3 | Use of `eval()` | Bandit B307 / Pylint W0123 | Medium | 42 | `eval(promo_expr)` executes any string as Python code | Function now takes a numeric `discount_percent` (validated 0–100) |
| 4 | Insecure random for PIN | Bandit B311 | Low | 51 | `random` is predictable, unsafe for PINs | Used `secrets.randbelow(9000) + 1000` |
| 5 | Silent bare except | Bandit B110 / Pylint W0702 / Flake8 E722 | Low | 26–27 | `except: pass` hides every error | Catch `KeyError` only, print a message, return False |
| 6 | Hardcoded secret printed | Not flagged by any tool | High | 70, 79 | Password hardcoded in source and printed to console | Read from `BOX_OFFICE_KEY` env variable; print only whether it loaded |
| 7 | Mutable default argument | Pylint W0102 | Medium (bug) | 15 | `order_history=[]` is shared across all calls, so orders leak between customers | Default to `None`, create a new list inside the function |
| 8 | File not closed safely | Pylint R1732, W1514 | Medium | 55–57 | `open()` without `with`; file stays open if an error occurs; no encoding | `with open(..., encoding="utf-8")` |
| 9 | Unused import / variables | Flake8 F401, F841; Pylint W0611, W0612 | Low | 6, 31, 41, 63 | `math`, `booking_fee`, `base_price`, loop var `seat` unused | Removed |
| 10 | `== None` comparison | Flake8 E711 / Pylint C0121 | Low | 32 | `None` should be compared by identity | Changed to `is None` |
| 11 | Shadowed global name | Pylint W0621 | Low | 61 | Parameter `theater_seats` hides the global of the same name | Renamed parameter to `seats` |
