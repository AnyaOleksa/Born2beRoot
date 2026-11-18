*This project has been created as part of the 42 curriculum by asparyns*

```
    ____                    ___   __       ____              __ 
   / __ )____  _________   |__ \ / /_     / __ \____  ____  / /_
  / __  / __ \/ ___/ __ \  __/ // __ \   / /_/ / __ \/ __ \/ __/
 / /_/ / /_/ / /  / / / / / __// /_/ /  / _, _/ /_/ / /_/ / /_  
/_____/\____/_/  /_/ /_/ /____/_.___/  /_/ |_|\____/\____/\__/
```
# Description
The 42 Born2BeRoot project consists of setting up a server by following specific rules.

Based on the instructions I approached this project through 4 distinct phases:

- Operating System Selection — Understanding the difference between the base platform and picking one most suited to the project.
- Network Hardening & Firewalls
- Secure Remote Access (SSH) 
- Access Control & Privilege Management
    
## Key learning outcomes
- Network Mechanics: Understanding how packets route across interfaces and how SSH protocol keys eliminate network sniffing/clear-text traffic manipulation.
- Infrastructure Layout: Defining what a server role actually implies, how a hypervisor abstracts bare-metal resources, and why we partition hard drives dynamically using Logical Volume Management (LVM).
- Data Integrity Protocols: Verifying system deployments using digital disk signatures (sha256sum) and leveraging VirtualBox snapshot states to safeguard active server iterations.

## Summary of new concepts learned

The primary focus of this exercise was to master a variety of system administration and security principles. Below is a list of the technical concepts the project allowed me to master:

1) Networking & Virtualization
 - Ports: Numerical network channels acting as targeted endpoints for incoming and outgoing communication threads.
 - Network: Any physical or logical medium connecting two or more devices to safely share data payloads.
 - Server Architecture:
    - The Host: The real, physical hardware running your primary operating system (e.g., your laptop) that you can physically touch
    - The VM (Virtual Machine): A simulated guest environment isolated from the host, running its own dedicated OS kernel and virtual storage.
    - The "Server": A role assigned to a machine. A server is created the exact moment a device is configured to listen continuously for inbound network requests and serve resources.
- Loopback IP Address (127.0.0.1): A universally hardcoded local loop line mapping a device back to itself. Network requests sent to this address loop directly into the local loopback processing interface without ever leaving the hardware stack.
- Hypervisors: Emulation layers that slice up physical hardware resources into multiple, isolated virtual environments.
    - Type 1 (Bare Metal): Installed directly onto the physical server hardware. Offers high optimization, security, and enterprise throughput.
    - Type 2 (Hosted): Runs as an application inside an existing Host OS. Perfect for developer sandbox configurations and local isolation tasks. This project utilizes a Type 2 hypervisor (Oracle VirtualBox).
- Virtual Disk Frameworks: Understanding file extensions like .vdi (VirtualBox native) and .vmdk (VMware compatible formats)    
- Protocols & Standards:
    - TCP (Transmission Control Protocol): A connection-oriented set of network rules ensuring reliable, ordered, and error-checked data delivery across a network fabric.
    - HSTS (HTTP Strict Transport Security): A web security policy header forcing browsers to interact with web servers using secure HTTPS connections exclusively.

2) Remote Access (SSH)
- SSH (Secure Shell): A cryptographic network protocol allowing secure remote access over unsecure networks, replacing text-clear management utilities like Telnet.
- SSHD Configuration Hardening:
    - Modifying config rules to state PermitRootLogin no blocks attackers from instantly knowing half of the puzzle (the username root) when launching automation scripts or brute-force tracking programs.
    - sshd (The Daemon): Acts as the receiver listening on the target machine, dictating access rules, ports, and allowed identities
    - ssh (The Client): Acts as the outbound interface tool executed to connect to remote targets.

Note: The sshd process does not strictly require an external text configuration file to load; its global fallback parameters are compiled directly into its machine binary. The system loads text files like /etc/ssh/sshd_config to explicitly override these hardcoded baselines.

3) System Internals & Scripts 
- Daemons: Background system services that run continuously without direct user intervention, waiting to capture or process specific jobs (e.g., sshd, cron).
- Cron: An automated background job scheduler utilizing granular chronographical expressions (crontab -e) to execute repetitive maintenance tracks.
- monitoring.sh: A custom automation script querying and logging live server telemetry data. __Note__: Execution outputs rely directly on the environment of the calling entity. A user shell loads a robust path dictionary, whereas the detached cron environment executes with a restricted search $PATH, meaning binary declarations often require absolute file paths to execute.
- CLI (Command Line Interface): Text-driven interaction shells used to manipulate operating system processes directly without a graphical interface (GUI).
- System Logging: Utilizing centralized engines to audit active behaviors. In modern minimal Debian deployments, tracking logs reside directly inside a structured systemd binary database managed via journalctl.
- PAM (Pluggable Authentication Modules): PAM is a centralized system guard that handles login security for all applications, allowing us to easily enforce strict password complexity rules across the entire OS.

4) Compute & Storage Hardware
- CPU Layout Metrics
    - Physical Processor (Sockets): The actual physical CPU hardware chips plugged directly into the motherboard slots.
    - Physical Core: Independent, individual hardware execution units housed within a single physical processor chip.
    - Virtual Processor (vCPU): The total logical threads presented directly to the OS kernel layout (calculated as: Sockets * Cores * Threads per Core).
- Storage Abstraction 
    - Disk Filesystem (df): A systematic structural methodology mapping how files are indexed and dynamically tracked across a physical volume block.
    - sda: Typically designates the primary physical storage block identified by the kernel framework.
    - LVM (Logical Volume Management): An advanced virtualization architecture abstracting raw storage lines into dynamically resizable logical drives, decoupling the filesystem from underlying rigid disk geometry.
    - Swap Space: A fallback area on a hard drive partition allocated to absorb data overflow when standard physical RAM capacity becomes completely saturated.
    - Crypt Level Layer: Utilizing encryption arrays to scramble disk volumes at rest.

5) File system Directory Anatomy 
- `/bin`: Essential user command binaries required in single-user mode (e.g., cat, ls).
- `/sbin`: System management binaries reserved for administrative execution tasks (e.g., fdisk, reboot).
- `/dev`: Virtual device interface nodes mapping raw hardware hardware endpoints into files (e.g., /dev/sda1).
- `/etc`: Centralized repository housing system-wide plain text configuration maps
- `/etc/shadow`: Highly secured system database storing user account password hashes alongside access expiration limits.
- `/etc/sudoers.d`: A configuration directory allowing modular drop-in administrative privilege tracking rules without modifying the main `/etc/sudoers` master template file.

## Defense in Depth Security Framework
Given my interest in Cybersecurity, I structured my server implementation around 5 distinct layers of a Defense-in-Depth architecture, creating overlapping rings of security control:

1. Principle of Least Privilege (PoLP) 
- Authentication & Access Boundaries: Users and internal processes must only operate with the absolute minimum privilege boundary needed to achieve their goals.
- Implementation: Setting up custom restricted sudo configuration groups, enforcing rigid account password lifetime schedules, and logging access behaviors.
- Root Mitigation: Minimizing exposure of the root account limits potential data extraction of PII (Personally Identifiable Information), cryptographic key maps, hashed passwords, or active session keys.

2. Segregation (Blast Containment)
- Storage Isolation: Using distinct LVM partitions limits cascading filesystem failures. If a log directory or user space fills up completely, core system sectors (/ or /boot) remain isolated and functional.

3. Auditing and Visibility (Monitoring Layer)
- Real-time Telemetry: Running custom scripts via automation ensures tracking metrics like CPU load changes, unexpected RAM usage, or unauthorized administrative elevation commands are compiled and visible across active terminal nodes immediately.

4. Cryptographic Hardening (Identity & Integrity Layer)
- Data at Rest & Transit: Enforcing strict full-disk volume encryption frameworks alongside cryptographic host authentication standards to establish authentic identities.

5. Network Hardening (Firewall Layer)
- Attack Surface Reduction: Minimizing open communication channels reduces opportunities for external interaction. Operating systems often leave various open listening points by default. Hardening involves enforcing an absolute default deny incoming policy, leaving only an obfuscated custom port (4242) active.
- MitM Protection: Forcing encrypted connections through SSH stops Man-in-the-Middle (MitM) credential sniffing, session hijacking, or malicious data injection payloads.

# Instructions

As per instructions, we only submitted the expected README.md file, and a signature.txt file at the root of out Git repository. We pasted the signature of our machine’s virtual disk into this file. To retrieve the signature from the ".vdi" file of the virtual machine in sha1 format when the evaluation starts we use the following.
file:

Please note that the virtual machine’s signature will be altered as soon as we start the virtual machine. To allow signature verification for all evaluations, __we will use a snapshots for each evaluation__.

Command to generate the signature: 

__on Linux__
 `sha1sum your_vm_name.vdi`

# Resources

- Google search and Gemini was mostly used for theoritical concepts. 
- This guide was used when my attempts at commands wouldn't work but always complemented by research to the meaning of each command: https://noreply.gitbook.io/born2beroot 
- About Virtual Box: https://fr.wikipedia.org/wiki/Oracle_VM_VirtualBox
- About Debian: https://fr.wikipedia.org/wiki/Debian

# Technical choices 

__OS Platform: Debian vs. Rocky Linux__

*Selection*: Debian

*Justification*: Enterprise OS selection balances stability, support footprints, and package supply lines. __Debian__ focuses on long-term software predictability, featuring community-driven package architectures (apt) that verify digital signatures before execution. Software packages inside stable Debian channels focus on structural reliability over cutting-edge feature additions, offering an environment for predictable long-term uptimes. __Rocky Linux__ targets commercial downstream data center deployments, natively leveraging Red Hat architectures (dnf, rpm) and enforcing complex mandatory access security policies (SELinux) tailored for cloud-scale configurations.

__Mandatory Access Control: AppArmor vs. SELinux__

*Selection*: AppArmor

*Justification*: __AppArmor__ Application Armor is a Linux security module that provides Mandatory Access Control (MAC) security.It restricts programs' capabilities and access to system resources. It uses security profiles to define what resources applications can access. Controls access based on file paths. Applications can only access explicitly allowed resources. It isolates processes by tracking paths and loading targeted profiles written in plain text, making it easy to use. __SELinux__ structures rules using deep security context labels: every single file, folder, network port and running program on the system is stamped with an invisible security label - for a program to talk to a file their labels must match according to the "masterpolicy rulebook". Highly secure, more dynamic but harder to learn.

__Firewall Frameworks: UFW vs. Firewalld__

*Selection*: UFW (Uncomplicated Firewall)

*Justification*: __UFW__ acts as a static, rule-based configuration manager with direct syntax parsing rules. It targets fixed ports and IP constraints, updating simple rulesets applied globally across the networking layer. As it name implies, it is much easier to use. __Firewalld__ functions dynamically using logical zones (trust layers tied directly to network interfaces), making it highly effective for multi-homed routers or complex corporate network layouts but much harder to set up.

__VirtualBox vs UTM__ 

*Selection*: VirtualBox

*Justification*: Beyond the cluster software availability, __VirtualBox__ is a Type 2 hypervisor designed natively for x86_64 architectures (Intel/AMD processors like our campuses machines are). VirtualBox can therefore pass instructions directly to the CPU. __UTM/QEMU__ was designed primarily for macOS. Additionally VirtualBox virtualises, expecting the VM OS to match the machine's hardware type. When UTM has to run on an OS fo a different architecture it has to use QEMU in the background to translate every single line of ode from x86 to ARM (causing massive performance slowdown). 

# Use of AI

Gemini was utilized during this project as an interactive educational assistant to deconstruct unfamiliar system theories and explain technical concepts at an accessible level. Gemini's assistant configuration was explicitly restricted to guiding theoretical understanding and clarifying structural concepts rather than generating operational script logic or system configurations. This README's content was authored by the student, with AI exclusively used as an editor to optimize formatting, alignment, and markdown layout readability.
