# NMAP — Network Mapper

> Nmap (Network Mapper) is an open-source network scanner, released in 1997. It is used for network reconnaissance and security assessment.

## Table of Contents

- [Why Nmap is needed](#why-nmap-is-needed)
- [Limitations of manual scanning methods](#limitations-of-manual-scanning-methods)
- [Use of Nmap](#use-of-nmap)
- [Host Discovery — Who is online](#host-discovery--who-is-online)
- [Port Scanning — Who is listening](#port-scanning--who-is-listening)
- [OS Detection](#os-detection--o)
- [Service and Version Detection](#service-and-version-detection--sv)
- [Aggressive Scan](#aggressive-scan--a)
- [Force Scan (Skip Host Discovery)](#force-scan--pn)
- [Timing — How Fast is Fast](#timing--how-fast-is-fast--t)
- [Parallel Probes](#parallel-probes)
- [Packet Sending Rate](#packet-sending-rate)
- [Host Timeout](#host-timeout)
- [Real Scan Examples](#real-scan-examples)
- [Verbosity](#verbosity--v)
- [Debugging](#debugging--d)
- [Saving Scan Reports](#saving-scan-reports)
- [Real Pentesting Example](#real-pentesting-example)

---

## Why Nmap is needed

Manual scanning takes too much time:

- Discover hosts using `ping`
- Find devices using `arp-scan`
- Check ports using `telnet`

## Limitations of manual scanning methods

### Ping Limitation
- Ping uses ICMP (Internet Control Message Protocol).
- If a firewall blocks ICMP packets, the host may still be alive, but it will not respond to ICMP requests.

### Arp-scan Limitation
- It only works on the local network.
- It can't discover hosts on other networks.

### Telnet Limitation
- **Very slow** — each port must be checked one by one, which is impractical for scanning 65,535 ports.
- **Limited features** — it only checks basic TCP connections and cannot perform advanced scanning.

## Use of Nmap

- Live host discovery
- Port scanning
- Service detection
- Version detection
- OS detection
- Timing control
- Output formatting

---

## Host Discovery — Who is online

**`-sn`** : Ping Scan / Host Discovery Scan

```bash
nmap -sn target
```

**Example:**
```bash
nmap -sn 192.168.1.1-10
```

This scans:
```
192.168.1.1
192.168.1.2
192.168.1.3
192.168.1.4
...
192.168.1.10
```

**More examples:**
```bash
nmap -sn example.com
nmap -sn 192.168.0.0/24
```
> `/24` means subnet mask `255.255.255.0`, i.e. `192.168.0.0`–`192.168.0.255` — scans the whole network.

### Local network scanning

Assume your IP is `192.168.66.89` and the network is `192.168.66.0/24`:

```bash
nmap -sn 192.168.66.0/24
```

**How does Nmap determine which hosts are online?**
- By using ARP. If the target replies to the request, that means the target is online.

```bash
nmap -sL 192.168.0.1/24      # Scan the list (no actual scanning)
```

### Host discovery probe types

| Flag | Meaning |
|------|---------|
| `-PS[portlist]` | TCP SYN host discovery |
| `-PA[portlist]` | TCP ACK host discovery |
| `-PU[portlist]` | UDP packet host discovery |

```bash
nmap -sn -PS80,443 192.168.1.0/24
```
> Nmap sends a SYN packet to ports 80 and 443.

---

## Port Scanning — Who is listening

TCP and UDP both have ports ranging from **1–65535**.

Manually, you could check with telnet:
```bash
telnet 192.168.1.10 22
telnet 192.168.1.10 80
telnet 192.168.1.10 53
```
Doing this for all 65,535 ports is impractical — that's why Nmap automates this scan.

### Connect Scan (`-sT`)
- A basic TCP scan.
```bash
nmap -sT target
```
- It completes the full **TCP three-way handshake**:
  1. Client sends **SYN**
  2. Server sends **SYN-ACK**
  3. Client sends **ACK**

If the connection is established, that means the port is **open**.

### SYN Scan (`-sS`)
- Also known as **Stealth Scan** or **Half-open Scan**.
```bash
nmap -sS target
```
- The key difference: it does **not** complete the three-way handshake.

SYN Scan sequence:
1. **SYN**
2. **SYN-ACK**
3. **RST** *(instead of ACK — Nmap breaks the connection)*

> **Benefit:** This stealth scan generates fewer logs.

### UDP Scan (`-sU`)
Uses UDP (User Datagram Protocol) — connectionless. Common UDP services:

| Service | Port |
|---------|------|
| DNS | 53 |
| DHCP | 67/68 |
| NTP | 123 |
| SNMP | 161 |
| VOIP | — |

```bash
nmap -sU target
```
> **Note:** Nmap does not scan all ports by default — only the **1000 most common ports**.

### Fast Scan (`-F`)
```bash
nmap -F target
```
Scans only **100 ports**.

### Port Range (`-p`)
Frequently used to limit the scan to a range:
```bash
nmap -p10-1024 target
```
Scans ports 10 to 1024.

---

## OS Detection (`-O`)

To identify the operating system of the target machine:
```bash
nmap -sS -O 192.168.124.211
```
- `-sS` → Stealth scan
- `-O` → OS detection

## Service and Version Detection (`-sV`)

To determine which software and version are running on open ports:
```bash
nmap -sS -sV 192.168.18.1
```

**Why is this information important?**
> Because if a software version is old, exploits may be available for it.

## Aggressive Scan (`-A`)

For a lot of information in a single command:
```bash
nmap -A 192.168.124.211
```

`-A` enables:
- OS detection (`-O`)
- Version detection (`-sV`)
- Script scanning (`-sC`)
- Traceroute
- Additional detection

> This is an all-in-one reconnaissance option, equivalent in spirit to:
```bash
nmap -sS -sV -O -Pn 192.168.124.211
```

---

## Force Scan (`-Pn`)

Normally, before scanning, Nmap performs **host discovery** first. If there's no reply to the **ICMP ping**, Nmap won't continue scanning.

**Problem:**
- Many servers block **ICMP** via firewall.
- So they don't respond to ping requests.
- But the host may still be online.

**Solution:**
```bash
nmap -Pn 192.168.124.211
```
> `-Pn` tells Nmap to **skip host discovery and assume the host is up**.

---

## Timing — How Fast is Fast (`-T`)

Nmap provides **6 predefined timing templates**:

| Flag | Name | Speed |
|------|------|-------|
| `-T0` | Paranoid | Very slow |
| `-T1` | Sneaky | Slow |
| `-T2` | Polite | Slightly slow |
| `-T3` | Normal | Default |
| `-T4` | Aggressive | Fast |
| `-T5` | Insane | Very fast |

> These templates matter because scanning too quickly can make **IDS/IPS flag the activity as suspicious**.

### `-T0` (Paranoid)
```bash
nmap -sS -T0 192.168.1.10
```
- Nmap waits a very long time between packets.
- In lab environments, scanning 100 ports may take about **9.8 minutes**.
- Used to avoid detection by IDS/IPS.

### `-T1` (Sneaky)
```bash
nmap -sS -T1 192.168.1.10
```
- Slightly faster than `-T0`.
- In the lab, it took around **27.5 minutes**.
- Keeps approximately a **15-second delay** between two ports.

### `-T2` (Polite)
```bash
nmap -sS -T2 192.168.1.10
```
- Used on shared networks.
- Places less load on the network.

### `-T3` (Normal)
```bash
nmap -sS -T3 192.168.1.10
```
- Default timing template.

### `-T4` (Aggressive)
```bash
nmap -sS -T4 192.168.1.10
```
- Fast and practical option.
- Commonly used in CTFs, labs, and internal penetration testing.

### `-T5` (Insane)
```bash
nmap -sS -T5 192.168.1.10
```
- Maximum speed.
- May produce inaccurate results on slow or unstable networks.
- Packet loss may occur.

---

## Parallel Probes

Nmap can probe multiple ports simultaneously — this behavior can be controlled.

**Minimum Parallelism**
```bash
nmap --min-parallelism 20 192.168.1.10
```
> At least **20 probes** will be sent simultaneously.

**Maximum Parallelism**
```bash
nmap --max-parallelism 100 192.168.1.10
```
> At most **100 probes** will be sent simultaneously.

By default:
- Slow networks → lower parallelism
- Fast networks → higher parallelism

---

## Packet Sending Rate

You can directly control how many packets Nmap sends per second.

**Minimum Rate**
```bash
nmap --min-rate 300 192.168.1.10
```
> Nmap will send at least **300 packets per second**.

**Maximum Rate**
```bash
nmap --max-rate 1000 192.168.1.10
```
> Nmap will send at most **1000 packets per second**.

---

## Host Timeout

Sometimes a host responds very slowly. To limit how long Nmap waits:
```bash
nmap --host-timeout 5m 192.168.1.10
```
> Nmap will stop scanning a host if it takes longer than **5 minutes**.

---

## Real Scan Examples

**Fast Internal Network Scan**
```bash
nmap -sS -T4 --min-rate 1000 192.168.1.0/24
```

**Stealthy Scan**
```bash
nmap -sS -T0 192.168.1.20
```

**Skip Slow Hosts**
```bash
nmap -sS --host-timeout 2m 192.168.1.0/24
```

---

## Verbosity (`-v`)

Sometimes Nmap scans take a long time with nothing appearing on screen:
```bash
nmap -sS 192.168.139.1/24
```
Here, Nmap silently performs the scan and shows results only at the end.

If you want to see:
- What Nmap is currently doing
- Which phase the scan is in
- Which hosts have been discovered
- Which ports have been found open

Use:
```bash
nmap -sS 192.168.139.1/24 -v
```

More `v` characters = more information:
```bash
nmap -vv 192.168.139.1/24
nmap -vvv 192.168.139.1/24
```

---

## Debugging (`-d`)

To see Nmap's internal working, use debugging mode:
```bash
nmap -d 192.168.1.10
```

Maximum debugging level:
```bash
nmap -d9 192.168.1.10
```

---

## Saving Scan Reports

Saving scan results is very important during penetration testing.

**Reasons:**
- Reporting
- Documentation
- Future analysis
- Evidence collection

### 1. Normal Output (`-oN`)
Saves a human-readable report.
```bash
nmap -sS 192.168.1.10 -oN scan.txt
```

### 2. XML Output (`-oX`)
XML format is useful for tools and automation.
```bash
nmap -sS 192.168.1.10 -oX scan.xml
```

### 3. Grepable Output (`-oG`)
Useful for filtering and scripting.
```bash
nmap -sS 192.168.1.10 -oG scan.gnmap
```

### 4. Save in All Formats (`-oA`)
```bash
nmap -sS 192.168.1.10 -oA scan
```
This saves results in three files:
- `scan.nmap`
- `scan.xml`
- `scan.gnmap`

---

## Real Pentesting Example

```bash
nmap -sS -sV -O -T4 -v -oA internal-scan 10.10.10.0/24
```

This command performs:
- SYN scan (`-sS`)
- Service and version detection (`-sV`)
- OS detection (`-O`)
- Fast scan using aggressive timing (`-T4`)
- Verbose output (`-v`)
- Saves results in all formats (`-oA`)
