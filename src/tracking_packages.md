# Tracking System packages

In order to list and control all installed packages on your newly installed Linux system you have to enter this command after you login to your system for very first time .

```bash
sudo dpkg-query -W -f='${Package} ${Version}\n' > fresh_installed_packages.txt
```

To list all installed packages along with their versions, use:  

```sh
dpkg-query -W -f='${Package} ${Version}\n' > fresh_installed_packages.txt
```

### **Steps to Compare Installed Packages and Versions**

1. **Run the above command** on your fresh **Debian 12.8** system and transfer `fresh_installed_packages.txt` to your affected laptop.

2. **On your affected laptop, run:**
   ```sh
   dpkg-query -W -f='${Package} ${Version}\n' > affected_installed_packages.txt
   ```

3. **Compare the two files:**
   ```sh
   diff fresh_installed_packages.txt affected_installed_packages.txt
   ```
   - Lines with `<` are present only in the fresh install.
   - Lines with `>` are present only in the affected system or have different versions.

### **Find Packages with Different Versions**

```sh
comm -3 <(sort fresh_installed_packages.txt) <(sort affected_installed_packages.txt)
```

🔹 This will show differences in installed packages and versions.

