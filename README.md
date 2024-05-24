# Atomic Red Team and Bluespawn

## Introduction
An active defense lab requiring us to analyze the efficiency of our EDR system (Bluespawn) given an attack scenario (Atomic Red Team). Because Atomic Red Team maps to the MITRE ATTA&CK Framework, we have the ability to specify our test technique accordingly and be able to detect it on our EDR tool. We can then see what the attack would look like from a blue team perspective. This analysis will help us to understand where security gaps may be present, furthering our understanding of what types of mitigation are needed. Remember, this lab is not about Bluespawn or any EDR equivilent. This is about how we can use Atomic Red Team to test and see how our EDR reacts to post exploitation activities that an attacker may do in our environment. From an active perspective, we can run these attacks. Can we detect it?

## Environment and tools
The following environment is from Antisyphon Training's Active Defense and Cyber Deception course with John Strand
- Windows 10
- Atomic Red Team - a tool for security teams used to simulate various cyber attacks. Each test, or “atomic tests,” maps directly to the MITRE ATT&CK Framework. Atomic Red Team is useful for security teams to improve on defensive capabilities
- Bluespawn - an endpoint detection and response tool (EDR) developed to help blue teams detect, investigate, and respond to cyber threat. Not to be used for enterprise use, Bluespawn is a good stand-in for lab environments where commercial EDR’s are not feasible

## Walkthrough
1. Ensure that Defender is disabled for this session
```
Set-MpPreference -DisableRealtimeMonitoring $true
```
<br>
<br>

2. Open terminal as admin. (this will be used for Atomic Red Team)
<br>
<br>

3. Open command prompt as admin (this will be used for Bluespawn)
<br>
<br>

4. In command prompt, change directory to tools (or wherever Bluespawn is) and run Bluespawn
```
C:\Users\adhd>cd \tools
C:\tools>BLUESPAWN-client-x64.exe --monitor --level Cursory
```
<img src="https://github.com/trixiahorner/Bluespawn/blob/main/images/B1.png?raw=true" height="80%" width="80%" alt="bluespawn"/>
<br>
<br>

5. Now I will use Atomic Red Team to test Bluespawn and see if it picks up on anything. In the powershell window, I navigate to the atomic red team directory and install the proper yaml modes
```
C:\Users\adhd> cd C:\AtomicRedTeam\invoke-atomicredteam\
C:\Users\adhd> Install-Module -Name powershell-yaml
C:\AtomicRedTeam\invoke-atomicredteam> Import-Module .\Invoke-AtomicRedTeam.psm1
```
<br>
<br>

6. Invoke atomic tests
```
C:\AtomicRedTeam\invoke-atomicredteam> Invoke-AtomicTest All
```
I can also be technique specific. This will make it faster. Remember, these techniques map to MITRE ATT&CK.
```
C:\AtomicRedTeam\invoke-atomicredteam> Invoke-AtomicTest T1004
```
<img src="https://github.com/trixiahorner/Bluespawn/blob/main/images/B2.png?raw=true" height="80%" width="80%" alt="atomictest"/>
<br>
<br>

7. Switch to command terminal and you should see Bluespawn alerts
<img src="https://github.com/trixiahorner/Bluespawn/blob/main/images/B3.png?raw=true" height="80%" width="80%" alt="atomictest"/>
Bluespawan easily detected T1004. I tried running a few other tests: T1013 and T1015, and it was not detected. From an active blue team perspective,
this is a gap in our security support structure.
<br>
<br>

9. In powershell, I need to return the environment to its original state after the attack. I do this with the *-cleanup* command
```
C:\AtomicRedTeam\invoke-atomicredteam> Invoke-AtomicTest All -Cleanup
```
<img src="https://github.com/trixiahorner/Bluespawn/blob/main/images/B4.png?raw=true" height="80%" width="80%" alt="cleanup"/>

## Conclusion
Using Atomic Red Team to simulate various attack scenarios, I was able to observe Bluespawn's efficiency in detecting, investigating, and responding to threats. This lab demonstrated the importance of active defense mechanisms and identifying potential security gaps to enhance cybersecurity posture.
