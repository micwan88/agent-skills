---
name: python-env
description: "Enforce the use of Python virtual environments (venv) for all Python script executions and dependency management within the workspace. Use this skill when you need to execute Python code or install new Python packages to ensure environment isolation and consistency."
metadata: {"openclaw":{"emoji":"🐍","os":["linux", "darwin"]}}
---

# Python Environment Management (venv)

This skill ensures that all Python-related tasks are performed within an isolated virtual environment (`venv`) located inside the current workspace.

## Workflow

### 1. Create virtual environment if it does not exist
Before any execution, check if a `venv` folder exists in the workspace root. If it is missing, create it:
```bash
python3 -m venv venv
```

### 2. Install dependencies under venv
When installing new packages, always use the `pip3` binary located inside the `venv` to ensure they are not installed globally:
```bash
venv/bin/pip3 install <package_name>
```
Or if you have a `requirements.txt`:
```bash
venv/bin/pip3 install -r requirements.txt
```

### 3. Execute Python scripts using the venv
Always invoke the Python interpreter from the `venv` directory to run your scripts:
```bash
venv/bin/python3 your_script.py
```
*Alternatively, you can activate the environment in a shell session:*
```bash
source venv/bin/activate
python3 your_script.py
```

## Best Practices
- **Persistence**: The `venv` folder stays inside your workspace, so installed packages will survive across different agent turns.
- **Isolation**: Never use the system `python3` or `pip3` directly for workspace-specific tasks.
- **Verification**: If a script fails due to missing modules, always verify if they were installed using `venv/bin/pip3 list`.

## Example Usage

**User**: "Run a script that uses the requests library."
**Agent**:
1. (Checks for `venv`) -> `ls -d venv`
2. (If missing) -> `python3 -m venv venv`
3. (Installs dependency) -> `venv/bin/pip3 install requests`
4. (Runs script) -> `venv/bin/python3 my_script.py`