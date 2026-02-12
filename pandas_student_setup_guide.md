# Student Setup Guide - Pandas Exercises

## What You'll Need

1. **The data file**: `class_pipeline_prices_v1_aapl_ge_psct.parquet`
2. **The exercise notebook**: `pandas_exercises.ipynb`
3. **Python environment** with Pandas installed

---

## Setup Options (Choose One)

### Option 1: Google Colab (Easiest - No Installation!)

**Best for**: Students without Python installed, quick start

**Steps**:
1. Go to [Google Colab](https://colab.research.google.com/)
2. Click **File → Upload notebook**
3. Upload `pandas_exercises.ipynb`
4. Upload the data file:
   ```python
   from google.colab import files
   uploaded = files.upload()  # Click and upload the .parquet file
   ```
5. Start coding!

**Pros**: 
- No installation needed
- Works on any device with browser
- Free GPU if needed
- Auto-saves to Google Drive

**Cons**:
- Requires Google account
- Files don't persist unless saved to Drive

---

### Option 2: Jupyter Notebook (Local)

**Best for**: Students who want to work offline

**Steps**:
1. Install Python and Jupyter:
   ```bash
   pip install jupyter pandas pyarrow numpy
   ```
2. Put both files in the same folder
3. Open terminal/command prompt in that folder
4. Run:
   ```bash
   jupyter notebook
   ```
5. Click on `pandas_exercises.ipynb` in the browser

**Pros**:
- Works offline
- Full control
- Faster than cloud options

**Cons**:
- Requires installation
- Setup time

---

### Option 3: JupyterLab (Modern Interface)

**Best for**: Students who want a better IDE experience

**Steps**:
1. Install:
   ```bash
   pip install jupyterlab pandas pyarrow numpy
   ```
2. Put files in same folder
3. Run:
   ```bash
   jupyter lab
   ```
4. Open the notebook

**Pros**:
- Better UI than Jupyter Notebook
- Multiple panels
- Built-in file browser

**Cons**:
- Slightly more complex than Notebook

---

### Option 4: VS Code (Most Professional)

**Best for**: Students familiar with coding editors

**Steps**:
1. Install [VS Code](https://code.visualstudio.com/)
2. Install Python extension in VS Code
3. Install Jupyter extension in VS Code
4. Install packages:
   ```bash
   pip install pandas pyarrow numpy ipykernel
   ```
5. Open the `.ipynb` file in VS Code
6. Select Python kernel when prompted

**Pros**:
- Professional development environment
- Great debugging tools
- Git integration

**Cons**:
- More features = steeper learning curve

---

### Option 5: Anaconda (Complete Package)

**Best for**: Students who want everything pre-installed

**Steps**:
1. Download [Anaconda](https://www.anaconda.com/download)
2. Install (includes Jupyter, Pandas, NumPy automatically)
3. Open Anaconda Navigator
4. Launch Jupyter Notebook or JupyterLab
5. Navigate to your files

**Pros**:
- Everything included
- Package management with conda
- No dependency issues

**Cons**:
- Large download (~500MB+)
- Can be slow on older computers

---

##  Quick Test

After setup, run this in a cell to verify everything works:

```python
import pandas as pd
import numpy as np

# Check versions
print(f"Pandas version: {pd.__version__}")
print(f"NumPy version: {np.__version__}")

# Try loading the data
df = pd.read_parquet('class_pipeline_prices_v1_aapl_ge_psct.parquet')
print(f"\n✓ Data loaded successfully!")
print(f"Shape: {df.shape}")
print(f"Columns: {df.columns.tolist()}")
```

If you see output without errors, you're ready to go! 

---

## Troubleshooting

### Error: "No module named 'pandas'"
```bash
pip install pandas
```

### Error: "No module named 'pyarrow'"
```bash
pip install pyarrow
```

### Error: "FileNotFoundError" when loading data
- Make sure the `.parquet` file is in the **same folder** as the notebook
- Or use full path: `pd.read_parquet('/full/path/to/file.parquet')`

### Jupyter not opening in browser
- Try: `jupyter notebook --no-browser` then copy the URL shown
- Or specify port: `jupyter notebook --port=8889`

### Can't run cells in VS Code
- Make sure you selected a Python kernel (top right of notebook)
- Install ipykernel: `pip install ipykernel`

---

## Tips for Success

### Keyboard Shortcuts (Jupyter)
- **Run cell**: `Shift + Enter`
- **Insert cell below**: `B`
- **Insert cell above**: `A`
- **Delete cell**: `DD` (press D twice)
- **Change to Markdown**: `M`
- **Change to Code**: `Y`
- **Save**: `Ctrl/Cmd + S`

### Best Practices
1. **Run Setup First**: Always run the first cell to load the data
2. **Run in Order**: Execute cells from top to bottom
3. **Save Often**: Hit save after completing each section
4. **Experiment**: Try different approaches!
5. **Check Output**: Make sure each cell produces expected output
6. **Use Comments**: Add `# comments` to explain your thinking

### Getting Help
- **Pandas Documentation**: https://pandas.pydata.org/docs/
- **Check built-in help**: `help(df.groupby)` or `df.groupby?`
- **Google is your friend**: "pandas how to [what you want to do]"


---

##  Alternative: Use the Markdown File

If you can't get Jupyter working, you can:

1. Use the `pandas_exercises.md` file instead
2. Write code in any Python editor (IDLE, PyCharm, Spyder, etc.)
3. Copy exercises one by one and run them in a Python script

**Example**:
```python
# save as: exercise1.py
import pandas as pd

df = pd.read_parquet('class_pipeline_prices_v1_aapl_ge_psct.parquet')

# Exercise 1.1 - Task 1
print(df.head())

# Exercise 1.1 - Task 2
print(df.tail(10))

# ... etc
```

Run with: `python exercise1.py`

---

Remember: The best way to learn Pandas is by doing. Don't just read the exercises—type them out and experiment!
