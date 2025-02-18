# **Python Installation, Environments, and Version Management**  

### **Python Installation**  
Python comes pre-installed on macOS. To check the version, run:  
```sh
python --version
```
For Python 3, use:  
```sh
python3 --version
```

### **Managing Python Environments**  
Python environments help isolate dependencies for different projects, avoiding conflicts between libraries.  

#### **Global Python Environment vs. Virtual Environments**  
- **Global Environment**: The default Python installation, shared across all projects. Installing packages here affects all projects.  
- **Virtual Environments**: Project-specific, isolated environments with their own dependencies.  

### **Creating a Virtual Environment using `venv`**  
Python’s built-in `venv` module helps create virtual environments.  

1. **Create a Virtual Environment:**  
   ```sh
   python3 -m venv myenv
   ```
2. **Activate the Virtual Environment:**  
   ```sh
   source myenv/bin/activate
   ```
3. **Deactivate the Virtual Environment:**  
   ```sh
   deactivate
   ```

### **Using `virtualenv`**  
`virtualenv` is an older tool but still widely used.  

1. Install `virtualenv`:  
   ```sh
   pip install virtualenv
   ```
2. Create a Virtual Environment:  
   ```sh
   virtualenv myenv
   ```

### **Using `pyenv` for Python Version Management**  
`pyenv` allows you to manage multiple Python versions and switch between them easily.  

1. **Install `pyenv` via Homebrew:**  
   ```sh
   brew install pyenv
   ```
2. **Install a Specific Version of Python:**  
   ```sh
   pyenv install 3.9.7
   ```
3. **Set Global or Local Python Version:**  
   - Set **globally**:  
     ```sh
     pyenv global 3.9.7
     ```
   - Set **locally** (in a project directory):  
     ```sh
     pyenv local 3.9.7
     ```
4. **Check Installed Versions:**  
   ```sh
   pyenv versions
   ```