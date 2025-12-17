Installation & Setup (Termux)

The 4KMobileGaming multi‑platform project includes an automated installer designed to audit, repair, and install all Node.js dependencies across the entire project tree. These instructions walk you through preparing Termux, creating the installer script, and running the full setup.

---

1. Prerequisites

Before beginning, ensure the following are installed or available:

- Termux (latest version)  
- Node.js & npm  
- Adequate storage space  
- A stable internet connection  

Install Node.js if needed:

`bash
pkg update && pkg upgrade
pkg install nodejs
`

---

2. Download and Extract the Project

1. Download the software package to your device.  
2. Extract the archive using Termux or a file manager.  
3. Navigate into the project directory:

`bash
cd multi-platform-project
`

---

3. Create the Installer Script

Inside the multi-platform-project directory, create a new script:

`bash
nano install-all.sh
`

Paste the following:

`bash

!/data/data/com.termux/files/usr/bin/bash

ROOT_DIR="$(pwd)"

echo "Scanning for package.json files under: $ROOT_DIR"

Find all package.json files recursively
find "$ROOTDIR/nodemodules" -type f -name "package.json" | while read -r pkg; do
    DIR="$(dirname "$pkg")"
    echo "--------------------------------------"
    echo "Processing: $DIR"
    echo "--------------------------------------"

    cd "$DIR" || continue

    echo "Running npm audit..."
    npm audit

    echo "Attempting npm audit fix..."
    npm audit fix --force

    echo "Installing dependencies..."
    npm install --no-audit --no-fund
done

echo "All audits and installations complete."
`

Save and exit (CTRL + O, ENTER, CTRL + X).

Make it executable:

`bash
chmod +x install-all.sh
`

---

4. Run the Installer

From inside the project directory:

`bash
./install-all.sh
`

The script will:

- Recursively scan all nested package.json files  
- Run npm audit on each module  
- Apply forced fixes  
- Reinstall dependencies as needed  

This process may take several minutes. Allow it to complete without interruption.

---

5. Completion

Once the script finishes, all audits and dependency installations are complete. You can now proceed with running or developing the project.

---

Troubleshooting

Installer won’t run (permission denied)
Make sure the script is executable:

`bash
chmod +x install-all.sh
`

npm or node not found
Install or update Node.js:

`bash
pkg install nodejs
`

npm audit or npm install fails
Common causes include storage restrictions or low disk space.

Try:

`bash
termux-setup-storage
`

Then re-run the installer.

Script exits early
You can safely run it again:

`bash
./install-all.sh
`

No package.json files found
Install top‑level dependencies first:

`bash
npm install
`

Then run the installer again.

Slow installation
This is normal on older devices. Let the script finish.

Corrupted extraction
Re-extract the project and ensure the folder structure is intact.

---

FAQ

Do I need root access?
No. Everything works in a standard Termux environment.

Why audit nested modules?
Many dependencies contain their own package.json files. The script ensures every module is checked and repaired.

Can I interrupt the installer?
It’s best not to. Interruptions may leave dependencies partially installed.

Is npm audit fix --force safe?
Yes — it’s used intentionally to ensure compatibility across mobile environments.

How do I reinstall everything from scratch?
Delete node_modules and run:

`bash
npm install
./install-all.sh
`

Does this work on all Android devices?
Most modern devices work well, but older hardware may struggle with large dependency trees.

---

Known Issues

Long installation times
Expected on mid‑range or older devices.

npm warnings
Deprecation or peer‑dependency warnings are normal and not harmful.

Missing node_modules
Re-extract the project if directories are missing.

Storage permission issues
Run:

`bash
termux-setup-storage
`

Network‑related failures
Retry once your connection stabilizes.

---

Performance Tips for Termux

Keep Termux updated
`bash
pkg update && pkg upgrade
`

Use internal storage
Avoid SD cards for faster I/O.

Close background apps
Frees RAM for Node.js operations.

Increase Node memory (optional)
`bash
export NODE_OPTIONS="--max-old-space-size=2048"
`

Use a stable internet connection
Improves dependency installation speed.

Avoid multiple Termux sessions
Prevents conflicts and slowdowns.

Reboot before installation
Helps ensure maximum available resources.
