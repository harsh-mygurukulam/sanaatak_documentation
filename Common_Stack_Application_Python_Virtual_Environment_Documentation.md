
# 🐍 SOP: Python Virtual Environment Setup

**Common Stack** | **Application** | **Python**  
**Component:** Virtual Environment Setup & Best Practices

---

## 📝 Description

This SOP explains **why** virtual environments are important in Python, **how to set them up**, and **best practices** for using them. It is written for absolute beginners and assumes no prior knowledge of Python environment management.

---

## ✅ Acceptance Criteria

This document must include and clearly explain:
- Why virtual environments are important in Python projects
- How to install and use `venv` for setting up virtual environments
- Best practices for managing Python virtual environments in real-world applications

---

## ❓ Why Use a Virtual Environment?

A virtual environment is a self-contained directory that contains a Python installation and specific packages for a project.

### 💡 Benefits:
- Avoids conflicts between dependencies of different projects
- Keeps your system Python clean and untouched
- Helps in replicating the exact same environment across machines
- Essential for deployment and production systems

---

## ⚙️ How to Set Up a Virtual Environment

### 1. Install Python (if not already installed)

Ubuntu/Debian:
```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip -y
```

### 2. Create a Virtual Environment

Navigate to your project folder or create a new one:
```bash
mkdir myproject && cd myproject
```

Create a virtual environment:
```bash
python3 -m venv venv
```
This will create a folder named `venv` with the isolated environment.

### 3. Activate the Virtual Environment

**On Linux/macOS:**
```bash
source venv/bin/activate
```

**On Windows (Command Prompt):**
```cmd
venv\Scripts\activate
```

Once activated, your terminal prompt will change to show `(venv)`.

### 4. Install Packages Inside the Virtual Environment

```bash
pip install <package-name>
```

_Example:_
```bash
pip install flask
```

### 5. Deactivate the Virtual Environment

To exit the virtual environment:
```bash
deactivate
```

---

## 🧰 Best Practices

- 🔒 Always create a virtual environment per project
- 📄 Use `requirements.txt` to track dependencies:
  ```bash
  pip freeze > requirements.txt
  ```
- 🔁 Reinstall dependencies easily:
  ```bash
  pip install -r requirements.txt
  ```
- 📂 Add `venv/` to your `.gitignore` to avoid pushing it to version control
- 📦 Prefer using `pip` for lightweight needs or `pipenv/poetry` for advanced dependency management

---

## 💡 Pro Tip: Alias for Activation

Add this to your `.bashrc` or `.zshrc` to simplify activation:

```bash
alias activate_myenv="source ~/path/to/venv/bin/activate"
```

---

**End of SOP**
