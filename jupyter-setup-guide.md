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

## 4. Start Jupyter Notebook

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

## Best Practices

1. Always use virtual environments for different projects
2. Keep your Python and Jupyter Notebook updated
3. Name your virtual environments meaningfully for different projects
4. Save requirements.txt for your projects:
```bash
pip freeze > requirements.txt
```

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
