# Using SSH Agent on Windows 10
2021-03-12 — Anthony Perkins

---

Windows 10 ships with OpenSSH, including the `ssh-agent` utility for caching SSH keys. This isn't
enabled by default, but takes just a few steps to enable.

In the Services panel, find the **OpenSSH Authentication Agent** service and change the startup type
to Automatic, then start the service.

Open a command prompt as your user, and run `ssh-add` to import your default SSH private key. This
will normally need to be saved as `C:\Users\username\.ssh\id_rsa` for RSA private keys.

Running `ssh-add -l` should now show your key fingerprint, and this should persist when the window
is closed and re-opened.

You should now be able to SSH to hosts without being asked for the key passphrase.

## Git on Windows

If you use Git on Windows, you will still be asked for a passphrase. This is because Git for Windows
ships with its own `ssh.exe` and uses that instead of the built-in Windows one.

To use the Windows `ssh.exe` and be able to use the agent, launch the **Edit environment variables
for your account** Control Panel tool from the search button. Create a new variable with the name
`GIT_SSH` and the value `C:\Windows\System32\OpenSSH\ssh.exe`.

---

Copyright © 2021 Anthony Perkins. Some rights reserved.
