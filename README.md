```text
 __  __  ______  __  ____            _                      __  __                                   
|  \/  |/ ___\ \/ / |  _ \ __ _  ___| | ____ _  __ _  ___  |  \/  | __ _ _ __   __ _  __ _  ___ _ __ 
| |\/| | |    \  /  | |_) / _` |/ __| |/ / _` |/ _` |/ _ \ | |\/| |/ _` | '_ \ / _` |/ _` |/ _ \ '__|
| |  | | |___ /  \  |  __/ (_| | (__|   < (_| | (_| |  __/ | |  | | (_| | | | | (_| | (_| |  __/ |   
|_|  |_|\____/_/\_\ |_|   \__,_|\___|_|\_\__,_|\__, |\___| |_|  |_|\__,_|_| |_|\__,_|\__, |\___|_|   
                                               |___/                                 |___/               
```

MCX is a high-performance, functional package management engine designed for Desind GNU/Linux. It bridges the gap between complex functional declarations and raw system execution by converting Guix-style definitions into optimized, standalone Makefiles.

---

## Package Structure

Each release (e.g., `mcx-0.26.tar.xz`) contains the following core components:

```text
mcx-v.xx/
├── mcx.c              # Source code for the binary manager
├── converter.py       # Intelligent S-expression to Makefile engine
├── bootstrap.sh       # Automation for registry generation and compilation
└── registry/      # Database of generated Makefile recipes
    ├── 0xffff/        # Individual package directory
    │   └── 0.10.mk    # Version-specific build and fetch instructions
    └── ...            # Thousands of pre-converted packages

```

---

## How It Works

### 1. The Conversion Engine (`converter.py`)

The engine scans the local repository for `.scm` package definitions. It parses nested S-expressions to extract metadata such as source URLs, versions, and recursive dependencies. It then flattens this complexity into a deterministic `.mk` file for every package version.

### 2. Sovereign Management (`mcx`)

The C-based manager handles the high-level logic:

* **Parallel Execution:** Utilizes all available CPU cores for compilation.
* **Isolation:** Supports installation into isolated environments via `chroot`.
* **System Integration:** Links the manager to `/usr/bin/` and enables bash auto-completion.

---

بما أنك لم تقم بتضمين ملف `install.sh` داخل الحزمة المضغوطة، يجب تحديث قسم التثبيت في ملف **README.md** ليكون دليلاً كاملاً يقوم المستخدم بتنفيذه يدوياً لضمان نقل الملفات وربطها بالنظام بشكل صحيح.

إليك قسم التثبيت المحدث:

---

## Installation Steps (Manual Deployment)

Since this package is a sovereign source-to-binary distribution, follow these manual steps to integrate MCX into your system:

### 1. Extract the Package

```bash
tar -xf mcx-0.26.tar.xz
cd mcx

```

### 2. Generate the Package Registry

Run the converter engine to scan the `.scm` definitions and generate the Makefile recipes:

```bash
python3 converter.py

```

### 3. Build the Sovereign Manager

Give **MCX** the excution permissions:

```bash
chmod +x mcx
```

### 4. System-Wide Integration (The Root Actions)

To make `mcx` available as a global command and enable all system features:

* **Move to Permanent Location**: It is recommended to keep the MCX directory in a stable path:
```bash
sudo mkdir -p /opt/mcx
sudo cp -r . /opt/mcx/
cd /opt/mcx
```

* **Link Binary and Enable Auto-completion**: Run the internal setup command to create symlinks in `/usr/bin/` and activate bash completion:
```bash
sudo ./mcx install-sys
```

---

**Desind OS Project** | *Building the future of sovereign computing.*

---

## Usage

Once installed, you can build and install any package from the registry:

```bash
# Build a package
mcx build 0xffff

# Install a package to the system (Requires Root)
sudo mcx install 0xffff

```
