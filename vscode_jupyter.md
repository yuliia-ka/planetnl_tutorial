# VS Code, Python and Jupyter in the CBS Research Access environment

Notes on getting VS Code, Python and Jupyter running in the CBS Research Access environment for any project. Replace `<project-id>` below with your 4-digit CBS project number (for example `1234`).

The project's Python environment:

```
C:\mambaforge\envs\<project-id>\
```

In Git Bash the same folder is written as `/c/mambaforge/envs/<project-id>/`. Use the Windows form (`C:\...`) in VS Code, and the Git Bash form (`/c/...`) in Git Bash commands.

The `H:` drive is the project-wide drive: everyone in the project sees the same files there. The Jupyter commands below open Jupyter on `H:`.

**Which route to use for notebooks:**

- **A. In VS Code:** the default. Open the notebook in VS Code and select the project kernel.
- **B. In the browser:** if you prefer the classic Jupyter Notebook interface.
- **C. VS Code connected to a Jupyter server:** a fallback if A does not work.

## One-time setup

### Install extensions

1. Press `Ctrl + Shift + P`
2. Choose the install command that matches what is in `Utilities / Microsoft VS Code / extensions`:
   - `.vsix` files: **Extensions: Install from VSIX...**
   - unpacked extension folders: **Developer: Install Extension from Location**
3. Install the Python and Jupyter-related extensions

## Python scripts in VS Code

### Select the project's Python interpreter

The interpreter is the Python that VS Code uses to run `.py` files. It does not affect notebooks: they use a kernel (see below).

1. Press `Ctrl + Shift + P`
2. Choose **Python: Select Interpreter**
3. Choose **Enter interpreter path**
4. Enter:

   ```
   C:\mambaforge\envs\<project-id>\python.exe
   ```

## Notebooks

The kernel is the process that runs a notebook's cells. Each notebook needs the project kernel selected.

### A. In VS Code (default)

1. Open the notebook in VS Code
2. Click **Select Kernel** in the top-right corner
3. Choose **Python Environments**
4. Select the `<project-id>` environment if it is listed. If not, enter:

   ```
   C:\mambaforge\envs\<project-id>\python.exe
   ```

### B. In the browser (Jupyter Notebook)

1. Open VS Code
2. Open a terminal (**Terminal → New Terminal**) and choose **Git Bash** from the dropdown next to **+**
3. Run:

   ```bash
   /c/mambaforge/envs/<project-id>/Scripts/jupyter-notebook.exe --NotebookApp.notebook_dir="H:/"
   ```

   Jupyter Notebook opens in the browser, showing the `H:` drive.

### C. VS Code connected to a Jupyter server (fallback)

Use this if selecting the kernel directly (A) does not work. It starts a Jupyter server without opening a browser, and VS Code connects to it.

1. Open VS Code
2. Open a terminal (**Terminal → New Terminal**) and choose **Git Bash** from the dropdown next to **+**
3. Run:

   ```bash
   /c/mambaforge/envs/<project-id>/Scripts/jupyter-notebook.exe --no-browser --NotebookApp.notebook_dir="H:/"
   ```

4. Copy the full server URL it prints, including the `?token=...` at the end
5. Connect from VS Code. How depends on the version of the Jupyter extension:
   - **Newer versions:** open the notebook, click **Select Kernel** → **Existing Jupyter Server...**, paste the URL and press `Enter`
   - **Older versions:** press `Ctrl + Shift + P` → **Jupyter: Specify Jupyter Server for Connections** → **Existing**, paste the URL and press `Enter`

Keep the terminal open while you work: closing it stops the server.

## Troubleshooting

### Jupyter options not recognised

The commands above use `--NotebookApp.*` options, which are for Jupyter Notebook 6 and older. Check the version with:

```bash
/c/mambaforge/envs/<project-id>/Scripts/jupyter-notebook.exe --version
```

On Notebook 7 or newer, use `--ServerApp.*` instead, and `--ServerApp.root_dir` instead of `--NotebookApp.notebook_dir`.

### VS Code cannot connect to the Jupyter server (route C)

Add these options to the command in route C:

```bash
--NotebookApp.ip="0.0.0.0" --NotebookApp.allow_origin="*"
```

They make the server listen on all network interfaces and accept requests from any origin, so only use them if needed.
