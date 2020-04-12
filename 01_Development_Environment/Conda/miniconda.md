# Install

- **Linux:** https://conda.io/projects/conda/en/latest/user-guide/install/linux.html

  ```python
  # 1. In your terminal window, run:
  #Miniconda:
  bash Miniconda3-latest-Linux-x86_64.sh
  #Anaconda:
  bash Anaconda-latest-Linux-x86_64.sh
  # 2. Follow the prompts on the installer screens. ENTER->yes->ENTER->
  # 3. To make the changes take effect, close and then re-open your terminal window.
  # 4. If you'd prefer that conda's base environment not be activated on startup, set the auto_activate_base parameter to false: 
  # for conda 4.82
  conda config --set auto_activate_base false
  # 
  # 5. Test your installation. In your terminal window or Anaconda Prompt, run the command 
  conda list
  # packages in environment at /home/ubuntu16/miniconda3:
  #
  # Name                    Version                   Build  Channel
  _libgcc_mutex             0.1                        main  
  ...
  ```

- **Mac:** http://conda.pydata.org/docs/install/quick.html#os-x-miniconda-install

- **Windows:** http://conda.pydata.org/docs/install/quick.html#windows-miniconda-install



# Getting started with conda

Conda is a powerful package manager and environment manager that you use with command line commands at the Anaconda Prompt for Windows, or in a terminal window for macOS or Linux.

```python
# Starting conda
open a terminal window

# managing conda
conda --version
conda update conda

'''
When you begin using conda, you already have a default environment named base. You don't want to put programs into your base environment, though. Create separate environments to keep your programs isolated from each other.
'''
# Create a new environment and install a package in it.
conda create --name env_name install_pkgs

# activate env
source activate snowflakes

# see a list of all your environments
conda info --envs

# Change your current environment back to the default (base): 
conda activate

# Create a new environment named "snakes" that contains Python 3.5
conda create --name snakes python=3.5

# Check to see if a package you have not installed named "beautifulsoup4" is available
conda search beautifulsoup4

# Install this package into the current environment:
conda install beautifulsoup4

# Check to see if the newly installed program is in this environment:
conda list

### Uninstalling Anaconda or Miniconda ### 
# 1. Open a terminal window. Remove the entire Miniconda install directory with:
rm -rf ~/miniconda
# 2. OPTIONAL: Edit ~/.bash_profile to remove the Miniconda directory from your PATH environment variable.
# 3. OPTIONAL: Remove the following hidden file and folders that may have been created in the home directory: .condarc file .conda directory .continuum directory By running:
rm -rf ~/.condarc ~/.conda ~/.continuum
```

