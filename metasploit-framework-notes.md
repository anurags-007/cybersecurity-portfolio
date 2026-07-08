# Metasploit Framework Notes

> Beginner-friendly notes for Metasploit Framework and `msfconsole`.

## Table of Contents

-   [Metasploit Framework](#metasploit-framework)
-   [Core Concepts](#core-concepts)
-   [Metasploit Modules](#metasploit-modules)
-   [Payload Types](#payload-types)
-   [Understanding msfconsole
    Prompts](#understanding-msfconsole-prompts)
-   [Common Parameters](#common-parameters)
-   [set vs setg](#set-vs-setg)
-   [Running Modules](#running-modules)
-   [Sessions](#sessions)
-   [Important Commands](#important-commands)
-   [Quick Workflow](#quick-workflow)
-   [Defender and SOC Perspective](#defender-and-soc-perspective)

------------------------------------------------------------------------

## Metasploit Framework

**Metasploit Framework** is an open-source penetration testing and
exploit development framework. It provides modules for:

-   Information gathering
-   Scanning and enumeration
-   Vulnerability validation
-   Exploitation
-   Payload handling
-   Post-exploitation

The main command-line interface is:

``` bash
msfconsole
```

After launching it, the prompt normally looks like:

``` text
msf6 >
```

### Mental Model

``` text
User
  ↓
msfconsole
  ↓
Metasploit Framework
  ↓
Modules
├── Auxiliary
├── Encoders
├── Evasion
├── Exploits
├── NOPs
├── Payloads
└── Post
```

------------------------------------------------------------------------

## Core Concepts

### Vulnerability

A **vulnerability** is a design, coding, logic, or configuration flaw in
a system.

``` text
System
  ↓
Weakness / Flaw
  ↓
Vulnerability
```

Examples include unsafe memory handling, weak authentication, and
improper input validation.

### Exploit

An **exploit** is code or a technique that leverages a vulnerability.

``` text
Vulnerability
      ↓
Exploit
      ↓
Security Impact
```

**Vulnerability = weakness**

**Exploit = method/code that uses the weakness**

### Payload

A **payload** is code that runs on the target after successful
exploitation to produce the intended result.

``` text
Vulnerability
      ↓
Exploit
      ↓
Payload
      ↓
Desired Result
```

Possible authorized lab outcomes include:

-   Execute a command
-   Launch a proof-of-concept application
-   Obtain a shell
-   Establish a Meterpreter session

### Shell

A **shell** is an interactive command-line connection to a system.

``` text
Tester types command
        ↓
Target executes command
        ↓
Output is returned
```

------------------------------------------------------------------------

## Metasploit Modules

### Auxiliary

Supporting modules used for tasks such as:

-   Scanning
-   Enumeration
-   Crawling
-   Fuzzing
-   Information gathering
-   Protocol interaction

Example module path:

``` text
auxiliary/scanner/ssh/ssh_login
```

Module paths provide useful context:

``` text
auxiliary → module type
scanner   → category
ssh       → protocol/service
ssh_login → module purpose
```

### Encoders

Encoders transform payload representation.

``` text
Original Payload
       ↓
Encoder
       ↓
Encoded Representation
```

Historically, encoding can also help deal with exploit constraints such
as problematic bytes.

> Encoding is not a guaranteed antivirus bypass.

Modern security tools may use behavioral analysis, heuristics, memory
inspection, sandboxing, and other detection methods.

### Evasion

Evasion modules explicitly attempt to test security-control evasion
techniques.

``` text
Encoder → transforms representation

Evasion → attempts to avoid security detection/enforcement
```

Use only in authorized environments.

### Exploits

Exploit modules leverage specific vulnerabilities.

Typical structure:

``` text
exploit/<platform>/<service>/<module>
```

Example:

``` text
exploit/windows/smb/ms17_010_eternalblue
```

Breakdown:

``` text
exploit                → module type
windows                → platform
smb                    → protocol/service
ms17_010_eternalblue   → specific exploit module
```

### NOPs

NOP means **No Operation**.

On Intel x86, `0x90` commonly represents a NOP instruction.

``` text
Instruction
NOP
NOP
Instruction
```

NOPs are associated with low-level exploit development concepts such as
padding and NOP sleds.

### Post

Post modules are used after an authorized session has been established.

They can support tasks such as:

-   System enumeration
-   User-context inspection
-   Network information gathering
-   Security configuration review
-   Impact validation

``` text
Exploit
  ↓
Payload
  ↓
Session
  ↓
Post Module
```

------------------------------------------------------------------------

## Payload Types

Metasploit payloads are organized into:

``` text
payloads/
├── adapters
├── singles
├── stagers
└── stages
```

### Adapters

Adapters wrap a single payload into another format or execution
representation.

``` text
Single Payload
      ↓
Adapter
      ↓
Different Wrapper / Format
```

### Singles

A single or inline payload is self-contained.

``` text
Exploit
  ↓
Complete Payload
  ↓
Action
```

It does not need an additional stage.

### Stagers

A stager is a small initial component responsible for establishing a
communication channel.

``` text
Exploit
  ↓
Small Stager
  ↓
Connection Channel
```

### Stages

A stage is the larger payload component delivered after the stager
establishes the channel.

``` text
Exploit
  ↓
Stager
  ↓
Channel
  ↓
Stage
  ↓
Larger Functionality
```

### Single vs Staged Payload Naming

``` text
generic/shell_reverse_tcp
```

The underscore in `shell_reverse_tcp` indicates an inline/single payload
in this naming convention.

``` text
windows/x64/shell/reverse_tcp
```

The slash between `shell` and `reverse_tcp` indicates a staged payload
structure.

### Memory Trick

``` text
_ = Single / Inline

/ = Staged
```

------------------------------------------------------------------------

## Understanding msfconsole Prompts

Prompt recognition is important because it tells you **where commands
are being executed**.

### 1. Linux Shell

``` text
root@attackbox:~#
```

Commands execute on your AttackBox or Kali Linux machine.

### 2. General Metasploit Console

``` text
msf6 >
```

You are inside `msfconsole`, but no specific module context is selected.

### 3. Module Context

``` text
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```

A module is selected and can be configured.

> Module context is not a filesystem directory.

### 4. Meterpreter Prompt

``` text
meterpreter >
```

You are interacting through an active Meterpreter session.

### 5. Target Shell

``` text
C:\Windows\system32>
```

Commands typed here execute on the target Windows shell.

### Prompt Memory Map

``` text
root@...#       → My Linux machine
msf6 >          → Metasploit console
msf6 module() > → Selected module context
meterpreter >   → Meterpreter session
C:\...>         → Target Windows shell
```

------------------------------------------------------------------------

## Common Parameters

Metasploit parameters are normally configured using:

``` text
set PARAMETER_NAME VALUE
```

Always inspect module requirements with:

``` text
show options
```

### RHOSTS

**Remote Host(s)** --- target IP address, range, CIDR network, or
supported hosts file.

``` text
set RHOSTS <authorized-lab-target>
```

Mental question:

> Which target?

### RPORT

**Remote Port** --- target-side service port.

``` text
RPORT = Target service port
```

A default value must still be verified against the actual target
service.

### PAYLOAD

The compatible payload selected for the exploit.

``` text
Exploit
  +
Payload
  =
Desired authorized test outcome
```

### LHOST

Listener or callback address used by a reverse payload.

``` text
Target
  ↓
Reverse Connection
  ↓
LHOST
```

Do not blindly assume `127.0.0.1`. The listener address must make sense
for the authorized lab network path.

### LPORT

Listener-side port.

``` text
LHOST:LPORT
```

The selected port should not conflict with another application already
using the same listening endpoint.

### SESSION

The ID of an existing Metasploit-managed connection.

``` text
Session 1 → Target A
Session 2 → Target B
```

Post modules commonly require a `SESSION` value.

### Parameter Memory Table

  Parameter   Meaning               Mental Question
  ----------- --------------------- ---------------------------------------------
  `RHOSTS`    Remote host(s)        Target kaun hai?
  `RPORT`     Remote port           Target service kis port par hai?
  `PAYLOAD`   Payload               Successful exploit ke baad kya action/code?
  `LHOST`     Listener address      Callback kahan aayega?
  `LPORT`     Listener port         Callback kis port par aayega?
  `SESSION`   Existing session ID   Kaunsi connection use karni hai?

------------------------------------------------------------------------

## set vs setg

### `set`

Sets a value in the current module/context datastore.

``` text
set RHOSTS <authorized-lab-target>
```

Mental model:

``` text
Module A
RHOSTS = configured value

Module B
RHOSTS = not automatically the same
```

### `setg`

Sets a global datastore value that can be used across modules.

``` text
setg RHOSTS <authorized-lab-target>
```

Mental model:

``` text
Global Datastore
       ↓
Module A
Module B
Module C
```

### `unset`

Clear one configured parameter:

``` text
unset RHOSTS
```

### `unset all`

Clear configured local datastore values:

``` text
unset all
```

Module defaults may still appear afterward.

### `unsetg`

Clear a global value:

``` text
unsetg RHOSTS
```

### Memory Trick

``` text
set    → Current context
setg   → Global datastore

unset  → Clear local value
unsetg → Clear global value
```

------------------------------------------------------------------------

## Running Modules

### `run`

Executes the currently selected module.

``` text
run
```

This command makes sense for exploits, scanners, and other module types.

### `exploit`

Used to execute an exploit module.

``` text
exploit
```

### `exploit -z`

Runs an exploit and backgrounds a newly opened session.

``` text
exploit -z
```

Concept:

``` text
Exploit
  ↓
Session Opens
  ↓
Session Backgrounded
  ↓
Return to Module Context
```

### `check`

Some modules support:

``` text
check
```

It attempts to assess whether the target appears vulnerable without
launching the full exploit path.

> A check result is not an absolute guarantee, and module behavior
> should be reviewed with `info`.

------------------------------------------------------------------------

## Sessions

A **session** is a communication channel established between Metasploit
and a target system in an authorized test.

``` text
Metasploit
    ↕
Session
    ↕
Target
```

### List Sessions

``` text
sessions
```

Example conceptual output:

``` text
Id  Type
1   meterpreter x64/windows
2   meterpreter x64/windows
```

### Interact With a Session

``` text
sessions -i 2
```

Breakdown:

``` text
sessions → session manager
-i       → interact
2        → session ID
```

### Background a Session

From Meterpreter:

``` text
background
```

Concept:

``` text
Active + Interacting
        ↓
background
        ↓
Active + Not Currently Interacting
```

**Backgrounding is not the same as closing the session.**

### Module Context vs Session Lifetime

``` text
Current Module Context
        ≠
Session Lifetime
```

You can leave a module context with:

``` text
back
```

and active sessions may still remain managed by Metasploit.

------------------------------------------------------------------------

## Important Commands

  Command              Purpose
  -------------------- -----------------------------------------
  `msfconsole`         Launch Metasploit console
  `help`               Display command help
  `help set`           Show help for the `set` command
  `history`            Show previously entered commands
  `search <keyword>`   Search for modules
  `use <module>`       Select a module
  `info`               Display detailed module information
  `show options`       Show module and payload options
  `show payloads`      Show compatible payloads
  `set`                Set a context-specific value
  `setg`               Set a global value
  `unset`              Clear a local value
  `unset all`          Clear configured local datastore values
  `unsetg`             Clear a global value
  `back`               Leave current module context
  `check`              Check target condition if supported
  `run`                Execute current module
  `exploit`            Execute exploit module
  `exploit -z`         Run exploit and background new session
  `sessions`           List active sessions
  `sessions -i <id>`   Interact with a session
  `background`         Background current Meterpreter session

------------------------------------------------------------------------

## Searching for Modules

Basic search:

``` text
search <keyword>
```

Modules can be searched using:

-   CVE identifiers
-   Vulnerability names
-   Exploit names
-   Protocols
-   Services
-   Platforms

Filtered search example:

``` text
search type:auxiliary telnet
```

Concept:

``` text
Module Type = Auxiliary
AND
Keyword = Telnet
```

Search results should be read using:

``` text
Type
Platform / Category
Rank
Check Support
Description
```

A search result index can also be used with `use`:

``` text
use <search-result-index>
```

The index belongs to the current search result and should not be treated
as a permanent module ID.

------------------------------------------------------------------------

## Exploit Ranking

Metasploit exploit ranks provide reliability guidance.

A commonly seen ordering is:

``` text
Excellent
   ↑
Great
   ↑
Good
   ↑
Normal
   ↑
Average
   ↑
Low
   ↑
Manual
```

Important:

``` text
Excellent ≠ Guaranteed success

Low ≠ Guaranteed failure
```

Actual behavior depends on factors such as:

-   Target version
-   Patch state
-   Architecture
-   Configuration
-   Memory state
-   Network conditions
-   Payload compatibility

An exploit may fail or destabilize a target. Authorized scope and risk
awareness are essential.

------------------------------------------------------------------------

## Quick Workflow

``` text
START
  ↓
msfconsole
  ↓
search <keyword>
  ↓
Read module type, rank and description
  ↓
use <module>
  ↓
info
  ↓
show options
  ↓
Configure required parameters
  ↓
Verify defaults
  ↓
show payloads
  ↓
Understand payload compatibility
  ↓
check
(if supported and appropriate)
  ↓
run / exploit
  ↓
Result
  ↓
Session may open
  ↓
sessions
  ↓
sessions -i <id>
  ↓
Authorized post-exploitation validation
```

### Golden Rule

``` text
SEARCH
  ↓
SELECT
  ↓
UNDERSTAND
  ↓
CONFIGURE
  ↓
VERIFY
  ↓
RUN
  ↓
ANALYZE
  ↓
DOCUMENT
```

Do not become a `use → set RHOSTS → run` command machine. Understand the
module, target assumptions, payload, expected impact, and detection
artifacts.

------------------------------------------------------------------------

## Defender and SOC Perspective

Metasploit concepts are also valuable for defensive security.

Attack-chain view:

``` text
Exposed Service
      ↓
Vulnerability
      ↓
Exploit Attempt
      ↓
Payload Execution
      ↓
Outbound Connection
      ↓
Session
      ↓
Post-Exploitation Activity
```

A SOC analyst should ask:

-   Which service was exposed?
-   Was unusual scanning or enumeration observed?
-   What network activity occurred before exploitation?
-   Did a process create an unusual outbound connection?
-   Which source and destination ports were used?
-   Were suspicious child processes created?
-   Did interactive command activity follow?
-   What endpoint and network telemetry supports the timeline?

### Reverse Connection Mental Model

``` text
Target Host : Ephemeral Source Port
              ↓
       Outbound TCP
              ↓
Listener Address : Listener Port
```

For defenders, a successful reverse session should trigger investigation
into:

``` text
Process
  ↓
Socket / Connection
  ↓
Destination
  ↓
Child Process
  ↓
Commands
  ↓
Persistence or Further Activity
```

------------------------------------------------------------------------

## Final Revision Notes

``` text
Vulnerability = Weakness

Exploit = Code/technique that leverages the weakness

Payload = Code/action executed on the target

Auxiliary = Supporting scanning/enumeration modules

Single = Self-contained payload

Stager = Small component that establishes a channel

Stage = Larger component delivered after the stager

RHOSTS = Remote target(s)

RPORT = Remote service port

LHOST = Listener/callback address

LPORT = Listener port

SESSION = Existing Metasploit connection ID

set = Current context

setg = Global datastore

background = Keep session active but leave interaction

sessions -i <id> = Interact with a session
```

------------------------------------------------------------------------

## Disclaimer

These notes are intended for cybersecurity education, legal labs, CTFs,
and authorized penetration testing. Only test systems you own or have
explicit permission to assess.
