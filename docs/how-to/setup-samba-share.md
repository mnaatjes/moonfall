# How-To: Set Up a Samba Share for the Parent Source Directory

This guide details how to configure a Samba (SMB) share on your Ubuntu Server (`192.168.1.146`) pointing to the parent directory `/home/hp_prodesk/src/`. This allows you to access both your Unity project (`moonfall`) and your asset pipeline (`lunar-data-pipeline`) side-by-side using a single mapped network drive in Windows 11.

---

## Step 1: Install Samba on the Ubuntu Server
Open your server terminal (or connect via SSH) and run:
```bash
sudo apt update
sudo apt install samba -y
```

---

## Step 2: Configure the Samba Share
1. Open the Samba configuration file in a text editor:
   ```bash
   sudo nano /etc/samba/smb.conf
   ```
2. Scroll to the bottom of the file and append the following share block:
   ```ini
   [src]
      comment = Source Projects Directory
      path = /home/hp_prodesk/src
      writable = yes
      guest ok = no
      valid users = hp_prodesk
      force user = hp_prodesk
      create mask = 0775
      directory mask = 0775
   ```
3. Save the file (`Ctrl+O`, then `Enter`) and exit the editor (`Ctrl+X`).

---

## Step 3: Set a Samba Password for the User
Samba uses its own password database. Create a password for your user account:
```bash
sudo smbpasswd -a hp_prodesk
```
*(Enter a secure password when prompted)*

---

## Step 4: Restart the Samba Services
Apply the configuration changes by restarting the services:
```bash
sudo systemctl restart smbd
sudo systemctl enable smbd
```

---

## Step 5: Mount the Share in Windows 11
1. Open **File Explorer** on your Windows 11 machine.
2. Right-click on **This PC** in the sidebar and select **Map network drive...**
3. Select a drive letter (e.g., `S:` for Source).
4. In the **Folder** field, enter:
   ```text
   \\192.168.1.146\src
   ```
5. Check **Reconnect at sign-in** and click **Finish**.
6. When prompted for credentials, enter:
   *   *Username:* `hp_prodesk`
   *   *Password:* The password you created in Step 3.

You will now see your `moonfall` and `lunar-data-pipeline` workspaces side-by-side on your mapped network drive.
