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

## Main config file

Start Notepad as administrator, and paste the following into it:

```
options {
	directory "C:\Program Files\ISC BIND 9\etc";
	dnssec-validation auto;
	listen-on { any; };
	listen-on-v6 { any; };
};

zone "."                    { type hint;   file "named.root"; };
zone "0.in-addr.arpa"       { type master; file "db.0";       };
zone "127.in-addr.arpa"     { type master; file "db.127";     };
zone "255.in-addr.arpa"     { type master; file "db.255";     };
zone "localhost"            { type master; file "db.local";   };

# Edit the lines below for any zones you use.
zone "10.in-addr.arpa"      { type master; file "db.empty";   };
zone "16.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "17.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "18.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "19.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "20.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "21.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "22.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "23.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "24.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "25.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "26.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "27.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "28.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "29.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "30.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "31.172.in-addr.arpa"  { type master; file "db.empty";   };
zone "168.192.in-addr.arpa" { type master; file "db.empty";   };
```

Change the **listen-on** and **listen-on-v6** lines as appropriate. For example, to listen only on
localhost, set them to:

````
listen-on { 127.0.0.1; };
listen-on-v6 { ::1; };
````

Save this file with **ANSI** encoding as `C:\Program Files\ISC BIND 9\etc\named.conf`.

## Required zone files

You will also need to create the following files in the same way:

### db.0

```
$TTL	604800
@	IN	SOA	localhost. root.localhost. (
			      1		; Serial
			 604800		; Refresh
			  86400		; Retry
			2419200		; Expire
			 604800 )	; Negative Cache TTL
;
@	IN	NS	localhost.
```

### db.127

```
$TTL	604800
@	IN	SOA	localhost. root.localhost. (
			      1		; Serial
			 604800		; Refresh
			  86400		; Retry
			2419200		; Expire
			 604800 )	; Negative Cache TTL
;
@	IN	NS	localhost.
1.0.0	IN	PTR	localhost.
```

### db.255

```
$TTL	604800
@	IN	SOA	localhost. root.localhost. (
			      1		; Serial
			 604800		; Refresh
			  86400		; Retry
			2419200		; Expire
			 604800 )	; Negative Cache TTL
;
@	IN	NS	localhost.
```

### db.empty

```
$TTL	86400
@	IN	SOA	localhost. root.localhost. (
			      1		; Serial
			 604800		; Refresh
			  86400		; Retry
			2419200		; Expire
			  86400 )	; Negative Cache TTL
;
@	IN	NS	localhost.
```

### db.local

```
$TTL	604800
@	IN	SOA	localhost. root.localhost. (
			      2		; Serial
			 604800		; Refresh
			  86400		; Retry
			2419200		; Expire
			 604800 )	; Negative Cache TTL
;
@	IN	NS	localhost.
@	IN	A	127.0.0.1
@	IN	AAAA	::1
```

## Root hints file

Download the **Root Hints File** [from IANA](https://www.iana.org/domains/root/files) and save it as
`C:\Program Files\ISC BIND 9\etc\named.root`. This file can be downloaded directly from
https://www.internic.net/domain/named.root or you can generate the output with `dig NS . >
named.root` from Command Prompt (if you run dig from PowerShell, you will need to change the
encoding of the output file from **UTF-16 LE** to **ANSI**).

## Final steps

Finally, change the security properties of the `C:\Program Files\ISC BIND 9` directory to grant
Modify permissions to the `.\named` user. You will now be able to start ISC BIND from the Services
utility.

---

Copyright © 2021 Anthony Perkins. Some rights reserved.
