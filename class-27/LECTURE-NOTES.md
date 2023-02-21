# Lecture Notes: Persistence

## Lecture 1: Atomic Red Team

**Why**
- Why do we need Atomic Red Team?
  - Testing coverage is fundamental to improving security outcomes
  - Testing should be fast and easy
  - Defenders need to keep learning how adversaries are operating
  - Without a testing method, admin will just download a VM and pull 100 viruses from VirusTotal to test their defenses. Not realistic.

**What**
- What is Atomic Red Team?
  - **Atomic Red Team** is "a library of simple tests that every security team can execute to test their defenses. Tests are focused, have few dependencies, and are defined in a structured format that can be used by automation frameworks."
- What is an Atomic Red Team test?
  - Every **Atomic Red Team test** is a small, highly portable detection test mapped to the MITRE ATT&CK Framework.
  - Mapped to a tactic
  - Facilitates defensive testing in a proactive, actionable fashion

**How**
Ref. [DEMO.md](DEMO.md)

## Lecture 2: PowerShell Empire

**Why**
- Why are we using PowerShell Empire?
  - Infamous for its use by nation-state APTs
  - Encrypted C2 traffic
  - Popular post-exploitation framework

**What** (Ref. [Raj Chandel - Hacking with Empire](https://www.hackingarticles.in/hacking-with-empire-powershell-post-exploitation-agent/), [OSCP - My Journey](https://anishmi123.gitbooks.io/oscp-my-journey/content/))
- Review: What is PowerShell Empire?
  - **PowerShell Empire** is a pure PowerShell post-exploitation agent built on cryptologically-secure communications and a flexible architecture.
  - Similar to Metasploit framework, Empire sought to draw attention to the potential for PowerShell to be weaponized
  - Its original mission has since been fulfilled and it is no longer maintained by its authors as of 2019. The PowerShell Empire project is now actively maintained by BC Security.
- What are the important components of Empire?
  - A **listener** is a process that runs on the C2 server and awaits connection requests from compromised hosts. This allows `STDOUT` data to route back to the C2 server's shell so the attacker can see what's happening.
  - An **agent** is a program that maintains a connection between C2 and the compromised host.
  - A **stager** is a snippet of code that allows malicious code to be run via the agent on the compromised host.
    - The **launcher stager** (./lib/stagers/launcher.py) is a commonly-used stager module, and generates a one-liner stage0 launcher for an Empire agent
    - The **launcher_bat stager** (./lib/stagers/launcher_bat.py) generates a self-deleting `.BAT` file that executes a one-liner stage0 launcher for an Empire agent.
    - The **macrostager** (./lib/stagers/macro.py) generates an office macro that launches an Empire stager. This macro can be embedded into any office document for the purposes of phishing.
    - The **dll stager** (./lib/stagers/dll.py) generates a reflectively-injectable MSF-compliant `.DLL` that loads up the .NET runtime into a process and execute a download-cradle to stage an Empire agent. These `.DLLs` are the key to running Empire in a process that’s not `powershell.exe`. Using these `.DLLs` with Metasploit is described here.
  - **Modules** execute malicious commands, which can harvest credentials and escalate privileges, etc. for the attacker

**How**
- Ref. [DEMO.md](DEMO.md)


# Lecture Notes: Python rotating log files

The python logging module gives us the `RotatingFileHandler` class and the `TimedRotatingFileHandler` class to split up the log files generated

**RotatingFileHandler**

`RotatingFileHandler` allows us to rotate logs into a new file every time the current log file reaches a certain size.

**Example**

This script will rotate logs into a new file when it reaches 500 bytes up to a max number of 2 backups

```python
import logging
import logging.handlers as handlers
import time

logger = logging.getLogger('my_app')
logger.setLevel(logging.INFO)

logHandler = handlers.RotatingFileHandler('app.log', maxBytes=500, backupCount=2)
logHandler.setLevel(logging.INFO)
logger.addHandler(logHandler)

def main():
    while True:
        time.sleep(1)
        logger.info("A Sample Log Statement")

main()
```

**TimedRotatingFileHandler**

`TimedRotatingFileHandler` allows us to capture log files by a time slice

```python
import logging
import logging.handlers as handlers
import time

logger = logging.getLogger('my_app')
logger.setLevel(logging.INFO)

logHandler = handlers.TimedRotatingFileHandler('timed_app.log', when='M', interval=1)
logHandler.setLevel(logging.INFO)
logger.addHandler(logHandler)

def main():
    while True:
        time.sleep(1)
        logger.info("A Sample Log Statement")

main()
```
