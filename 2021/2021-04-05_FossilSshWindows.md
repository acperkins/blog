# Fossil SSH command on Windows
2021-04-05 — Anthony Perkins

---

[Fossil](https://www.fossil-scm.org/) on Windows defaults to using `plink.exe` for SSH
communication, but this might not be installed. If the command is missing fossil will print the
error below:

    cannot create child process

To fix this, specify the path to the built-in Windows SSH client:

    fossil set --global ssh-command C:\Windows\System32\OpenSSH\ssh.exe

---

Copyright © 2021 Anthony Perkins. Some rights reserved.
