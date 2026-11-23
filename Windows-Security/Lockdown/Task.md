Lab 02: Lockdown — Multi-Stage Intrusion Investigation (DFIR)

1. Scenario

The SOC of a fictional company, TechNova Systems, detected anomalous outbound traffic from a public-facing, cloud-hosted IIS web server. The behavior suggests a web shell followed by command-and-control communication to an unknown external host.

You received three artifacts: a network capture (PCAP), a memory dump of the server and a malware sample recovered from disk. Determine the root cause, reconstruct the attacker's timeline and recommend containment.
	CyberDefenders, "Lockdown" challenge: https://cyberdefenders.org/blueteam-ctf-challenges/lockdown/

2. Rules of engagement
Work only inside your isolated analysis VM. Take a snapshot before you start.
Never execute the malware sample. This lab is static analysis only.
Keep the VM's network disabled (or host-only) while the artifacts are open.
Write down the SHA-256 of every artifact before you touch it.
Do not read public write-ups or walkthroughs before you finish. You learn nothing from copied answers, and in an interview they will ask how you found each thing.

3. Environment
VirtualBox analysis VM (Windows or a Linux distribution of your choice) with:
Wireshark
Volatility 3 and/or MemProcFS
FLOSS and strings
A browser for threat intelligence lookups (hash and IOC lookups only)
Artifacts downloaded from the challenge page (sign-in required)
A notes file where you log every command, timestamp and finding as you go

4. Stages

Each stage has tasks and guiding questions. The questions do not contain answers. Record the answer, the evidence and how you found it for each one.

Stage 0: Setup and evidence handling

Tasks:

Create the analysis VM and a clean snapshot.
Record file names, sizes and SHA-256 hashes of all artifacts in a table.
Start a timeline file (UTC timestamps, source, event, why it matters).

Questions:

Which artifact do you expect to give you the earliest events, and why?
Stage 1: Network forensics (PCAP)

Tasks:

Get an overview first: capture duration, protocol hierarchy, top talkers (Statistics menu in Wireshark).
Identify the server and the external hosts it communicates with.
Examine HTTP requests and responses. Look at methods, URIs, user agents and response codes.
Extract transferred files or payloads and hash them.
Analyze outbound traffic for regular timing patterns.

Questions:

Which external address interacted with the server first, and what did it do?
Which request looks like the initial exploitation or web shell interaction? What makes it stand out from normal traffic?
Was any file uploaded to the server? Under what name and path?
Is there evidence of beaconing? What is the interval, the destination and the protocol?
Stage 2: Memory forensics

Tasks:

Identify the operating system profile and image details.
List processes and build the process tree. Inspect parent–child relationships and command lines.
List network connections and map them to process IDs.
Look for code injected into process memory.
Cross-check what you saw in the PCAP: do the memory connections match?

Questions:

Which process is suspicious, and what makes its parentage or behavior abnormal for this server?
Which process owns the connection to the external host?
Is there memory-resident code that does not map to a legitimate file on disk?
What command lines were executed, and what do they tell you about attacker intent?
Stage 3: Malware triage (static only)

Tasks:

Hash the sample (MD5, SHA-1, SHA-256) and check the hash on a threat intelligence platform.
Review file type, compile timestamp and imports.
Extract strings with strings and FLOSS. Compare the results: what did FLOSS reveal that strings did not?
Collect indicators of compromise (IOCs): domains, IPs, file paths, registry keys, mutexes, user agents.

Questions:

What is the sample's family or classification according to threat intelligence, and how confident are you?
Which IOCs from the sample also appear in the PCAP or memory image?
What capabilities does the sample appear to have, based on strings and imports alone?
Stage 4: Correlation and timeline

Tasks:

Merge your findings into one timeline across the three sources.
Mark each event with its source (PCAP, memory or sample).
Identify the order of events: initial access, execution, persistence, discovery, lateral movement attempts, command and control.

Questions:

What is the root cause of the compromise?
What is the dwell time visible in your evidence?
Which gaps remain? What evidence would you want next (for example IIS logs, EDR telemetry, disk image)?
Stage 5: MITRE ATT&CK mapping

Fill in this table. Only include techniques you can support with evidence from your own analysis.

Tactic	Technique (name and ID)	Evidence (source, timestamp, screenshot)	Detection idea
Execution			
Persistence			
Privilege Escalation			
Stealth (Defense Evasion)			
Discovery			
Lateral Movement			
Command and Control			

The "Detection idea" column is the part employers care about: for each technique, write one sentence on how a SOC could have caught it (log source, field or pattern).

Stage 6: Incident report and recommendations

Write the report described in section 7.

6. Evidence checklist
[ ] Artifact hash table
[ ] Wireshark screenshots: protocol hierarchy, the suspicious HTTP request, the extracted payload, the beaconing pattern
[ ] Volatility or MemProcFS screenshots: process tree, network connections, injected memory finding
[ ] FLOSS and strings output excerpts with the IOCs highlighted
[ ] Threat intelligence lookup screenshot
[ ] Unified timeline
[ ] Completed ATT&CK table
7. Report requirements

Create Incident_Response_Report.md in the lab folder with these sections:

Executive summary (5 sentences maximum, non-technical): what happened, impact, current status.
Scope and evidence: artifacts analyzed and their hashes.
Timeline: table, UTC timestamps.
Technical findings: one subsection per stage with screenshots.
Indicators of compromise: table (type, value, where found).
MITRE ATT&CK mapping: the completed table.
Containment and recommendations: immediate actions (first 24 hours), short-term fixes and long-term improvements. Be specific to this incident.
Limitations: what you could not determine and why.

Write in your own words. Short, precise sentences beat long ones.

8. Repository structure
Lockdown-Lab/
├── README.md                      # short overview, links to the report
├── Incident_Response_Report.md    # the full report
├── img/                           # screenshots, referenced as img/name.png
├── iocs.md                        # indicators of compromise
└── notes/                         # optional: command log, timeline draft

Rules:

Use relative image paths such as ![Protocol hierarchy](img/protocol-hierarchy.png).
Do not commit the malware sample, the memory image or the PCAP. Link to the challenge page instead.
Remove anything that is not part of the training scenario (personal paths, usernames, tokens).
9. Completion criteria
[ ] Every stage has recorded answers with evidence
[ ] The timeline covers all three artifact sources
[ ] ATT&CK table includes a detection idea for every technique
[ ] The report is complete and written in your own words
[ ] The repository folder follows the structure above
[ ] You can explain the full attack chain aloud in two minutes
10. Hints

Hints are available on request, one stage at a time, in three levels: a nudge, a direction, a specific pointer. Ask for the level you need, and tell me which stage you are stuck on and what you have already tried.

11. Submission

Send me your draft Incident_Response_Report.md (paste it into the chat). I will review it for clarity, evidence quality and gaps, then help you polish it for the portfolio.