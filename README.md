# Python Project Template

A reproducible Python project environment using **Micromamba**, **direnv**, **Visual Studio Code**, and **Jupyter**.

This template provides an isolated environment for each project, with automatic activation and a consistent development setup.

The goal is to make projects easy to create, maintain, and reproduce on another computer.

## 1. What's included?

- Python 3.12
- Project-specific Micromamba environment
- Automatic environment activation with direnv
- NumPy, pandas, and Matplotlib
- Jupyter Notebook support
- Python linting and formatting with Ruff
- VS Code project settings
- Reproducible dependency installation through `environment.yml`

## 2. Create a new project

### 2.1 Create a repository from this template

On GitHub:

1. Open this template repository.
2. Click **Use this template → Create a new repository**.
3. Choose a name for your new project.
4. Choose the repository visibility.
5. Create the repository.

### 2.2 Clone the repository

Open a terminal:

```bash
cd ~/Projects
git clone <REPOSITORY_URL>
cd <PROJECT_NAME>
```

Replace `<REPOSITORY_URL>` with your new repository's URL.

### 2.3 Create the project environment

The dependencies are defined in `environment.yml`.

Create a Micromamba environment named after the project directory:

```bash
micromamba create -f environment.yml -n "$(basename "$PWD")"
```

This installs Python and all packages specified in the environment definition.

### 2.4 Enable automatic environment activation

The project contains a `.envrc` file that configures automatic activation.

Allow direnv to load it:

```bash
direnv allow
```

Whenever you enter the project directory, direnv automatically activates the corresponding Micromamba environment.

When you leave the directory, the environment is deactivated.

Verify the active environment:

```bash
echo "$CONDA_DEFAULT_ENV"
```

Check the Python executable:

```bash
which python
```

It should point to:

```text
~/micromamba/envs/<PROJECT_NAME>/bin/python
```

### 2.5 Open the project in VS Code

```bash
code .
```

VS Code should detect the Python environment.

If necessary:

1. Open the Command Palette.
2. Select **Python: Select Interpreter**.
3. Choose the interpreter inside the project environment:

```text
~/micromamba/envs/<PROJECT_NAME>/bin/python
```

## 3. Python development

### 3.1 Project structure

A typical project can follow this structure:

```text
PROJECT_NAME/
│
├── .git/
├── .vscode/
│   └── settings.json
│
├── .envrc
├── .gitignore
├── environment.yml
├── README.md
│
├── data/
├── notebooks/
├── scripts/
└── results/
```

The `data`, `notebooks`, `scripts`, and `results` directories are optional and can be created as needed.

### 3.2 Python scripts

Create Python scripts in the project directory or in `scripts/`.

For example:

```text
scripts/
└── analysis.py
```

Run a script from the activated project environment:

```bash
python scripts/analysis.py
```

### 3.3 Jupyter Notebooks

The environment includes `ipykernel`.

VS Code should be able to discover the project environment as a Jupyter kernel.

If necessary, register it manually:

```bash
python -m ipykernel install --user \
  --name "$(basename "$PWD")" \
  --display-name "Python ($(basename "$PWD"))"
```

Open a notebook (`.ipynb`) in VS Code and select the corresponding Python kernel.

### 3.4 Code formatting and linting with Ruff

Ruff is used for Python code quality and formatting.

The shared VS Code settings enable:

- Formatting on save
- Ruff linting
- Ruff formatting
- Ruff quick fixes

You can also run Ruff manually.

Check for linting issues:

```bash
ruff check .
```

Format Python files:

```bash
ruff format .
```

## 4. Managing dependencies

The file `environment.yml` is the central record of the project's dependencies.

Whenever you add a package, update this file so the environment can be reproduced on another computer.

For example, add a Python package under `dependencies`:

```yaml
dependencies:
  - python=3.12
  - pip
  - ipykernel
  - numpy
  - pandas
  - matplotlib
  - ruff
  - scipy
```

Update the existing environment:

```bash
micromamba env update -f environment.yml -n "$(basename "$PWD")"
```

If you need to recreate the environment from scratch:

```bash
micromamba env remove -n "$(basename "$PWD")"
micromamba create -f environment.yml -n "$(basename "$PWD")"
```

After recreating it, select the interpreter and Jupyter kernel again in VS Code if necessary.

## 5. Daily workflow

### Starting work

Enter the project directory:

```bash
cd ~/Projects/<PROJECT_NAME>
```

The environment activates automatically.

Open VS Code:

```bash
code .
```

### Working with Python

Use the activated environment to run scripts, work with notebooks, and install or manage dependencies.

### Leaving the project

Simply leave the directory:

```bash
cd ..
```

direnv automatically deactivates the project environment.

## 6. Git and version control

The project uses Git for version control.

The `.gitignore` file excludes temporary and local development files, including:

- Python cache files
- Ruff cache
- Jupyter checkpoints
- Operating-system metadata

The Micromamba environment itself is not stored in Git. Instead, `environment.yml` records the dependencies needed to recreate it.

Commit changes to `environment.yml` whenever project dependencies are modified.

---

# 7. Installing prerequisites on a new computer

The following instructions describe how to prepare a new Ubuntu computer to use this template.

## 7.1 System requirements

The setup assumes:

- Ubuntu Linux (64-bit)
- Bash shell
- Git
- Micromamba
- direnv
- Visual Studio Code

## 7.2 Install basic utilities and Git

Update Ubuntu:

```bash
sudo apt update
sudo apt upgrade
```

Install Git and basic utilities:

```bash
sudo apt install git curl wget unzip build-essential
```

Verify Git:

```bash
git --version
```

## 7.3 Install Micromamba

Install Micromamba:

```bash
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)
```

Restart the terminal or reload the Bash configuration.

Verify:

```bash
micromamba --version
```

Configure the Micromamba root directory:

```bash
micromamba config set root_prefix ~/micromamba
```

Create the environment directory:

```bash
mkdir -p ~/micromamba/envs
```

Configure conda-forge as the package channel:

```bash
micromamba config append channels conda-forge
micromamba config set channel_priority strict
```

Disable automatic activation of the base environment:

```bash
micromamba config set auto_activate_base false
```

Initialize Micromamba for Bash:

```bash
micromamba shell init --shell bash --root-prefix ~/micromamba
```

Restart the terminal.

## 7.4 Install direnv

Install direnv:

```bash
sudo apt install direnv
```

Add the Bash hook to `~/.bashrc`:

```bash
echo 'eval "$(direnv hook bash)"' >> ~/.bashrc
```

Configure the Micromamba environment directory:

```bash
echo 'export MAMBA_ENVS_DIR="$HOME/micromamba/envs"' >> ~/.bashrc
```

Reload the configuration:

```bash
source ~/.bashrc
```

Verify:

```bash
direnv version
echo "$MAMBA_ENVS_DIR"
```

## 7.5 Install Visual Studio Code

Install Visual Studio Code using the preferred installation method for your Ubuntu system.

Verify:

```bash
code --version
```

## 7.6 Install VS Code extensions

The following extensions are recommended.

**Python development:**

- Python (`ms-python.python`)
- Pylance (`ms-python.vscode-pylance`)
- Jupyter (`ms-toolsai.jupyter`)
- Ruff (`charliermarsh.ruff`)

They can be installed through the VS Code Extensions panel.

## 8. Reproducing a project on another computer

To reproduce a project on another Ubuntu computer:

1. Install Git, Micromamba, direnv, and VS Code.
2. Configure Bash as described above.
3. Clone the project repository.
4. Enter the project directory.
5. Create the environment using `environment.yml`.
6. Run `direnv allow`.
7. Open the project in VS Code.
8. Select the Python interpreter and Jupyter kernel if necessary.

The project environment can then be recreated without copying the original computer's installed packages.

---

**Environment management:** Micromamba  
**Automatic activation:** direnv  
**Editor:** Visual Studio Code  
**Notebook support:** Jupyter  
**Linting and formatting:** Ruff