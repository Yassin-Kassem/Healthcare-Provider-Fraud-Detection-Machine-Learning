# Installing Packages for Jupyter Notebooks

## Problem
You're getting `ModuleNotFoundError: No module named 'xgboost'` even though xgboost is installed.

## Root Cause
Your Jupyter notebook kernel is using a different Python environment than your command-line Python.

## Solution

### Option 1: Install from Within Jupyter (RECOMMENDED)
Add this cell **at the very beginning** of your notebook (before all imports):

```python
import sys
!{sys.executable} -m pip install xgboost
```

Then restart the kernel and run all cells.

### Option 2: Install All Requirements from Jupyter
Add this cell at the beginning of your notebook:

```python
import sys
!{sys.executable} -m pip install -r ../requirements.txt
```

### Option 3: Verify Installation
Check which Python your Jupyter is using:

```python
import sys
print(f"Python executable: {sys.executable}")
print(f"Python version: {sys.version}")

# Try importing xgboost
try:
    import xgboost
    print(f"✓ XGBoost version: {xgboost.__version__}")
except ImportError as e:
    print(f"✗ XGBoost not found: {e}")
```

## Why This Happens
- You might have multiple Python installations
- Jupyter might be using a different virtual environment
- The PATH might point to a different Python than Jupyter uses

## Permanent Fix
After installing from within Jupyter, the package will remain installed for that kernel.
