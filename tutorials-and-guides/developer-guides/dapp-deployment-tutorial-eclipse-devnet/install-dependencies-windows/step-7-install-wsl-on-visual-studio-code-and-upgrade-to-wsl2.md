# Step 7: Install WSL on Visual Studio Code and Upgrade to WSL2

The Windows Subsystem for Linux (WSL) allows you to run a Linux environment directly on Windows, without the overhead of a traditional virtual machine or dual-boot setup. Installing WSL and upgrading to WSL2 enhances performance and supports full system call compatibility, which is crucial for development tasks.

**Install WSL on Visual Studio Code**

1. **Enable WSL on Windows**:
   *   Before installing a Linux distribution, you must enable the Windows Subsystem for Linux feature. Open PowerShell as Administrator and run:

       ```powershell
       dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
       ```
2. **Install Your Linux Distribution of Choice (Ubuntu)**:
   *   Open the Microsoft Store and search for Ubuntu. Select the version you wish to install (e.g., Ubuntu 20.04 LTS) and click "Get" to install it.\


       <figure><img src="https://lh7-us.googleusercontent.com/yY_Oe5uhq2UlEK0QzWStLQprwsFabLzT3hOhUSOak2AVAlK0GwwrBYqe7twngpZyftIePQmo36Cz_cqgvnRi_afFfjZzCKsLo1tj94Btqg_nS3IOt1JANIvEGYiRYOG-eu-eQXLbPxRxOvgQerCHCLk" alt=""><figcaption></figcaption></figure>
   *   Once installed, launch Ubuntu from the Start menu, and you'll be prompted to create a user account and password.\


       <figure><img src="https://lh7-us.googleusercontent.com/f4ekncPDoKbr4Klpugoxa3jpAgwkv5iY1u2dzsDYHd5dAs4eVyF9RM_4TG1XCbBdYeGfwgS-Vx5iQQrBbYuxSRCKsohLkcONENL9j2NDBCprXnO4KbGwrbvN1Hf7IfUjNUy8hfcdEf-L16WLnvjmDTA" alt=""><figcaption><p>Example Output</p></figcaption></figure>

**Upgrade to WSL2**

1. **Ensure Your System Supports WSL2**:
   * WSL2 requires Windows 10 version 1903 or higher. Ensure your Windows is up to date.
2. **Download the Linux Kernel Update Package**:
   * Follow the instructions at [Microsoft's official guide](https://learn.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package) to download and install the Linux kernel update package for WSL2.
3. **Set WSL2 as Your Default Version**:
   *   Open PowerShell as Administrator and run:

       ```powershell
       wsl --set-default-version 2
       ```
   * This command sets WSL2 as the default version for any future Linux distribution installations.
4. **Verify Installation**:
   *   To confirm that WSL2 is installed and running, you can open your Linux distribution (e.g., Ubuntu) and run:

       ```bash
       uname -a
       ```
   * This command should display a Linux kernel version indicating that WSL2 is actively running.

**Install WSL Extension in VSC**

* **Navegate to "Extensions" tab in VSC and search for "WSL"; install the extension:**

<figure><img src="https://lh7-us.googleusercontent.com/yMwisx6N6GyWiLk6AlO5LkHevOHAnGb8NHqaj3aq1hM9Q-wemZODHc36WTdAqOmh4gM8WKHrv6vEquwyaGbqaCHhYEND4jQR6Lep-Jusu4mKD0dHpXongcCfKMX_D_2Uhe4IUzufzEJj9mYkYH7oRk8" alt=""><figcaption></figcaption></figure>
