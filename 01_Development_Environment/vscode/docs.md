# Visual Studio Code Docs

***官方文档：https://code.visualstudio.com/docs （最全教程）***

## 0. My Setting file

**插件列表：**

```Python
c/c++
python

code spell checker
korofileheader

latex workshop # 需要安装texlive
markdown preview enhanced

gitignore

```

**配置文件：**>preferences: Open settings (json)

```json
{
  "editor.wordWrap": "on",
  "workbench.startupEditor": "newUntitledFile",
  "latex-workshop.view.pdf.viewer": "tab",
  "latex-workshop.latex.recipes": [
      {
          "name": "xelatex",
          "tools": [
            "xelatex",
            "xelatex"
          ]
        },
      {
          "name": "xelatexb",
          "tools": [
            "xelatex",
            "bibtex",
            "xelatex",
            "xelatex"
          ]
        },
      {
        "name": "latexmk",
        "tools": [
          "latexmk"
        ]
      },
      {
        "name": "pdflatex -> bibtex -> pdflatex*2",
        "tools": [
          "pdflatex",
          "bibtex",
          "pdflatex",
          "pdflatex"
        ]
      }
    ],

    "latex-workshop.latex.tools": [
      {
          "name": "xelatex",
          "command": "xelatex",
          "args": [
              "-synctex=1",
              "-interaction=nonstopmode",
              "-file-line-error",
              "%DOC%"
          ]
      },
      {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
          "-synctex=1",
          "-interaction=nonstopmode",
          "-file-line-error",
          "-pdf",
          "%DOC%"
        ]
      },
      {
        "name": "pdflatex",
        "command": "pdflatex",
        "args": [
          "-synctex=1",
          "-interaction=nonstopmode",
          "-file-line-error",
          "%DOC%"
        ]
      },
      {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
          "%DOCFILE%"
        ]
      }
    ],

    "latex-workshop.latex.autoClean.run": "onBuilt",
    "python.dataScience.sendSelectionToInteractiveWindow": true,
    "explorer.confirmDelete": false,


    "fileheader.configObj": {
      "createFileTime": true,
      "language": {
        "languagetest": {
          "head": "/$$",
          "middle": " $ @",
          "end": " $/"
        },
        "py":{
          "head": "'''",
          "middle": "@",
          "end": "'''"
        },
        "tex":{
          "head": "% % % % % % % % % % % % % % %",
          "middle": "%",
          "end": "% % % % % % % % % % % % % % %"
        }
      },
      "autoAdd": true,
      "autoAlready": true,
      "annotationStr": {
        "head": "/*",
        "middle": " * @",
        "end": " */",
        "use": false
      },
      "headInsertLine": {
        "php": 2
      },
      "beforeAnnotation": {},
      "afterAnnotation": {},
      "specialOptions": {},
      "switch": {
        "newlineAddAnnotation": true
      },
      "prohibitAutoAdd": [
        "json"
      ],
      "moveCursor": true,
      "dateFormat": "YYYY-MM-DD HH:mm:ss",
      "atSymbol": "@",
      "atSymbolObj": {},
      "colon": ": ",
      "colonObj": {}
    },

    "fileheader.customMade": {
      "Author": "Shuai Wang",
      "Github": "https://github.com/wsustcid",
      "Version": "1.0.0",
      "Date": "Do not edit",
      "LastEditTime": "Do not edit"
    },

    "fileheader.cursorMode": {
        "description": "",
        "param": "",
        "return": ""
    }
}


```



## 1. Setup

### 1.1 Overview

Visual Studio Code is a lightweight but powerful source code editor which runs on your desktop and is available for Windows, macOS and Linux. It comes with built-in support for JavaScript, TypeScript and Node.js and has a rich ecosystem of extensions for other languages (such as C++, C#, Java, Python, PHP, Go) and runtimes (such as .NET and Unity). Begin your journey with VS Code with these [introductory videos](https://code.visualstudio.com/docs/introvideos/overview).

### 1.2 Linux

**Installation:**

See the [Download Visual Studio Code](https://code.visualstudio.com/download) page for a complete list of available installation options

**Setting VS Code as the default text editor**

```
sudo update-alternatives --set editor /usr/bin/code
```



## 2. Get Started

## 3. User Guide

## 4. Languages

## 5. Python

### 5.1 Tutorial

In this tutorial, you use Python 3 to create the simplest Python "Hello World" application in Visual Studio Code. By using the Python extension, you make VS Code into a great lightweight Python IDE (which you may find a productive alternative to PyCharm).

This tutorial introduces you to VS Code as a Python environment, primarily how to edit, run, and debug code through the following tasks:

- Write, run, and debug a Python "Hello World" Application
- Learn how to install packages by creating Python virtual environments
- Write a simple Python script to plot figures within VS Code

> **Note**: You can use VS Code with Python 2 with this tutorial, but you need to make appropriate changes to the code, which are not covered here.

#### Prerequisites

To successfully complete this tutorial, you need to first setup your Python development environment. Specifically, this tutorial requires:

- VS Code
- VS Code Python extension
- Python 3

#### **Install Visual Studio Code and the Python Extension**

1. If you have not already done so, install [VS Code](https://code.visualstudio.com/).
2. Next, install the [Python extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) from the Visual Studio Marketplace. 

#### **Install a Python interpreter**

Along with the Python extension, you need to install a Python interpreter. Which interpreter you use is dependent on your specific needs, but some guidance is provided below.

- **Windows**: Install [Python from python.org](https://www.python.org/downloads/). You can typically use the **Download Python** button that appears first on the page to download the latest version.

- **macOS：**The system install of Python on macOS is not supported. Instead, an installation through [Homebrew](https://brew.sh/) is recommended. To install Python using Homebrew on macOS use `brew install python3` at the Terminal prompt.

> **Note** On macOS, make sure the location of your VS Code installation is included in your PATH environment variable. See [these setup instructions](https://code.visualstudio.com/docs/setup/mac#_launching-from-the-command-line) for more information.

- **Linux：**The built-in Python 3 installation on Linux works well, but to install other Python packages you must install `pip` with [`get-pip.py`](https://pip.pypa.io/en/stable/installing/#installing-with-get-pip-py).

- **Data Science**: If your primary purpose for using Python is Data Science, then you might consider a download from [Anaconda](https://www.anaconda.com/download/). Anaconda provides not just a Python interpreter, but many useful libraries and tools for data science.

#### **Verify the Python installation**

To verify that you've installed Python successfully on your machine, run one of the following commands (depending on your operating system):

- Linux/macOS: open a Terminal Window and type the following command:

  ```
  python3 --version
  ```

- Windows: open a command prompt and run the following command:

  ```
  py -3 --version
  ```

If the installation was successful, the output window should show the version of Python that you installed.

> **Note** You can use the `py -0` command in the VS Code integrated terminal to view the versions of python installed on your machine. The default interpreter is identified by an asterisk (*).

#### Start VS Code in a project (workspace) folder

Using a command prompt or terminal, create an empty folder called "hello", navigate into it, and open VS Code (`code`) in that folder (`.`) by entering the following commands:

```linux
mkdir hello
cd hello
code .
```

> **Note**: If you're using an Anaconda distribution, be sure to use an Anaconda command prompt.

By starting VS Code in a folder, that folder becomes your "workspace". **VS Code stores settings that are specific to that workspace in `.vscode/settings.json`, which are separate from user settings that are stored globally.**

Alternately, you can run VS Code through the operating system UI, then use **File > Open Folder** to open the project folder.

#### Select a Python interpreter

Python is an interpreted language, and in order to run Python code and get Python IntelliSense, you must tell VS Code which interpreter to use.

- From within VS Code, select a Python 3 interpreter by opening the **Command Palette** (Ctrl+Shift+P), start typing the **Python: Select Interpreter** command to search, then select the command.  You can also use the **Select Python Environment** option on the Status Bar if available (it may already show a selected interpreter, too):

![No interpreter selected](https://code.visualstudio.com/assets/docs/python/environments/no-interpreter-selected-statusbar.png)

The command presents a list of available interpreters that VS Code can find automatically, including virtual environments. If you don't see the desired interpreter, see [Configuring Python environments](https://code.visualstudio.com/docs/python/environments).

> **Note**: When using an Anaconda distribution, the correct interpreter should have the suffix `('base':conda)`, for example `Python 3.7.3 64-bit ('base':conda)`.

Selecting an interpreter sets the `python.pythonPath` value in your workspace settings to the path of the interpreter. To see the setting, select **File** > **Preferences** > **Settings** (**Code** > **Preferences** > **Settings** on macOS), then select the **Workspace Settings** tab.

> **Note**: If you select an interpreter without a workspace folder open, VS Code sets `python.pythonPath` in your user settings instead, which sets the default interpreter for VS Code in general. The user setting makes sure you always have a default interpreter for Python projects. The workspace settings lets you override the user setting.

#### Create a Python Hello World source code file

- From the File Explorer toolbar, select the **New File** button on the `hello` folder. Name the file `hello.py`, and it automatically opens in the editor:

> **Note**: The File Explorer toolbar also allows you to create folders within your workspace to better organize your code. You can use the **New folder** button to quickly create a folder.

- Now that you have a code file in your Workspace, enter the following source code in `hello.py`:

```
msg = "Hello World"
print(msg)
```

When you start typing `print`, notice how [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) presents auto-completion options.

![IntelliSense appearing for Python code](https://code.visualstudio.com/assets/docs/python/tutorial/intellisense01.png)

IntelliSense and auto-completions work for standard Python modules as well as other packages you've installed into the environment of the selected Python interpreter. **It also provides completions for methods available on object types.** For example, because the `msg` variable contains a string, IntelliSense provides string methods when you type `msg.`:

![IntelliSense appearing for a variable whose type provides methods](https://code.visualstudio.com/assets/docs/python/tutorial/intellisense02.png)

Feel free to experiment with IntelliSense some more, but then revert your changes so you have only the `msg` variable and the `print` call, and save the file (Ctrl+S).

For full details on editing, formatting, and refactoring, see [Editing code](https://code.visualstudio.com/docs/python/editing). The Python extension also has full support for [Linting](https://code.visualstudio.com/docs/python/linting).

#### Run Hello World

It's simple to run `hello.py` with Python. Just click the **Run Python File in Terminal** play button in the top-right side of the editor.

![Using the run python file in terminal button](https://code.visualstudio.com/assets/docs/python/tutorial/run-python-file-in-terminal-button.png)

The button opens a terminal panel in which your Python interpreter is automatically activated, then runs `python3 hello.py` (macOS/Linux) or `python hello.py` (Windows):

![Program output in a Python terminal](https://code.visualstudio.com/assets/docs/python/tutorial/output-in-terminal.png)

There are three other ways you can run Python code within VS Code:

- Right-click anywhere in the editor window and select **Run Python File in Terminal** (which saves the file automatically):

- Select one or more lines, then press Shift+Enter or right-click and select **Run Selection/Line in Python Terminal**. This command is convenient **for testing just a part of a file.**

- From the Command Palette (Ctrl+Shift+P), select the **Python: Start REPL** command to open a REPL terminal for the currently selected Python interpreter. In the REPL, you can then enter and run lines of code one at a time.

#### Configure and run the debugger

Let's now try debugging our simple Hello World program.

- First, set a breakpoint on line 2 of `hello.py` by placing the cursor on the `print` call and pressing F9. Alternately, just click in the editor's left gutter, next to the line numbers. When you set a breakpoint, a red circle appears in the gutter.

![Setting a breakpoint in hello.py](https://code.visualstudio.com/assets/docs/python/tutorial/breakpoint-set.png)

- Next, to initialize the debugger, press F5. Since this is your first time debugging this file, a configuration menu will open from the Command Palette allowing you to select the type of debug configuration you would like for the opened file.

![Debug configurations after launch.json is created](https://code.visualstudio.com/assets/docs/python/tutorial/debug-configurations.png)

**Note**: VS Code uses JSON files for all of its various configurations; `launch.json` is the standard name for a file containing debugging configurations. These different configurations are fully explained in [Debugging configurations](https://code.visualstudio.com/docs/python/debugging); for now, just select **Python File**, which is the configuration that runs the current file shown in the editor using the currently selected Python interpreter.

- The debugger will stop at the first line of the file breakpoint. The current line is indicated with a yellow arrow in the left margin. If you examine the **Local** variables window at this point, you will see now defined `msg` variable appears in the **Local** pane.

![Debugging step 2 - variable defined](https://code.visualstudio.com/assets/docs/python/tutorial/debug-step-02.png)

- A debug toolbar appears along the top with the following commands from left to right: continue (F5), step over (F10), step into (F11), step out (Shift+F11), restart (Ctrl+Shift+F5), and stop (Shift+F5).

![Debugging toolbar](https://code.visualstudio.com/assets/docs/python/tutorial/debug-toolbar.png)

- The Status Bar also changes color (orange in many themes) to indicate that you're in debug mode. The **Python Debug Console** also appears automatically in the lower right panel to show the commands being run, along with the program output.

- To continue running the program, select the continue command on the debug toolbar (F5). The debugger runs the program to the end.

> **Tip** Debugging information can also be seen by hovering over code, such as variables. In the case of `msg`, hovering over the variable will display the string `Hello world` in a box above the variable.

- You can also work with variables in the **Debug Console** (If you don't see it, select **Debug Console** in the lower right area of VS Code, or select it from the **...** menu.) Then try entering the following lines, one by one, at the **>** prompt at the bottom of the console:

```python
# 注意这一步是紧接着之前的步骤的，即设置断点->F5执行->Debug console 继续调试
msg
msg.capitalize()
msg.split()
```

![Debugging step 3 - using the debug console](https://code.visualstudio.com/assets/docs/python/tutorial/debug-step-03.png)

- Select the blue **Continue** button on the toolbar again (or press F5) to run the program to completion. "Hello World" appears in the **Python Debug Console** if you switch back to it, and VS Code exits debugging mode once the program is complete.

- If you restart the debugger, the debugger again stops on the first breakpoint.

- To stop running a program before it's complete, use the red square stop button on the debug toolbar (Shift+F5), or use the **Run > Stop debugging** menu command.

- For full details, see [Debugging configurations](https://code.visualstudio.com/docs/python/debugging), which includes notes on how to use a specific Python interpreter for debugging.

> **Tip: Use Logpoints instead of print statements**: Developers often litter source code with `print` statements to quickly inspect variables without necessarily stepping through each line of code in a debugger. In VS Code, you can instead use **Logpoints**. A Logpoint is like a breakpoint except that it logs a message to the console and doesn't stop the program. For more information, see [Logpoints](https://code.visualstudio.com/docs/editor/debugging#_logpoints) in the main VS Code debugging article.

#### Install and use packages（创建虚拟环境）

Let's now run an example that's a little more interesting. In Python, packages are how you obtain any number of useful code libraries, typically from [PyPI](https://pypi.org/). For this example, you use the `matplotlib` and `numpy` packages to create a graphical plot as is commonly done with data science.

- Return to the **Explorer** view (the top-most icon on the left side, which shows files), create a new file called `standardplot.py`, and paste in the following source code:

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 20, 100)  # Create a list of evenly-spaced numbers over the range
plt.plot(x, np.sin(x))       # Plot the sine of each x point
plt.show()                   # Display the plot
```

> **Tip**: If you enter the above code by hand, you may find that auto-completions change the names after the `as` keywords when you press Enter at the end of a line. To avoid this, type a space, then Enter.

- Next, try running the file in the debugger using the "Python: Current file" configuration as described in the last section.

Unless you're using an Anaconda distribution or have previously installed the `matplotlib` package, you should see the message, **"ModuleNotFoundError: No module named 'matplotlib'"**. Such a message indicates that the required package isn't available in your system.

- To install the `matplotlib` package (which also installs `numpy` as a dependency), stop the debugger and use the Command Palette to run **Terminal: Create New Integrated Terminal** (Ctrl+Shift+`)). This command opens a command prompt for your selected interpreter.

**A best practice among Python developers is to avoid installing packages into a global interpreter environment.** 

***You instead use a project-specific `virtual environment` that contains a copy of a global interpreter. Once you activate that environment, any packages you then install are isolated from other environments. Such isolation reduces many complications that can arise from conflicting package versions.*** 

- To **create a *virtual environment* and install the required packages**, enter the following commands as appropriate for your operating system:

> **Note**: For additional information about virtual environments, see [Environments](https://code.visualstudio.com/docs/python/environments#_global-virtual-and-conda-environments).

1. Create and activate the virtual environment

   > **Note**: When you create a new virtual environment, you should be prompted by VS Code to set it as the default for your workspace folder. If selected, the environment will automatically be activated when you open a new terminal.

   ![Virtual environment dialog](https://code.visualstudio.com/assets/docs/python/tutorial/virtual-env-dialog.png)

   **For windows**

   ```
   py -3 -m venv .venv
   .venv\scripts\activate
   ```

   If the activate command generates the message "Activate.ps1 is not digitally signed. You cannot run this script on the current system.", then you need to temporarily change the PowerShell execution policy to allow scripts to run (see [About Execution Policies](https://go.microsoft.com/fwlink/?LinkID=135170) in the PowerShell documentation):

   ```
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process
   ```

   **For macOS/Linux**

   ```
   python3 -m venv .venv
   source .venv/bin/activate
   ```

2. Select your new environment by using the **Python: Select Interpreter** command from the **Command Palette**.

3. Install the packages

   ```
   # Don't use with Anaconda distributions because they include matplotlib already.
   
   # macOS
   python3 -m pip install matplotlib
   
   # Windows (may require elevation)
   python -m pip install matplotlib
   
   # Linux (Debian)
   apt-get install python3-tk
   python3 -m pip install matplotlib
   ```

4. Rerun the program now (with or without the debugger) and after a few moments a plot window appears with the output:

   ![matplotlib output](https://code.visualstudio.com/assets/docs/python/tutorial/plot-output.png)

5. Once you are finished, type `deactivate` in the terminal window to deactivate the virtual environment.

- For additional examples of creating and activating a virtual environment and installing packages, see the [Django tutorial](https://code.visualstudio.com/docs/python/tutorial-django) and the [Flask tutorial](https://code.visualstudio.com/docs/python/tutorial-flask).

**手动指定自己的虚拟环境路径：**

```json
"python.pythonPath": "/home/abc/dev/ala/venv/bin/python",
```

- 多个路径通过逗号隔开

- 设置python.venv path 不好使，待解决！

### 5.2 Editing Code

### 5.3 Linting

### 5.4 Debugging

### 5.5 Environments

An "environment" in Python is the context in which a Python program runs. An environment consists of an interpreter and any number of installed packages. The Python extension for VS Code provides helpful integration features for working with different environments.

#### Select and activate an environment

By default, the Python extension looks for and uses the first Python interpreter it finds in the system path. If it doesn't find an interpreter, it issues a warning. On macOS, the extension also issues a warning if you're using the OS-installed Python interpreter, because you typically want to use an interpreter you install directly. In either case, you can disable these warnings by setting `python.disableInstallationCheck` to `true` in your user [settings](https://code.visualstudio.com/docs/getstarted/settings).

- To select a specific environment, use the **Python: Select Interpreter** command from the **Command Palette** (Ctrl+Shift+P).

  ![Python: Select Interpreter command](https://code.visualstudio.com/assets/docs/python/environments/select-interpreters-command.png)

  - You can switch environments at any time; switching environments helps you test different parts of your project with different interpreters or library versions as needed.

  - The **Python: Select Interpreter** command displays a list of available global environments, conda environments, and virtual environments. (See the [Where the extension looks for environments](https://code.visualstudio.com/docs/python/environments#_where-the-extension-looks-for-environments) section for details, including the distinctions between these types of environments.) The following image, for example, shows several Anaconda and CPython installations along with a conda environment and a virtual environment (`env`) that's located within the workspace folder:

    ![List of interpreters](https://code.visualstudio.com/assets/docs/python/environments/interpreters-list.png)

    > **Note:** On Windows, it can take a little time for VS Code to detect available conda environments. During that process, you may see "(cached)" before the path to an environment. The label indicates that VS Code is presently working with cached information for that environment.



**设置默认解释器路径：**

*Selecting an interpreter from the list adds an entry for `python.pythonPath` with the path to the interpreter inside your [Workspace Settings](https://code.visualstudio.com/docs/getstarted/settings). Because the path is part of the workspace settings, the same environment should already be selected whenever you open that workspace.* 

If you'd like to set up a default interpreter for your applications, you can instead add an entry for `python.pythonPath` manually inside your User Settings. To do so, open the Command Palette (Ctrl+Shift+P) and enter **Preferences: Open User Settings**. Then set `python.pythonPath`, which is in the Python extension section of User Settings, with the appropriate interpreter.

**解释器与终端:**

- The Python extension uses the selected environment for running Python code (using the **Python: Run Python File in Terminal** command), providing language services (auto-complete, syntax checking, linting, formatting, etc.) when you have a `.py` file open in the editor, 

- and opening a terminal with the **Terminal: Create New Integrated Terminal** command. In the latter case, VS Code automatically activated the selected environment.

- Changing interpreters with the **Python: Select Interpreter** command doesn't affect terminal panels that are already open. You can thus activate separate environments in a split terminal: select the first interpreter, create a terminal for it, select a different interpreter, then use the split button (Ctrl+Shift+5) in the terminal title bar.

- **However, launching VS Code from a shell in which a certain Python environment is activated does not automatically activate that environment in the default Integrated Terminal.** Use the **Terminal: Create New Integrated Terminal** command after VS Code is running.

  > **Note:** conda environments cannot be automatically activated in the integrated terminal if PowerShell is set as the integrated shell. See [Integrated terminal - Configuration](https://code.visualstudio.com/docs/editor/integrated-terminal#_configuration) for how to change the shell.

> **Tip**: To prevent automatic activation of a selected environment, add `"python.terminal.activateEnvironment": false` to your `settings.json` file (it can be placed anywhere as a sibling to the existing settings).

> **Note**: By default, VS Code uses the interpreter identified by `python:pythonPath` setting when debugging code. You can override this behavior by specifying a different path in the `pythonPath` property of a debug configuration. See [Choose a debugging environment](https://code.visualstudio.com/docs/python/environments#_choose-a-debugging-environment).

The Status Bar always shows the current interpreter.

![Status Bar showing a selected interpreter](https://code.visualstudio.com/assets/docs/python/environments/selected-interpreter-status-bar.png)

The Status Bar also reflects when no interpreter is selected.

![No interpreter selected](https://code.visualstudio.com/assets/docs/python/environments/no-interpreter-selected-statusbar.png)

In either case, clicking this area of the Status Bar is a convenient shortcut for the **Python: Select Interpreter** command.

#### Choose a debugging environment

By default, the `python.pythonPath` setting specifies the Python interpreter to use for debugging. However, if you have a `pythonPath` property in the **debug configuration** of `launch.json`, that interpreter is used instead. To be more specific, VS Code applies the following order of precedence when determining which interpreter to use for debugging:

1. `pythonPath` property of the selected debug configuration in `launch.json`
2. `python.pythonPath` setting in the workspace `settings.json`
3. `python.pythonPath` setting in the user `settings.json`

For more details on debug configuration, see [Debugging configurations](https://code.visualstudio.com/docs/python/debugging).

#### Where the extension looks for environments

The extension automatically looks for interpreters in the following locations:

- Standard install paths such as `/usr/local/bin`, `/usr/sbin`, `/sbin`, `c:\\python27`, `c:\\python36`, etc.
- Virtual environments located directly under the workspace (project) folder.
- Virtual environments located in the folder identified by the `python.venvPath` setting (see [General settings](https://code.visualstudio.com/docs/python/settings-reference#_general-settings)), which can contain multiple virtual environments. The extension looks for virtual environments in the first-level subfolders of `venvPath`.
- Virtual environments located in a `~/.virtualenvs` folder for [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/).
- Interpreters installed by [pyenv](https://github.com/pyenv/pyenv).
- A [pipenv](https://pipenv.readthedocs.io/) environment for the workplace folder. If one is found, then no other interpreters are searched for or listed as pipenv expects to manage all aspects.
- Virtual environments located in the path identified by `WORKON_HOME` (as used by [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/) and [pipenv](https://pipenv.readthedocs.io/)).
- Conda environments that contain a Python interpreter. VS Code does not show conda environments that don't contain an interpreter.
- Interpreters installed in a `.direnv` folder for [direnv](https://direnv.net/) under the workspace (project) folder.

You can also [manually specify an interpreter](https://code.visualstudio.com/docs/python/environments#_manually-specify-an-interpreter) if Visual Studio Code does not locate it automatically.

The extension also loads an [environment variable definitions file](https://code.visualstudio.com/docs/python/environments#_environment-variable-definitions-file) identified by the `python.envFile` setting. The default value of this setting is `${workspaceFolder}/.env`.

#### Global, virtual, and conda environments

developers often create a **virtual environment** for a project. A virtual environment is a subfolder in a project that contains a copy of a specific interpreter. When you activate the virtual environment, any packages you install are installed only in that environment's subfolder. When you then run a Python program within that environment, you know that it's running against only those specific packages.

> **Tip**: A **conda environment** is a virtual environments that's created and managed using the `conda` package manager. See [Conda environments](https://code.visualstudio.com/docs/python/environments#_conda-environments) for more details.

To create a virtual environment, use the following command, where ".venv" is the name of the environment folder:

```
# macOS/Linux
# You may need to run sudo apt-get install python3-venv first
python3 -m venv .venv

# Windows
# You can also use py -3 -m venv .venv
python -m venv .venv
```

When you create a new virtual environment, a prompt will be displayed to allow you to select it for the workspace.

![Python environment prompt](https://code.visualstudio.com/assets/docs/python/environments/python-environment-prompt.png)

This will add the path to the Python interpreter from the new virtual environment to your workspace settings. That environment will then be used when installing packages and running code through the Python extension. For examples of using virtual environment in projects, see the [Django tutorial](https://code.visualstudio.com/docs/python/tutorial-django) and the [Flask tutorial](https://code.visualstudio.com/docs/python/tutorial-flask).

If the activate command generates the message "Activate.ps1 is not digitally signed. You cannot run this script on the current system.", then you need to temporarily change the PowerShell execution policy to allow scripts to run (see [About Execution Policies](https://go.microsoft.com/fwlink/?LinkID=135170) in the PowerShell documentation):

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process
```

> **Note**: If you're using a version of the Python extension prior to 2018.10, and you create a virtual environment in a VS Code terminal, you must run the **Reload Window** command from the Command Palette and then use **Python: Select Interpreter** to activate the environment. If you have any problems with VS Code recognizing a virtual environment, please [file an issue](https://github.com/Microsoft/vscode-docs/issues) in the documentation repository so we can help determine the cause.

> **Tip**: When you're ready to deploy the application to other computers, you can create a `requirements.txt` file with the command `pip freeze > requirements.txt` (`pip3` on macOS/Linux). The requirements file describes the packages you've installed in your virtual environment. With only this file, you or other developers can restore those packages using `pip install -r requirements.txt` (or, again, `pip3` on macOS/Linux). By using a requirements file, you need not commit the virtual environment itself to source control.

#### Conda environments

A conda environment is a Python environment that's managed using the `conda` package manager (see [Getting started with conda](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html) ([conda.io](http://conda.io/))). Conda works well to create environments with interrelated dependencies as well as binary packages. Unlike virtual environments, which are scoped to a project, conda environments are available globally on any given computer. This availability makes it easy to configure several distinct conda environments and then choose the appropriate one for any given project.

As noted earlier, the Python extension automatically detects existing conda environments provided that the environment contains a Python interpreter. For example, the following command creates a conda environment with the Python 3.4 interpreter and several libraries, which VS Code then shows in the list of available interpreters:

```
conda create -n env-01 python=3.4 scipy=0.15.0 astroid babel
```

In contrast, if you fail to specify an interpreter, as with `conda create --name env-00`, the environment won't appear in the list.

For more information on the conda command line, see [Conda environments](https://conda.io/docs/user-guide/tasks/manage-environments.html) ([conda.io](http://conda.io/)).

Additional notes:

- If you create a new conda environment while VS Code is running, use the **Reload Window** command to refresh the environment list shown with **Python: Select Interpreter**; otherwise you may not see the environment there. It might take a short time to appear; if you don't see it at first, wait 15 seconds then try using the command again.
- Although the Python extension for VS Code doesn't currently have direct integration with conda environment.yml files, VS Code itself is a great YAML editor.
- Conda environments can't be automatically activated in the VS Code Integrated Terminal if the default shell is set to PowerShell. To change the shell, see [Integrated terminal - Configuration](https://code.visualstudio.com/docs/editor/integrated-terminal#_configuration).
- You can manually specify the path to the conda executable to use for activation (version 4.4+). To do so, open the Command Palette (Ctrl+Shift+P) and enter **Preferences: Open User Settings**. Then set `python.condaPath`, which is in the Python extension section of User Settings, with the appropriate path.

#### 手动指定自己的解释器路径

If VS Code does not automatically locate an interpreter you want to use, you can set the path to it manually in your **Workspace Settings** `settings.json` file. With any of the entries that follow, you can just add the line as a sibling to other existing settings.)

First, select the **File** (**Code** on macOS) > **Preferences** > **Settings** menu command (Ctrl+,) to open your [Settings](https://code.visualstudio.com/docs/getstarted/settings), select **Workspace**.

Then do any of the following steps:

1. Create or modify an entry for `python.pythonPath` with the full path to the Python executable (if you edit `settings.json` directly, add the line below as the setting):

   For example:

   - Windows:

     ```
     "python.pythonPath": "c:/python36/python.exe",
     ```

   - macOS/Linux:

     ```
     "python.pythonPath": "/home/python36/python",
     ```

2. You can also use `python.pythonPath` to point to a virtual environment, for example:

   Windows:

   ```
   "python.pythonPath": "c:/dev/ala/venv/Scripts/python.exe",
   ```

   macOS/Linux: **(添加自己的虚拟环境是通过此项设置设置成功的！！)**

   ```
   "python.pythonPath": "/home/abc/dev/ala/venv/bin/python",
   ```

3. You can use an environment variable in the path setting using the syntax `${env:VARIABLE}`. For example, if you've created a variable named `PYTHON_INSTALL_LOC` with a path to an interpreter, you can then use the following setting value:

   ```
   "python.pythonPath": "${env:PYTHON_INSTALL_LOC}",
   ```

   By using an environment variable, you can easily transfer a project between operating systems where the paths are different, just be sure to set the environment variable on the operating system first.

#### Environment variable definitions file

An environment variable definitions file is a simple text file containing key-value pairs in the form of `environment_variable=value`, with `#` used for comments. Multiline values are not supported, but values can refer to any other environment variable that's already defined in the system or earlier in the file. For more information, see [Variable substitution](https://code.visualstudio.com/docs/python/environments#_variable-substitution).

By default, the Python extension looks for and loads a file named `.env` in the current workspace folder, then applies those definitions. The file is identified by the default entry `"python.envFile": "${workspaceFolder}/.env"` in your user settings (see [General settings](https://code.visualstudio.com/docs/python/settings-reference#_general-settings)). You can change the `python.envFile` setting at any time to use a different definitions file.

A debug configuration also contains an `envFile` property that also defaults to the `.env` file in the current workspace (see [Debugging - Set configuration options](https://code.visualstudio.com/docs/python/debugging#_set-configuration-options)). This property allows you to easily set variables for debugging purposes that replace variables specified in the default `.env` file.

For example, when developing a web application, you might want to easily switch between development and production servers. Instead of coding the different URLs and other settings into your application directly, you could use separate definitions files for each. For example:

**dev.env file**

```
# dev.env - development configuration

# API endpoint
MYPROJECT_APIENDPOINT=https://my.domain.com/api/dev/

# Variables for the database
MYPROJECT_DBURL=https://my.domain.com/db/dev
MYPROJECT_DBUSER=devadmin
MYPROJECT_DBPASSWORD=!dfka**213=
```

**prod.env file**

```
# prod.env - production configuration

# API endpoint
MYPROJECT_APIENDPOINT=https://my.domain.com/api/

# Variables for the database
MYPROJECT_DBURL=https://my.domain.com/db/
MYPROJECT_DBUSER=coreuser
MYPROJECT_DBPASSWORD=kKKfa98*11@
```

You can then set the `python.envFile` setting to `${workspaceFolder}/prod.env`, then set the `envFile` property in the debug configuration to `${workspaceFolder}/dev.env`.

#### Variable substitution

When defining an environment variable in a definitions file, you can use the value of any existing environment variable with the following general syntax:

```
<VARIABLE>=...${EXISTING_VARIABLE}...
```

where `...` means any other text as used in the value. The curly braces are required.

Within this syntax, the following rules apply:

- Variables are processed in the order they appear in the `.env` file, so you can use any variable that's defined earlier in the file.
- Single or double quotes don't affect substituted value and are included in the defined value. For example, if the value of `VAR1` is `abcedfg`, then `VAR2='${VAR1}'` assigns the value `'abcedfg'` to `VAR2`.
- The `$` character can be escaped with a backslash, as in `\$`.
- You can use recursive substitution, such as `PYTHONPATH=${PROJ_DIR}:${PYTHONPATH}` (where `PROJ_DIR` is any other environment variable).
- You can use only simple substitution; nesting such as `${_${VAR1}_EX}` is not supported.
- Entries with unsupported syntax are left as-is.

#### Use of the PYTHONPATH variable

The [PYTHONPATH](https://docs.python.org/3/using/cmdline.html#envvar-PYTHONPATH) environment variable specifies additional locations where the Python interpreter should look for modules. In VS Code, PYTHONPATH can be set through the terminal settings (terminal.integrated.env.*) and/or within an `.env` file.

When the terminal settings are used, PYTHONPATH affects any tools that are run within the terminal by a user, as well as any action the extension performs for a user that is routed through the terminal such as debugging. However, in this case when the extension is performing an action that isn't routed through the terminal, such as the use of a linter or formatter, then this setting will not have an effect on module look-up.

When PYTHONPATH is set using an `.env` file, it will affect anything the extension does on your behalf and actions performed by the debugger, but it will not affect tools run in the terminal.

If needed, you can set PYTHONPATH using both methods.

An example of when to use PYTHONPATH would be if you have source code in a `src` folder and tests in a `tests` folder. When running tests, however, those tests can't normally access modules in `src` unless you hard-code relative paths. To solve this problem, add the path to `src` to PYTHONPATH.

The value of PYTHONPATH can contain multiple locations separated by `os.pathsep`: a semicolon (`;`) on Windows and a colon (`:`) on Linux/macOS. Invalid paths are ignored. If you find that your value for PYTHONPATH isn't working as expected, make sure that you're using the correct separator between locations for the operating system. For example, using a colon to separate locations on Windows, or using a semicolon to separate locations on Linux/macOS results in an invalid value for PYTHONPATH, which is ignored.

> **Note**: PYTHONPATH does **not** specify a path to a Python interpreter itself, and should not be used with the `python.pythonPath` setting. For additional information about PYTHONPATH, read the [PYTHONPATH documentation](https://docs.python.org/3/using/cmdline.html#envvar-PYTHONPATH)

## 6. C++

## 7. Remote









































## 0.2 使用VSCode 调试Python

- 首先，下载并安装VS Code：[下载地址](https://code.visualstudio.com/)
  - 安装完成后，打开软件会自动提示你安装一些重要插件，如中文语言包，Git等，这里可以直接选择安装Python插件(也可以使用Ctrl+Shift+X可以打开扩展商店然后输入Python搜索)。可以使用微软的插件
  - 点击 install
  - **卸载时要想完全删除插件及配置文件，需要手动删除home 目录下的.vscode文件**

- 用VS打开项目文件夹
  - 首先，创建一个空文件夹''hello''，然后使用VS Code打开它。通过VS Code打开文件夹，该文件夹就变成了你的”工作区“。设定解释器后，VS Code在`.vscode/settings.json`中存储该工作去的特殊配置，与用户的全局设定相分开。

- 选取Python解释器
  - 使用`Ctrl+Shift+P`打开命令板，输入`Python: Select Interpreter`进行搜索。
    ![img](https:////upload-images.jianshu.io/upload_images/3860226-4297a5204f6111e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/653/format/webp)
  - 接下来会显示VS Code所能找到的全部解释器，选择你需要的哪个就好。如果没找到你需要的哪个，参考[Python环境变量配置](https://code.visualstudio.com/docs/python/environments)。

- 创建Hello World
  - 在Hello文件夹下新建文件，命名为hello.py
  - 编写如下程序，可以自动补全，编写完成后按`ctrl+s`保存。

```python
msg = "Hello World"
print(msg)
```

- 运行Hello World
  - 在空白处右键选择**在终端运行Python文件**，就可以看到运行结果了。
  - 此外，VS Code中还有一些运行Python代码的方式：
  - 选择一行或者多行，使用`Shift+Enter`或者右键选择**在Python终端中运行选定内容/行**运行一部分代码。
  - 编辑完代码按F5即可运行。初次运行会让你选环境，选择python即可。
    - 默认按F5后需要再按一次F5程序才会运行，如果要按F5马上运行需要将launch.json文件的  "stopOnEntry": true,改为  "stopOnEntry": false。
  - 推介个插件，*vscode-icons*可以使VSCode左侧的资源管理器根据文件类型显示图标
- 配置及运行调试器
  - 首先，把光标移到第二行然后按`F9`，就可以设置一个断点。同样，也可以在行号左边双击设置。
  - 接下来，在侧边栏打开Debug视图（蜘蛛图标）或 ctrl+shif+D
  - 然后点击配置按钮，选择Python，然后Python插件会自动创建包含一系列配置的`launch.json`文件，可以在下拉列表里面选择，现在选择`Python: Current File`即可。

  - ![img](https:////upload-images.jianshu.io/upload_images/3860226-72a61c7a5aadd16b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)
  - 为了让调试器在自动在程序开始时停在第一行，这个功能顾名思义，意思就是在进入程序的时候就暂停执行，相当于在程序的第一行放一个断点。打开这个功能非常方便，只需要在`lanuch.json`中加入下图中红线那一行就可以了。这个功能是默认禁止的，所以删掉这一行或者把`true`改成`false`都可以起到禁止的效果。，添加一条配置stopOnEntry": true

```
{
    "name": "Python: Current File",
    "type": "python",
    "request": "launch",
    "program": "${file}",
    "stopOnEntry": true
},
```

在编辑器中跳转回`hello.py`，点击绿色箭头或者按`F5`启动调试器。调试器会停留在文件的第一行。


- 调试程序, 调试工具栏出现在页面上方，从左到右功能分别是：
  - 运行（F5），跳过（F10），跳入（F11），跳出（Shift+F11），重新开始（Ctrl+Shift+F5），以及停止（Shift+F5）。
  - 左侧分别为变量栏，用来显示各种变量；
  - WATCH栏用来监控变量值变化，可以通过选中某一变量，然后右键选择：Debug: add to watch
- 编辑断点：在断点上右键，可以选择编辑断点，比如使用log message选项，可以添加一些输出信息（输出变量用{}括起来），而不用终止或修改程序，程序输出会在Debug console出现
- 编辑断点-expression：可以写入一个条件表达式，程序仅当此表达式为真时此断电被触发，多用于循环语句中，在某一循环处触发断点，如：name=="Testuser"
- BREAKINGPOINTS栏选中 Rasied Exceptions，程序会在发生异常处中断，可手动设置一个异常进行尝试：`raise ValueError()`

#### 插件

pylint

配置flake8

安装flake8之后写代码的时候编辑器就会提示哪里出错，代码格式不规范也会提示

1. 打开命令行
2. 输入 "pip install flake8"
3. 安装flake8成功后，打开VScode，文件->首选项->用户设置，在settings.json文件中输入"python.linting.flake8Enabled": true



![img](https:////upload-images.jianshu.io/upload_images/2704468-85217cb5bbf72173.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)



配置yapf

安装yapf之后在VSCode中按Alt+Shift+F即可自动格式化代码

1. 打开命令行
2. 输入 "pip install yapf"
3. 安装yapf成功后，打开VSCode，文件->首选项->用户设置，在settings.json文件中输入"python.formatting.provider": "yapf"



![img](https:////upload-images.jianshu.io/upload_images/2704468-efa0329e7e39a5f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

yapf配置.png



![img](https:////upload-images.jianshu.io/upload_images/2704468-2db2cf123ffd98c1.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

yapf效果图.gif

几个小技巧

1. 查看函数或者类的定义
   Ctrl+鼠标左键点击函数名或者类名即可跳转到定义处，在函数名或者类名上按F12也可以实现同样功能

2. 更改变量名
   在变量名上按F2即可实现重命名变量

3. python断点调试
   在行号的左边点击即可设置断点，在左边的调试界面可以查看变量的变化

4. 隐藏菜单栏
   这个属于个人习惯，如果你也感觉菜单栏很碍眼，可以点击查看->切换菜单栏，即可隐藏菜单栏。需要菜单栏的时候按Alt键即可查看

5. 设置快捷键
   文件->首选项->键盘快捷方式，将需要的修改的快捷键的整个大括号里面的内容复制到右边keybindings.json文件中，然后修改“key”的值为你需要的快捷键即可。我这边只修改了复制一行和删除一行的快捷键。

   

   ![img](https:////upload-images.jianshu.io/upload_images/2704468-72522bb625b48a1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

   快捷键设置.png

更新

1. 2016-8-25 更新
   推介两个插件
2. Guides，缩进线插件，让代码看起来更清晰
3. vscode-todo，使VSCode支持TODO的插件



## 附录:

##### 1. 软件版本阶段说明

- `Alpha版`: 此版本表示该软件在此阶段主要是以实现软件功能为主，通常只在软件开发者内部交流，一般而言，该版本软件的Bug较多，需要继续修改。
- `Beta版`: 该版本相对于α版已有了很大的改进，消除了严重的错误，但还是存在着一些缺陷，需要经过多次测试来进一步消除，此版本主要的修改对像是软件的UI。
- `RC版`: 该版本已经相当成熟了，基本上不存在导致错误的BUG，与即将发行的正式版相差无几。
- `Release版`: 该版本意味“最终版本”，在前面版本的一系列测试版之后，终归会有一个正式版本，是最终交付用户使用的一个版本。该版本有时也称为标准版。一般情况下，`Release` 不会以单词形式出现在软件封面上，取而代之的是符号(R)。

##### 2. 版本命名规范

软件版本号由四部分组成：

- 第一个1为主版本号
- 第二个1为子版本号
- 第三个1为阶段版本号
- 第四部分为日期版本号加希腊字母版本号

希腊字母版本号共有5种，分别为：`base`、`alpha`、`beta`、`RC`、`release`。例如：1.1.1.051021_beta

**常规**：完全的版本号定义，分三项：：<主版本号>.<次版本号>.<修订版本号>，如 1.0.0

##### 3. 版本号定修改规则

- `主版本号(1)`：当功能模块有较大的变动，比如增加多个模块或者整体架构发生变化。此版本号由项目决定是否修改。
- `子版本号(1)`：当功能有一定的增加或变化，比如增加了对权限控制、增加自定义视图等功能。此版本号由项目决定是否修改。
- `阶段版本号(1)`：一般是 Bug 修复或是一些小的变动，要经常发布修订版，时间间隔不限，修复一个严重的bug即可发布一个修订版。此版本号由项目经理决定是否修改。
- `日期版本号(051021)`: 用于记录修改项目的当前日期，每天对项目的修改都需要更改日期版本号。此版本号由开发人员决定是否修改。
- `希腊字母版本号(beta)`: 此版本号用于标注当前版本的软件处于哪个开发阶段，当软件进入到另一个阶段时需要修改此版本号。此版本号由项目决定是否修改。

##### 4. 文件命名规范

文件名称由四部分组成：第一部分为项目名称，第二部分为文件的描述，第三部分为当前软件的版本号，第四部分为文件阶段标识加文件后缀，例如：项目外 包平台测试报告 `1.1.1.051021_beta_b.xls`，此文件为项目外包平台的测试报告文档，版本号为：`1.1.1.051021_beta`。

如果是同一版本同一阶段的文件修改过两次以上，则在阶段标识后面加以数字标识，每次修改数字加1，项目外包平台测试报告 `1.1.1.051021_beta_b1.xls`。

当有多人同时提交同一份文件时，可以在阶段标识的后面加入人名或缩写来区别，例如：项目外包平台测试报告 `1.1.1.051021_beta_b_LiuQi.xls`。当此文件再次提交时也可以在人名或人名缩写的后面加入序号来区别，例如：项目外包平台测试 报告 `1.1.1.051021_beta_b_LiuQi2.xls`。

##### 5. 版本号的阶段标识

软件的每个版本中包括11个阶段，详细阶段描述如下：

| 阶段名称     | 阶段标识 |
| ------------ | -------- |
| 需求控制     | a        |
| 设计阶段     | b        |
| 编码阶段     | c        |
| 单元测试     | d        |
| 单元测试修改 | e        |
| 集成测试     | f        |
| 集成测试修改 | g        |
| 系统测试     | h        |
| 系统测试修改 | i        |
| 验收测试     | j        |
| 验收测试修改 | k        |

。