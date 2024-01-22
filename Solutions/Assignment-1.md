# Assignment-1

```jsx
Course: CY243L - Penetration Testing - Lab
Date: 18/10/2023
FlagsSubmitted: 5/5
```

## Found Flags

Setup Flag: `CY243L{SETUP_5e00142dc7bd6a35d5a27a0083d7cd5a}`

WebApp Flag: `CY243L{WEB_4b02f09827f641f7e87d15c13ad02b09}`

Share.local Flag 1: `CY243L{SAMBA1_47488c47541ecc06ec88fc10c7243e20}`

Share.local Flag 2:`CY243L{SAMBA_ADMIN_7c641b7daa7c3611c35bd02f50f05875}`

UDP.local Flag: `CY243L_UDP{jw34ibgtbjgtlpyzaq27g8v7uykh3fe7}`

## Lab Setup

Setup the lab using 

```jsx
chmod +x start.sh
sudo ./start.sh
```

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled.png)

## Additional Questions:

### What service is running on webapp.local ?

Answer: `17613`

### What user exists on share.local ?

Answer: `hodor`

### What is the password for the user on share.local ? [Add a screenshot of how you found it]

Answer: `stark`

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%201.png)

### How many udp ports are open on udp.local ?

Answer: 1 port only, `24782`

## Flag 1

After completing got the first flag

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%202.png)

Flag 01: `CY243L{SETUP_5e00142dc7bd6a35d5a27a0083d7cd5a}`

## Webapp.local

First did a complete nmap scan to find open port

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%203.png)

now directory busting using `dirb`

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%204.png)

Through fuzzing found that `/api/robots.txt` has a hint

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%205.png)

Now to automate the process I wrote a python script

```python
import requests

base_url = "http://10.10.18.17:17613"
directory = "/musicnews/"

for i in range(100):
    flag_url = f"{base_url}{directory}flag{str(i).zfill(2)}.txt"

    response = requests.get(flag_url)
    if response.status_code == 200:
        print(f"Flag found: {response.text}")
        break
    else:
        print(f"Flag not found at {flag_url}")
else:
    print("Flag not found in the specified range (00 to 99).")
```

Now saving this as `[script](http://script.sh).py` and running it iterates 00-100 to look for flag

Finally got flag:

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%206.png)

Flag: `CY243L{WEB_4b02f09827f641f7e87d15c13ad02b09}`

## Share.local

We start by doing an nmap scan

```jsx
kali@kali:~$ nmap -sCV -v -oN tcp share.local 
Starting Nmap 7.80 ( https://nmap.org ) at 2023-10-18 12:08 UTC
```

Got this in results

```jsx
PORT    STATE SERVICE     VERSION
139/tcp open  netbios-ssn Samba smbd 4.6.2
445/tcp open  netbios-ssn Samba smbd 4.6.2

Host script results:
| smb2-security-mode: 
|   2.10: 
|_    Message signing enabled but not required
|_smb2-time: Protocol negotiation failed (SMB2)
```

I started the enumeration with SMB and found some inaccessible SMB shares.

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%207.png)

By doing an smbclient scan to look for sharenames inside `share.local` we get the following.

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%208.png)

By opening into the `share.local/share` we open it using `kali` password and get flag 2

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%209.png)

By running the following we recover two `.txt` files from the smb

```jsx
smb: \> get readme.txt
smb: \> get flag.txt
```

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%2010.png)

Flag 1: `CY243L{SAMBA1_47488c47541ecc06ec88fc10c7243e20}`

### Part 2

For second part, we had already found credentials in `web.local` 

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%201.png)

By base64 decode we get the credentials

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%2011.png)

Credentials: `hodor:stark`

Login to samba sample using following command

```jsx
kali@kali:~$ smbclient //share.local/secure-share -U hodor%stark
```

Got access and did `get readme.txt` and opened it to find the second flag

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%2012.png)

Flag 2: `CY243L{SAMBA_ADMIN_7c641b7daa7c3611c35bd02f50f05875}`

## UDP.local

I did a udp scan using nmap

```python
sudo nmap -sU --min-rate 5000 -p- udp.local
```

Here `-sU` is UDP scan, `--min-rate 5000` is used to evade any firewall or security setup that blocks packets since UDP scan takes alot of time, `-p-` is for all 1-65535 ports.

I got this

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%2013.png)

Now to connect using `nc` I did following command

```python
kali@kali:~$ nc -u udp.local 24782
```

It displayed this.

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%2014.png)

By inputting copied `flag` it gave me the flag:

![Untitled](Assignment-1%20abed4cf22beb467a8c3e6df05ed5b3b1/Untitled%2015.png)

Flag: `CY243L_UDP{jw34ibgtbjgtlpyzaq27g8v7uykh3fe7}`