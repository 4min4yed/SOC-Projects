# Phishing Detection and Response Simulation

## Introduction
This report documents a hands-on SOC simulation: Phishing Attacks in a life-like environment, with realistic noise, false positives and various real time incoming alerts.

**What I Learnt:** Detecting, Analyzing, and Responding to phishing attacks.  
It walks through the end-to-end process of investigating suspicious emails, classifying alerts as true or false positives, and applying appropriate remediation steps.

**Tools:** Splunk SIEM, IP reputation scanners, malicious link/attachment detectors.

---

## Process
I had multiple alerts on hand.

### First, Triage Process
- Prioritizing alerts affecting sensitive areas 
- Grouping alerts with similar aspects (same source, same link…)

After determining the alert to work on first, begins the analysis:

![Assigned Alert](Images/assignd%20alert.png)

Let’s take this first alert as an example; Understanding the different parts of the alert helps identify the threat if there is any:

- **Timestamps** help link incidents with logs (e.g. if a phishing email was sent at a time and a firewall log to a malicious site was timestamped moments after the alert’s, maybe the user has clicked the malicious link)
- **Subject and Content** give a brief explanation of what happened and how it was detected
- **Sender and Recipient** show affected user/machine

So I started with analyzing the attachment in the mail using URL Scanning tools like: URLscan.io, VirusTotal, PhishTank, CheckPhish, or the one in this simulation: **TryDetectThis**:

![Analyze URL](Images/analyze%20URL.png)

It says it is clean, but that’s not enough, further investigation should be conducted:

- Checked Splunk logs and found reoccurring interactions with the domain
- Checked if it is a shortened URL (bit.ly, short.url …)
- Looked for intentionally misspelled words similar to actual sites (micr0soft, goog1e…)
- Verified if it is a newly hosted server (<1yr)
- Assessed for suspicious requests (pay now, …)
- Tested it in a sandboxed environment

After being reassured it was safe, I closed the alert as a **False Positive**:

![False Positive](Images/false%20positive.png)

and wrote a report to justify my judgement:

![False Positive Report](Images/report%20of%20false%20positive.png)

---

## Investigating a True Positive

Now another alert caught my attention:

![Phishing Microsoft](Images/phishing%20m1crosoft.png)

The content included an obvious phishing link with a misspelled Microsoft (`m1crosoft`) so I scanned it just for evidence; and indeed, it was **malicious**:

![Analyze Phishing URL](Images/analyze%20phishing%20URL.png)

Next, I checked Splunk logs, and applied filters to see if the hazard had spread:

- **Firewall logs:** check if the user has clicked the malicious link or not, and if clicked check affected machines
- **Other emails:** check if others have received the same link and if they clicked

![Splunk Investigation](Images/went%20on%20splunk%20to%20see%20if%20the%20same%20ip%20sent%20other%20malicious%20links%20to%20other%20recipients%20in%20the%20network.png)

Thankfully in this case it was fine (employees were well trained).

Finally, I went ahead and reported the incident as a **True Positive** and escalated the alert following the **3Ws principle (Who, When, What)** and finally suggested remediation like:

- Credential reset
- Blacklisting sender’s IP
- Making all employees aware of the incident to raise awareness

![True Positive Report](Images/TruePositive%20Report.png)

---

## Conclusion
This SOC operations simulation was highly realistic and gave me a factual glimpse of how SOC teams deal with everyday alerts, especially how to handle phishing attacks in a professional real-world environment.
