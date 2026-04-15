# ESP-IDF AI-Assisted Template

This is a generic, clean, and highly structured template for building firmware across the Espressif ESP32 family (**ESP32**, **ESP32-S3**, **ESP32-C6**) using the **ESP-IDF** framework. 

It is designed from the ground up to integrate perfectly with **VSCode** and to be managed by AI Agents (specifically **Gemini CLI / Antigravity**) using a structured System of Skills and Workflows.

---

## 📂 1. Directory Structure

- **`main/`**: The core application. Includes a basic `HelloWorldSample` demonstrating a clean, modular C++ architecture (optional, but recommended).
- **`components/`**: Place your custom, reusable hardware drivers here.
- **`.vscode/`**: Pre-configured tasks and launch configurations for building, flashing, and debugging.
- **`.agents/`**: The "brain" of the AI assistant. Contains instructions, skills, and workflows.

---

## ⚡ 2. VSCode Integration (Compilation & Debugging)

This template leverages `.vscode/tasks.json` and `.vscode/launch.json` to provide a seamless development experience without leaving the editor. 

### VSCode Tasks (Ctrl+Shift+P -> Tasks: Run Task)
- **`Set ESP-IDF Target`**: Interactive prompt to select your board (`esp32`, `esp32s3`, `esp32c6`). It automatically generates the correct `sdkconfig` for your MCU.
- **`ESP-IDF Menuconfig`**: Opens the interactive configuration menu (`idf.py menuconfig`) directly in the VSCode terminal to adjust hardware settings, RTOS config, etc.
- **`Build ESP-IDF`**: Compiles the project (`idf.py build`). *(Shortcut: `Ctrl+Shift+B`)*
- **`Flash ESP-IDF`**: Flashes the compiled binary to the board (`idf.py flash`). Automatically kills any active serial monitor to prevent port locking.
- **`Monitor ESP-IDF`**: Opens the serial monitor (`idf.py monitor`).
- **`Flash and Monitor ESP-IDF`**: Executes build, flash, and monitor sequentially.
- **`Kill Monitor`**: A utility task to forcefully close any dangling `idf_monitor` processes holding the `/dev/ttyACM*` or `/dev/ttyUSB*` ports.

### Debugging (Ctrl+Shift+D)
The `.vscode/launch.json` is pre-configured with built-in USB JTAG debug profiles for modern chips. No external hardware debugger (like ESP-Prog) is required for these:
- **`Debug (ESP32-S3)`**: Uses the Xtensa debugger and USB JTAG.
- **`Debug (ESP32-C6)`**: Uses the RISC-V debugger and USB JTAG.
Just select your target in the Run/Debug panel and hit Play to start placing breakpoints.

---

## 🤖 3. Artificial Intelligence Configuration (`.agents/`)

This project is tailored for AI-assisted development. The `.agents/` directory instructs the AI on how to write code, review it, and interact with the repository.

### 📜 A. Rules (The Orchestrator)
- **`rules/01-project-instructions.md`**: The master file. It tells the AI its tone (professional, English-only) and explicitly defines **when** to activate specific Skills or read Workflows based on your prompts.

### 🧠 B. Skills (The Experts)
Skills are specialized "hats" the agent wears for specific technical tasks:
- **`ESP32_Expert`**: Loaded when writing or refactoring code. Enforces C++20, RAII, smart pointers (`std::unique_ptr`), and a strict Doxygen-only commenting policy (no redundant inline comments).
- **`Hardware_Specialist`**: Loaded when assigning pins. Validates pinouts (avoiding strapping pins or internal JTAG pins) and checks power requirements (e.g., decoupling capacitors). *It reads MCU Datasheets you place in `.agents/docs/`.*
- **`Quality_Auditor`**: Loaded for code reviews. Performs strict technical audits before finalizing implementations, rejecting memory leaks, raw pointers, and poor architecture.

### 📋 C. Workflows (The Recipes)
Workflows are step-by-step procedures the AI follows. They are prefixed to group them logically:

**Git Workflows (`git_*`)**: Includes *oh-my-zsh* aliases (`gcb`, `grbi`, `gpsup`) alongside standard Git CLI commands.
- **`git_create_branch.md`**: Standardized branch creation (`feature/<name>`).
- **`git_squash_commits.md`**: Interactive rebase guide to clean up WIP commits.
- **`git_submit_pr.md`**: Generates a formatted Pull Request after passing the Quality Auditor.

**Technical Workflows (`skill_*`)**:
- **`skill_hardware_validation.md`**: The pre-coding check. Asks the Hardware Specialist to validate pins and electrical requirements.
- **`skill_create_sample.md`**: Instructions on how to build a new feature using the modular `ISample` architecture (if the project uses it).
- **`skill_verify_implementation.md`**: The QA check. Invokes the Quality Auditor to ensure RAII, Doxygen, and separation of concerns before flashing.

---

## 🚀 4. Getting Started

1. **Clone** or copy this template for your new ESP32 project.
2. **Add Datasheet**: Place the specific MCU Datasheet (PDF) in `.agents/docs/` so the AI can read it.
3. **Open** the project in VSCode.
4. **Configure Target**: Run the **`Set ESP-IDF Target`** task from the Command Palette to select your chip (ESP32, S3, or C6).
5. **Build**: Run the `Build ESP-IDF` task (`Ctrl+Shift+B`) to ensure your environment compiles correctly.
6. **Start Working**: Ask your AI Agent something like: *"Create a new branch to add an I2C sensor"* or *"Perform a Hardware Validation for an SD Card on my ESP32-C6"* to see the workflows in action!
