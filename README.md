# Social Engineering Toolkit (SET) – Authorized Lab

**Status:** Authorized educational exercise conducted in a fully isolated laboratory environment.

## Objective

Demonstrate how social-engineering attack vectors work in a controlled setting so that defensive controls and user-awareness measures can be better understood and improved.

## Lab Environment

- Two isolated virtual machines:
  - **Attacker machine** – runs the Social Engineering Toolkit (SET)
  - **Victim machine** – represents the target endpoint
- Full network isolation and VM snapshots used throughout
- All activity performed under explicit authorization

## High-Level Process

### 1. Attacker machine preparation

The Social Engineering Toolkit is launched on the designated attacker system. An attack vector is selected (commonly a website clone / credential-harvesting page or a payload-delivery option). The toolkit is configured so that successful interaction from the victim side produces a visible callback or session notification on the attacker console.

![SET interface](media/image1.png)

![SET menu / configuration](media/image2.jpeg)

![SET options](media/image3.jpeg)

![SET configuration screen](media/image4.png)

![Additional SET settings](media/image5.jpeg)

![Payload / attack vector setup](media/image6.jpeg)

![Listener or delivery configuration](media/image7.png)

![Status / progress indicator](media/image8.jpeg)

![Configuration summary](media/image9.jpeg)

### 2. Switch to the victim machine

Control is moved to a second virtual machine that represents the target endpoint. This machine is used only to simulate the actions a real user might take (for example, visiting a crafted link or interacting with delivered content).

![Switching to victim machine](media/image11.png)

*Representing the victim machine.*

![Victim machine view](media/image10.jpeg)

### 3. Simulated victim interaction

On the victim machine the operator performs the actions that would trigger the chosen social-engineering vector. Because the lab is fully isolated, this interaction stays inside the controlled environment.

### 4. Attacker-side confirmation

The attacker console receives a clear indication that the payload or session is active. This confirms that the delivery path worked as expected inside the lab and allows the operator to observe the resulting session or harvested data (again, only within the isolated environment).

**The attacker will know when the payload is running.**

![Payload active / session notification](media/image12.png)

![Session details](media/image13.png)

![Additional confirmation / results](media/image14.png)

![Final session / harvested data view](media/image15.png)

### 5. Documentation and cleanup

Screenshots of the SET interface, the victim-side interaction, and the successful callback are captured for the lab report. Virtual machines are then reverted to clean snapshots, ensuring no residual artifacts remain.

## Key Observations

- The attacker can immediately see when the simulated payload becomes active.
- Social-engineering success depends heavily on user interaction rather than pure technical exploitation.
- The same techniques that succeed in a lab can be detected and mitigated in real environments through layered defenses.

## Defensive Takeaways

- User-awareness training remains one of the most effective controls.
- Technical mitigations such as multi-factor authentication, email/web filtering, application allow-listing, and endpoint detection can interrupt many of these vectors.
- Network segmentation and least-privilege principles limit the impact of any successful social-engineering attempt.
- Regular authorized testing in controlled labs helps organizations identify gaps before real adversaries do.

## Disclaimer

All activities described in this repository were performed in an isolated laboratory environment under explicit authorization.  

The techniques demonstrated must **never** be used against systems or individuals without prior written permission.  

This write-up is intended solely for educational and defensive purposes.
