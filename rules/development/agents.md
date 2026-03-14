---
trigger: always_on
---

# Agent Rules

- Please understand the existing code from source file before make any changes (**Don't based on memory**)
- After update any source file, please review all lines again included comments just double confirm if anything wrong

# Python Guide

- **Virtual Environment**: All Python execution MUST use the virtual environment located at `.venv` in the project root.
   - Python: `.venv\bin\python`
   - Install packages: `.venv\bin\pip install ...`
- Setup the logger in the early main process if any

# Coding Guide

- Be a professional developer. Make things be modularized and try to make things to be reusable
- Try make 'hardcode' value to be declare as constants rather just hardcode inline or even make it configurable
- Try avoid inner function (except lambda). Just want to make everything is reusable
- Please don't declare inline import statement, should declare at the top
- Please declare constants at the top if any
- Please use logger instead of 'print' / 'sysout' statement