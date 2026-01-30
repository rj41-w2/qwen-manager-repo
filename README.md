# 🤖 Auto-Auth Agent: Qwen-Claude Bridge

> **A self-healing, auto-updating automation tool to enable free LLM usage with Agentic Workflows.**

![Project Status](https://img.shields.io/badge/Status-Active-success)
![Platform](https://img.shields.io/badge/Platform-Windows-blue)
![License](https://img.shields.io/badge/License-MIT-green)

## 🧐 The Problem
Developers using **Claude Code Router** with local or free LLM providers (like **Qwen**) face a major bottleneck: **Session Expiry**.
* Qwen API keys expire every **4 hours**.
* Manual re-authentication requires opening a terminal, logging in via browser, and manually copying tokens to JSON config files.
* This disruption breaks long-running Agentic AI workflows.

## 💡 The Solution
**Qwen Token Manager** is a lightweight, OS-level utility that automates this entire lifecycle. It runs silently in the background, handles the browser-based login flow, extracts the fresh token, and injects it into the Claude Router configuration without user intervention.

---

## 🚀 Key Features

* **🔄 Zero-Touch Automation:** Designed to run via Windows Task Scheduler every 3h 55m (just before token expiry).
* **📡 OTA Auto-Updates:** Built-in update engine fetches the latest patches/features directly from GitHub releases.
* **🛠️ Self-Healing:** Detects "hung" CLI processes or file locks and force-kills them to ensure successful updates.
* **⚡ Resource Efficient:** Uses OS-level scheduling instead of background loops (0% RAM usage when idle).
* **📦 Portable:** Compiled as a single `.exe` with embedded batch scripts using PyInstaller.

---

## ⚙️ Installation & Setup

### Prerequisites
1.  **Qwen CLI:** Ensure you have the Qwen CLI installed via Node.js:
    ```bash
    npm install -g @modelscope/qwen-cli
    ```
2.  **Claude Code Router:** Should be installed and configured.

### Quick Start
1.  Download the latest `QwenManager.exe` from the **[Releases Section](https://github.com/rj41-w2/qwen-manager-repo/releases/download/v1.0/auto_auth1.exe)**.
2.  Place it in a safe folder (e.g., `Documents/QwenAutoAuth`).
3.  Run the app once manually to verify login.

---

## 📅 Task Scheduler Configuration (Automated Mode)

To enable 24/7 automation, set up Windows Task Scheduler as follows:

1.  Open **Task Scheduler** > **Create Task**.
2.  **General Tab:**
    * Name: `AutoQwenRefresher`
    * Check: **Run with highest privileges**.
    * Configure for: **Windows 10/11**.
3.  **Triggers Tab:**
    * Begin the task: **At log on**.
    * Repeat task every: `3 hours 55 minutes` (Type this manually).
    * Duration: **Indefinitely**.
4.  **Actions Tab:**
    * Action: **Start a program**.
    * Program/script: Browse and select `QwenManager.exe`.
    * **Add arguments:** `auto`
        > ⚠️ **Important:** The `auto` argument tells the app to run in silent mode, close the CLI window automatically, and exit after updating.
5.  **Conditions Tab:**
    * Uncheck: "Start the task only if the computer is on AC power" (allows running on battery).

---

## 🏗️ Technical Architecture

### How it works under the hood:
1.  **Trigger:** App launches via Task Scheduler argument `auto`.
2.  **Sanitization:** Checks for and kills any stuck `node.exe` or `cmd.exe` processes to prevent file locking.
3.  **Execution:** Extracts an embedded `run_qwen.bat` script from the PyInstaller bundle to a temporary directory.
4.  **Authentication:** Executes `qwen login` in a detached subprocess.
5.  **Token Parsing:** Monitors `~/.qwen/oauth_creds.json` for changes. parses the JSON to extract the Bearer `access_token`.
6.  **Injection:** Updates `~/.claude-code-router/config.json` while preserving the existing JSON structure.
7.  **Cleanup:** Force-closes the terminal window and self-terminates.

### Auto-Update Logic
The app checks `version.txt` from the GitHub repository main branch on startup. If a mismatch is found, it:
1.  Downloads the new binary.
2.  Creates a temporary batch script.
3.  Swaps the old `.exe` with the new one.
4.  Restarts the application.

---


## 🤝 Contributing
Contributions are welcome! Please open an issue or submit a pull request for any improvements.

---

**Built with ❤️ for the AI Community.**
