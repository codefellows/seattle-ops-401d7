# Lecture Notes: Remote Code Execution

## Threat Modeling and Analysis

**Why**
- Why should we study threat modeling and analysis?
  - **Cyber threat analysts** are information security professionals who leverage skills and expertise of network engineering to mitigate and avoid cyberattacks on the organization or its employees. Threat analysts identify vulnerabilities, study digital forensics, and conduct threat modeling in order to monitor and defend against threats faced by organizational infrastructure.

**What**
- What is threat analysis?
  - Cyber **threat analysis** is a process used by cybersecurity threat analysts to study and align a particular organization's defenses against its potential or realized cyber threats
  - According to software engineer Goran Aviani, the goal of a threat analysis is to answer the four questions of:
    - What are we working on?
    - What are the things that can go wrong?
    - How do we go about problems that occur?
    - Did we do a good job?

- What is threat modeling?
  - Cyber **threat modeling** is the structured process used to determine potential security threats and weaknesses, assess risk levels per threat and implement mitigations against specific threats.

## Lecture: Remote Code Execution

**Why**
- Why should we prepare to defend against remote code execution?
  - Discuss [MS15-011: Vulnerability in Group Policy could allow remote code execution: February 10, 2015](https://support.microsoft.com/en-us/topic/ms15-011-vulnerability-in-group-policy-could-allow-remote-code-execution-february-10-2015-91b4bda2-945d-455b-ebbb-01d1ec191328)

- Why is PowerShell important in cybersecurity?
  - "PowerShell represents one of the most interesting and powerful languages for a pentesting purpose."
  - "...for us, as pentesters, PowerShell represent a powerful shell and scripting language which is present (in most cases from windows 7, it’s integrated by default) on our pentest targets and provide to us specially a powerful post-exploitation “tool/language” that can give us so much power and a very big attack surface/possibility." -[InfosecInstitute](https://resources.infosecinstitute.com/topic/powershell-for-pentesters-part-1-introduction-to-powershell-and-cmdlets/)

**What**
- What is remote code execution?
  - **Remote code execution (RCE)** is a class of security vulnerabilities that allows a malicious actor to execute any code of their choice on a remote machine over LAN, WAN, or the Internet

- What is a post-exploitation framework?
  - **Post-exploitation** includes the phases of operation once a victim's system has been compromised by the attacker.
  - Examples of post-exploitation frameworks include Empire and Metasploit.

- What is PowerShell Empire?
  - **PowerShell Empire** is a pure PowerShell post-exploitation agent built on cryptologically-secure communications and a flexible architecture.
  - Empire implements the ability to run PowerShell agents without needing powershell.exe
  - Includes rapidly deployable post-exploitation modules ranging from key loggers to Mimikatz
  - Supports adaptable communications to evade network detection
  - Presented in an accessible framework (unified)
  - **Basic Terms**
    - Listener is a local process that listens for a connection from the attacked remote host
    - Stages is a piece of code that uploads the Agent to the attacked remote host
    - Agent is a process running on the attacked remote host that connects to your Listener
    - Module is the code executed by the Agent to achieve certain goals

- What is Invoke-PsExec?
  - **Invoke-PsExec** is a cmdlet that lets you execute PowerShell and batch/cmd.exe code asynchronously on target Windows computers, using PsExec.exe.
  - PsExec can be downloaded from the SysInternals suite on Microsoft's site
  - Remember, a batch file ends in .bat and contains non-PowerShell Windows commands only in the form of a script.

**How**
- How can we defend against RCE threats?

Ref. [DEMO.md](DEMO.md)

# Lecture Notes: Python Logging

Python's built-in logging module allows writing status messages to a file or any other output streams. The file can contain the information on which part of the code is executed and what problems have occurred

**Levels of Log Message**

There are give built-in levels of the log message

- **Debug**: used to give detailed information, usually useful when diagnosing problems
- **Info**: used to confirm that things are working as expected
- **Warning**: used as an indication that something unexpected happened, or is a sign of some problem in the near future
- **Error**: sign of a serious problem
- **Critical**: sign that the program is unable to continue running, a serious error

**Logger objects**

Several logger objects offered by the module

- Logger.info(msg) : This will log a message with level INFO on this logger.
- Logger.warning(msg) : This will log a message with a level WARNING on this logger.
- Logger.error(msg) : This will log a message with level ERROR on this logger
- Logger.critical(msg) : This will log a message with level CRITICAL on this logger.
- Logger.log(lvl,msg) : This will Logs a message with integer level lvl on this logger.
- Logger.exception(msg) : This will log a message with level ERROR on this logger.
- Logger.setLevel(lvl) : This function sets the threshold of this logger to lvl. This means that all the messages below this level will be ignored.
- Logger.addFilter(filt) : This adds a specific filter filt into this logger.
- Logger.removeFilter(filt) : This removes a specific filter filt into this logger.
- Logger.filter(record) : This method applies the logger’s filter to the record provided and returns True if the record is to be processed. Else, it will return False.
- Logger.addHandler(hdlr) : This adds a specific handler hdlr to this logger.
- Logger.removeHandler(hdlr) : This removes a specific handler hdlr into this logger.
- Logger.hasHandlers() : This checks if the logger has any handler configured or not.


**Example**

This code will generate a file with the provided name and will write logging messages to the file

```python
# importing module
import logging

# Create and configure logger
logging.basicConfig(filename="newfile.log",
					format='%(asctime)s %(message)s',
					filemode='w')

# Creating an object
logger = logging.getLogger()

# Setting the threshold of logger to DEBUG
logger.setLevel(logging.DEBUG)

# Test messages
logger.debug("Harmless debug Message")
logger.info("Just an information")
logger.warning("Its a Warning")
logger.error("Did you try to divide by zero")
logger.critical("Internet is down")
```
