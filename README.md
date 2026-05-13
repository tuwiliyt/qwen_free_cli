# Qwen Code CLI Starter

Minimal template for running `Qwen Code CLI` as a coding agent via OpenAI-compatible API.

The main idea: the `qwen_free_cli` folder is added inside any project, and `Qwen Code` is launched from it to work with the parent project's files.

## What you get

After setup, you can open the project in PyCharm, VS Code, Google Colab Terminal, or a regular terminal and run:

```bash
./qwen_free_cli/scripts/run_qwen_code.sh
```

Then you can give the agent regular tasks:

```text
Explain the project structure.
Jelaskan struktur proyek.
```

```text
Create a folder ./demo and a file ./demo/test.txt with text "hello".
Buat folder ./demo dan file ./demo/test.txt dengan teks "halo".
```

```text
Create a file ./src/main.py with a simple main() function and explain what you did.
Buat file ./src/main.py dengan fungsi main() sederhana dan jelaskan apa yang Anda lakukan.
```

## How it works

`Qwen Code CLI` is not the model itself. It is a terminal agent for programming.

The scheme is like this:

```text
Qwen Code CLI
  -> runs from ./qwen_free_cli
  -> takes access_token from ./qwen_free_cli/credentials.json
  -> connects to https://qwen.aikit.club/v1
  -> uses model qwen3.6-plus
  -> reads and changes files of the parent project
```

We do not use `Qwen OAuth` in the IDE window. The launch goes through `--auth-type openai`, so the CLI gets the key directly from the project.

## Installation from GitHub

Create or open your project folder:

```bash
mkdir my_project
cd my_project
```

Clone `qwen_free_cli` inside the project:

```bash
git clone https://github.com/Staks-sor/qwen_free_cli.git
```

Install `Qwen Code CLI`:

```bash
npm install -g @qwen-code/qwen-code@latest
```

Create a Python environment in the root of your project:

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r qwen_free_cli/requirements.txt
```

On Windows PowerShell:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
py -m pip install -r qwen_free_cli\requirements.txt
```

Important: `.venv` is created in the root of your project, and dependencies are taken from `qwen_free_cli/requirements.txt`.

### Google Colab Setup

To run in Google Colab Terminal:

1. Mount your Google Drive (if using Drive):
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```
2. Navigate to your project folder:
   ```bash
   cd /content/drive/MyDrive/your_project_folder
   ```
3. Install Node.js and npm (if not already installed):
   ```bash
   !apt-get update && apt-get install -y nodejs npm
   ```
4. Install Qwen Code CLI:
   ```bash
   !npm install -g @qwen-code/qwen-code@latest
   ```
5. Set up Python environment:
   ```bash
   !python3 -m venv .venv
   !source .venv/bin/activate
   !pip install -r qwen_free_cli/requirements.txt
   ```
6. Run the agent:
   ```bash
   !chmod +x qwen_free_cli/scripts/run_qwen_code.sh
   !./qwen_free_cli/scripts/run_qwen_code.sh
   ```

## Key Configuration

Open the existing file `qwen_free_cli/credentials.json` and paste your token:

```json
{
  "sessions": [
    {
      "qwen_credentials": {
        "access_token": "PASTE_YOUR_TOKEN_HERE"
      }
    }
  ]
}
```

Paste the real token instead of `PASTE_YOUR_TOKEN_HERE`.

Do not publish `credentials.json` with a real token.

## Running the Agent

Make the script executable:

```bash
chmod +x qwen_free_cli/scripts/run_qwen_code.sh
```

Run interactive mode:

```bash
./qwen_free_cli/scripts/run_qwen_code.sh
```

For testing, you can write:

```text
Hi. Answer in Indonesian and tell me which project folder you are working in.
Halo. Jawab dalam bahasa Indonesia dan beri tahu saya di folder proyek mana Anda bekerja.
```

Then test file operations:

```text
Create a folder ./demo and a file ./demo/test.txt with text "Qwen works from the project".
Buat folder ./demo dan file ./demo/test.txt dengan teks "Qwen bekerja dari proyek".
```

Important: Use the interactive session to create and change files.

One-shot mode:

```bash
./qwen_free_cli/scripts/run_qwen_code.sh "answer in one word: test" --output-format text
```

is suitable for quick text checks, but some OpenAI-compatible endpoints may return tool-call as plain text instead of actually changing files.

## Quick API Check via Python

```bash
python3 qwen_free_cli/chat.py
```

This is not a coding agent, but a simple chat via the same API key. It is only needed to check that the token and endpoint work.

## PyCharm, VS Code, and Google Colab

This project is not tied to a specific IDE.

In PyCharm:

```bash
./qwen_free_cli/scripts/run_qwen_code.sh
```

can be run directly in the built-in terminal.

In VS Code:

```bash
./qwen_free_cli/scripts/run_qwen_code.sh
```

can be run in the built-in terminal. If there are agent extensions, they are not required: this template already works via CLI.

In Google Colab Terminal:

```bash
./qwen_free_cli/scripts/run_qwen_code.sh
```

can be run in the terminal cell or directly in the Colab terminal interface.

## What's Inside the Project

- `scripts/run_qwen_code.sh` - main launcher for `Qwen Code CLI`.
- `.qwen/settings.json` - Qwen Code settings for the project.
- `QWEN.md` - rules for the agent: bilingual (Indonesian/English) and work only inside the current folder.
- `qwen_client.py` - minimal Python client for the API.
- `chat.py` - simple terminal chat for API checking.
- `credentials.json` - file for the token with a safe placeholder.

## Security

Do not publish real tokens.

Before pushing to GitHub, check:

```bash
git status
```

Before publishing, check that `credentials.json` does not contain a real token.

## Common Errors

If `Qwen Code` asks for `Qwen OAuth`, it means the agent is not launched via this script. Launch it like this:

```bash
./qwen_free_cli/scripts/run_qwen_code.sh
```

If the agent creates files in the wrong place, ask it to explicitly specify the relative path:

```text
Create a file ./demo/test.txt
Buat file ./demo/test.txt
```

If one-shot mode returns JSON with a tool-call but the file does not appear, use the interactive session:

```bash
./qwen_free_cli/scripts/run_qwen_code.sh
```