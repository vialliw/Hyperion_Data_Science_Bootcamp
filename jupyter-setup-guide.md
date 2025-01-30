# Setting Up Jupyter Notebook in Windows

This guide walks through the process of setting up and running Jupyter Notebook in Windows, including virtual environment setup.

## Prerequisites

Before starting, ensure you have administrative access to your Windows machine.

## 1. Install Python

1. Visit [python.org](https://www.python.org/downloads/)
2. Download the latest Python version for Windows
3. Run the installer
4. **Important**: Check the box that says "Add Python to PATH"
5. Click "Install Now"

## 2. Set Up Virtual Environment

Open Command Prompt and follow these steps:

```bash
# Navigate to your project folder
cd your_project_folder

# Create a virtual environment
python -m venv myenv

# Activate the virtual environment
myenv\Scripts\activate
```

You should see `(myenv)` appear at the beginning of your command prompt line, indicating the virtual environment is active.

## 3. Install Jupyter Notebook

With your virtual environment activated:

```bash
# Install Jupyter Notebook
pip install notebook
```

## 4. Install Required Packages

Before starting your project, install any additional Python packages you'll need:

```bash
# Install common data science packages
pip install numpy pandas matplotlib seaborn scikit-learn fuzzywuzzy chardet python-Levenshtein mlxtend missingno

# Install specific version of a package
pip install tensorflow==2.8.0

# Install multiple packages using requirements.txt
pip install -r requirements.txt
```

### Creating requirements.txt

There are two ways to create a requirements.txt file:

1. Manually create and edit:
```bash
# Create empty requirements.txt
echo numpy==1.21.0 > requirements.txt
echo pandas==1.3.0 >> requirements.txt
```

2. Automatically from your environment:
```bash
# Export all installed packages
pip freeze > requirements.txt
```

### Best Practices for Package Management

1. Install packages only after activating your virtual environment
2. Document package versions in requirements.txt
3. Update packages carefully to avoid compatibility issues
4. Consider using separate requirements files for development and production

## 5. Start Jupyter Notebook

```bash
# Launch Jupyter Notebook
jupyter notebook
```

This will automatically:
- Start the Jupyter Notebook server
- Open your default web browser to http://localhost:8888
- Display the Jupyter file navigator

## Common Tasks

### Creating a New Notebook
1. Click the "New" button in the top right
2. Select "Python 3" from the dropdown menu

### Stopping Jupyter Notebook
1. Return to Command Prompt
2. Press `Ctrl+C`
3. Confirm by typing `y`

### Managing Virtual Environment
```bash
# Deactivate virtual environment when done
deactivate

# Reactivate virtual environment later
myenv\Scripts\activate
```

## Troubleshooting

If you encounter "jupyter not recognized":
1. Ensure your virtual environment is activated
2. Try reinstalling with: `pip install --upgrade notebook`

If you can't create a virtual environment:
1. Verify Python is in PATH: `python --version`
2. Try running Command Prompt as administrator

If packages fail to install:
1. Check your internet connection
2. Verify pip is updated: `python -m pip install --upgrade pip`
3. Look for package conflicts in requirements.txt
4. Try installing packages one by one to identify issues

## Best Practices

1. Always use virtual environments for different projects
2. Keep your Python and Jupyter Notebook updated
3. Name your virtual environments meaningfully for different projects
4. Regularly update your requirements.txt as you add new packages
5. Test your environment with a new requirements.txt installation before deployment

## Appendix: Command Syntax

Jupyter commands can be written in two equivalent ways:

1. Space-separated syntax (canonical form):
```bash
jupyter notebook   # Start Jupyter Notebook
jupyter lab       # Start JupyterLab
jupyter console   # Start Jupyter Console
```

2. Hyphenated syntax (alternative form):
```bash
jupyter-notebook  # Same as jupyter notebook
jupyter-lab      # Same as jupyter lab
jupyter-console  # Same as jupyter console
```

Both forms are functionally identical:
- The space-separated version is the official syntax and most commonly used in documentation
- The hyphenated version exists for compatibility and convenience, especially in scripts where spaces might cause issues
- You can use either syntax based on your preference - they both execute the same command
