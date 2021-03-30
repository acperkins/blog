# Running ISC BIND on Windows
2021-03-12 — Anthony Perkins

---

ISC BIND is the standard DNS server on Linux and Unix systems. It is also packaged for Windows, but
it needs some additional configuration after installation.

Download the latest Current-Stable installer for Windows from
[isc.org/download](https://www.isc.org/download/). As of the time of writing this is named
`BIND9.16.13.x64.zip`, but your version number may be higher.

Extract the ZIP file, and run `BINDInstall.exe` as Administrator.

Follow the prompts and install the service. I will assume you have used the default install path of
`C:\Program Files\ISC BIND 9` for the rest of this article. If you have used a different path, you
will need to adjust the paths below accordingly.

Start Notepad as administrator, and paste the following into it:

```
options {
	directory "C:\Program Files\ISC BIND 9\etc";
	dnssec-validation auto;
	listen-on { any; };
	listen-on-v6 { any; };
};

zone "." {
	type hint;
	file "named.root";
};
```

Change the **listen-on** and **listen-on-v6** lines as appropriate. For example, to listen only on
localhost, set them to:

````
listen-on { 127.0.0.1; };
listen-on-v6 { ::1; };
````

Save this file with **ANSI** encoding as `C:\Program Files\ISC BIND 9\etc\named.conf`.

Download the **Root Hints File** [from IANA](https://www.iana.org/domains/root/files) and save it as
`C:\Program Files\ISC BIND 9\etc\named.root`. This file can be downloaded directly from
https://www.internic.net/domain/named.root or you can generate the output with `dig NS . >
named.root` from Command Prompt (if you run dig from PowerShell, you will need to change the
encoding of the output file from **UTF-16 LE** to **ANSI**).

Finally, change the security properties of the `C:\Program Files\ISC BIND 9` directory to grant
Modify permissions to the `.\named` user. You will now be able to start ISC BIND from the Services
utility.

---

Copyright © 2021 Anthony Perkins. Some rights reserved.
