# SIGNATE Competition Setup Guide for Google Colab (Updated for CLI v0.10.4+)

This guide explains how to set up the SIGNATE API on Google Colab to download data and submit results for the competition **[092375ab3c4a43c18c8277e1fd264aa9]**.

**Note:** The SIGNATE CLI has been updated (v0.10.4+). Older guides referencing `signate list` or `signate files` are outdated. The new CLI uses a **Key-based** system (Competition Key -> Task Key -> File Key).

## 1. Prerequisites

1.  **SIGNATE Account**: Ensure you have an account on [SIGNATE](https://signate.jp/).
2.  **API Token**:
    *   Go to your [Account Settings](https://signate.jp/account_settings).
    *   Scroll to the API Token section.
    *   Click "Create New Token" (if you haven't already) and download `signate.json`.
3.  **Join the Competition**: You *must* click the "Participate" button on the competition page in your browser before using the API.

## 2. Installation on Google Colab

Run the following cell in your Colab notebook to install the CLI:

```python
!pip install signate
```

## 3. Authentication (Handling `signate.json`)

You need to place `signate.json` in the `~/.signate/` directory.

**Option A: Upload directly (Easiest for one-off sessions)**
1.  Upload `signate.json` to the Colab file browser (drag and drop).
2.  Run:

```python
import os

# Create the directory
os.makedirs('/root/.signate', exist_ok=True)

# Move the uploaded file
!mv signate.json /root/.signate/
!chmod 600 /root/.signate/signate.json
```

**Option B: Mount Google Drive (Recommended for persistent use)**
1.  Upload `signate.json` to your Google Drive (e.g., in a folder named `My Drive/Signate/`).
2.  Mount Drive and link the file:

```python
from google.colab import drive
drive.mount('/content/drive')

import os
os.makedirs('/root/.signate', exist_ok=True)

# Copy or symlink (adjust path as needed)
!cp /content/drive/MyDrive/Signate/signate.json /root/.signate/
!chmod 600 /root/.signate/signate.json
```

## 4. Downloading Data

The new CLI uses a hierarchy: **Competition** -> **Task** -> **File**.
Your Competition Key is: `092375ab3c4a43c18c8277e1fd264aa9`.

### Step 4.1: Get the Task Key
First, find the task associated with the competition.

```python
!signate task-list --competition_key=092375ab3c4a43c18c8277e1fd264aa9
```

**Output Example:**
```text
public_key                        task_name
--------------------------------  -----------
<TASK_KEY_HERE>                   Task Name
```
Copy the `public_key` from the output. This is your **Task Key**.

### Step 4.2: List Files
Use the Task Key to list available files.

```python
# Replace <TASK_KEY> with the actual key from the previous step
!signate file-list --task_key=<TASK_KEY>
```

**Output Example:**
```text
public_key                        file_name              title
--------------------------------  ---------------------  ---------------------
<FILE_KEY_1>                      train.csv              Training Data
<FILE_KEY_2>                      test.csv               Test Data
<FILE_KEY_3>                      sample_submit.csv      Sample Submission
```
Copy the `public_key` for each file you need (e.g., train, test, sample).

### Step 4.3: Download Files
Download files individually using their File Keys.

```python
# Create a data directory
!mkdir -p data

# Download specific files (Replace <TASK_KEY> and <FILE_KEY>)
!signate download --task_key=<TASK_KEY> --file_key=<FILE_KEY_1> --path=./data/
!signate download --task_key=<TASK_KEY> --file_key=<FILE_KEY_2> --path=./data/
```

## 5. Submitting Results

To submit your predictions, use the `submit` command with the Task Key.

```python
# Replace <TASK_KEY> and your filename
!signate submit --task_key=<TASK_KEY> ./submission.csv --memo "My first submission"
```

## 6. Security Note

*   **Never commit `signate.json` to a public repository.**
*   In this project, `.gitignore` has been configured to ignore `signate.json`.
*   If using Google Colab with GitHub integration, ensure you don't accidentally save the `signate.json` file into the repo directory before pushing. Ideally, keep it in `/root/.signate/` (outside the repo) or on Google Drive.
