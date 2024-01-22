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
![Untitled](https://github.com/thegr1ffyn/CY243-L/assets/95119705/cdd8a8be-8ff6-4d44-83b2-65e41d8aead1)


## Additional Questions:

### What service is running on webapp.local ?

Answer: `17613`

### What user exists on share.local ?

Answer: `hodor`

### What is the password for the user on share.local ? [Add a screenshot of how you found it]

Answer: `stark`
![Untitled 1](https://github.com/thegr1ffyn/CY243-L/assets/95119705/6020cd52-319b-4299-ac2f-ff699e18d601)


### How many udp ports are open on udp.local ?

Answer: 1 port only, `24782`

## Flag 1

After completing got the first flag

![Untitled 2](https://github.com/thegr1ffyn/CY243-L/assets/95119705/f90db2a7-bf6f-46ba-9b67-429447ea56dc)

Flag 01: `CY243L{SETUP_5e00142dc7bd6a35d5a27a0083d7cd5a}`

## Webapp.local

First did a complete nmap scan to find open port

![Untitled 3](https://github.com/thegr1ffyn/CY243-L/assets/95119705/9415af5e-b358-4c1d-8e4e-b641a2bfa53c)

now directory busting using `dirb`

![Untitled 4](https://github.com/thegr1ffyn/CY243-L/assets/95119705/17e037cd-5e6b-4117-a8c9-a972e0a26ce8)

Through fuzzing found that `/api/robots.txt` has a hint

![Untitled 5](https://github.com/thegr1ffyn/CY243-L/assets/95119705/719da77c-3d57-4fb0-aaf0-a2b682dc18b5)

Now to automate the process I wrote a python script

```python
import requests

base_url = "http://ip:port"
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

![Untitled 6](https://github.com/thegr1ffyn/CY243-L/assets/95119705/15245900-85e6-40d1-9ee0-8bf018eeefbb)

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

![Untitled 7](https://github.com/thegr1ffyn/CY243-L/assets/95119705/d66f8091-b0f5-4f8c-a5a2-c392914e94c2)

By doing an smbclient scan to look for sharenames inside `share.local` we get the following.

![Untitled 8](https://github.com/thegr1ffyn/CY243-L/assets/95119705/10fd3822-b89f-4e9f-815b-db40afa65255)

By opening into the `share.local/share` we open it using `kali` password and get flag 2

![Untitled 9](https://github.com/thegr1ffyn/CY243-L/assets/95119705/7d44b8df-a040-4768-b021-96a5323bd560)

By running the following we recover two `.txt` files from the smb

```jsx
smb: \> get readme.txt
smb: \> get flag.txt
```

![Untitled 10](https://github.com/thegr1ffyn/CY243-L/assets/95119705/dc4a9c92-1dec-4a15-af1e-c9c2abe0e6f2)

Flag 1: `CY243L{SAMBA1_47488c47541ecc06ec88fc10c7243e20}`

### Part 2

For second part, we had already found credentials in `web.local` 
By base64 decode we get the credentials
![Untitled 11](https://github.com/thegr1ffyn/CY243-L/assets/95119705/b934653a-6437-45de-ab42-7eb9bbeaaa79)


Credentials: `hodor:stark`

Login to samba sample using following command

```jsx
kali@kali:~$ smbclient //share.local/secure-share -U hodor%stark
```

Got access and did `get readme.txt` and opened it to find the second flag

![Untitled 12](https://github.com/thegr1ffyn/CY243-L/assets/95119705/991192b0-e5df-46cf-a7c3-fe0dcdd60a3b)

Flag 2: `CY243L{SAMBA_ADMIN_7c641b7daa7c3611c35bd02f50f05875}`

## UDP.local

I did a udp scan using nmap

```python
sudo nmap -sU --min-rate 5000 -p- udp.local
```

Here `-sU` is UDP scan, `--min-rate 5000` is used to evade any firewall or security setup that blocks packets since UDP scan takes alot of time, `-p-` is for all 1-65535 ports.

I got this

![Untitled 13](https://github.com/thegr1ffyn/CY243-L/assets/95119705/0da69e7b-7674-4736-aa18-5682c923440c)

Now to connect using `nc` I did following command

```python
kali@kali:~$ nc -u udp.local 24782
```

It displayed this.

![Untitled 14](https://github.com/thegr1ffyn/CY243-L/assets/95119705/cbc2433e-cac1-4182-be8e-12f04a3d4b05)

By inputting copied `flag` it gave me the flag:
![Untitled 15](https://github.com/thegr1ffyn/CY243-L/assets/95119705/1c906dcc-283c-4f92-bce2-bae240f1448b)


Flag: `CY243L_UDP{jw34ibgtbjgtlpyzaq27g8v7uykh3fe7}`
