
# Issue: DNS Resolution Stopped Working on Laptop (Home Network)

## Problem Overview

After a recent change or update, the laptop on the home network was unable to resolve domain names (e.g., `minipc.home.lab`, `adguard.home.lab`) via the configured DNS server. The following symptoms were observed:

- The laptop could only access services via IP address and not by hostname.
- Other devices on the network (e.g., mobile phones) were able to resolve domain names without issue.
- The `/etc/resolv.conf` file on the laptop was not being updated to reflect the correct DNS settings, despite the laptop being configured to use `192.168.10.95` as the DNS server.

## Root Cause

The main issue stemmed from **NetworkManager** being misconfigured and not applying the DNS settings properly. Additionally, the file `/etc/resolv.conf` had the **immutable flag** set, preventing changes to the DNS configuration. 

This resulted in:
- The `/etc/resolv.conf` file pointing to incorrect or default DNS servers.
- NetworkManager not properly managing DNS settings as expected.

## Solution

### Step 1: Verify and Remove Immutable Flag from `/etc/resolv.conf`

1. Check if the immutable flag is set on `/etc/resolv.conf`:
   ```bash
   lsattr /etc/resolv.conf
   ```

   If you see `i` in the output (e.g., `----i--------`), it indicates that the file is immutable.

2. Remove the immutable flag:
   ```bash
   sudo chattr -i /etc/resolv.conf
   ```

### Step 2: Update `/etc/resolv.conf` to Use Correct DNS Server

1. Remove the current `/etc/resolv.conf`:
   ```bash
   sudo rm -f /etc/resolv.conf
   ```

2. Create a symbolic link to `/run/NetworkManager/resolv.conf`:
   ```bash
   sudo ln -s /run/NetworkManager/resolv.conf /etc/resolv.conf
   ```

### Step 3: Configure NetworkManager DNS Settings

1. Use `nmcli` to modify the network connection and set DNS to `192.168.10.95`:
   ```bash
   sudo nmcli connection modify "EL REGUETON ES DE PRINGAOS" ipv4.ignore-auto-dns yes
   sudo nmcli connection modify "EL REGUETON ES DE PRINGAOS" ipv4.dns "192.168.10.95"
sudo nmcli connection up "EL REGUETON ES DE PRINGAOS"
   ```

> [!NOTE]
> If nmcli complains about password, add the `--ask` flag to `connection up`

1. Restart the NetworkManager to apply changes:
   ```bash
   sudo systemctl restart NetworkManager
   ```

### Step 4: Verify DNS Resolution

1. Check the contents of `/etc/resolv.conf`:
   ```bash
   cat /etc/resolv.conf
   ```

2. Test DNS resolution by pinging a domain name:
   ```bash
   ping adguard.home.lab
   ```

   You should now be able to resolve the domain and receive responses, confirming that the issue is fixed.

## Conclusion

By removing the immutable flag, reconfiguring NetworkManager to manage DNS, and creating the correct symlink for `/etc/resolv.conf`, DNS resolution was restored on the laptop, and it can now resolve domain names as expected.
