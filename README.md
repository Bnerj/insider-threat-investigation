# insider-threat-investigation
Hands-on cybersecurity lab investigating suspicious email activity using Wireshark, SMTP packet analysis, DHCP logs, and system security logs to identify the source IP, host device, and associated user account.

# Unit 1 Lab Report: It Wasn't Me – Insider Threat Investigation

## Lab Summary

The purpose of this lab was to investigate suspicious network activity within a simulated corporate environment and identify the employee responsible for sending unauthorized emails throughout the company. Acting as a Blue Team analyst within a Security Operations Center (SOC), I used network traffic analysis, DHCP logs, and system security logs to trace the suspicious activity back to a specific host device and user account.

The investigation demonstrated how multiple sources of digital evidence can be correlated to identify the origin of suspicious network activity and determine which user was logged into the device when the incident occurred.

## Tools & Technologies Used

- **Wireshark:** Network traffic analysis and packet inspection.
- **PCAP Files:** Captured network traffic used to investigate suspicious email activity.
- **DHCP Logs:** Used to associate an IP address with a specific host device.
- **System Security Logs:** Used to examine user logon and logoff sessions.
- **SMTP:** Protocol analyzed to identify email communications.
- **Environment:** CodePath Cybersecurity Sandbox.

## Lab Procedures

### Step 1: Opening the SMTP.pcap File

I opened the provided `SMTP.pcap` file in Wireshark to examine the captured network traffic and confirm that the file contained 60 packets.

<img width="1470" height="918" alt="S1_PACKETS" src="https://github.com/user-attachments/assets/664a2d90-c8eb-49c4-8f64-9286e1a74ddf" />


### Step 2: Initial Packet Filtering

I applied the `smtp` display filter in Wireshark to isolate Simple Mail Transfer Protocol (SMTP) traffic. This eliminated unrelated network traffic and narrowed the investigation to email-related communications.

<img width="1470" height="876" alt="S2_SMTP" src="https://github.com/user-attachments/assets/732a4829-a113-4f9f-aacf-2c388847ab76" />


### Step 3: Target Packet Isolation and IP Extraction

I refined the display filter using `smtp contains "FROM"` to locate the packet containing the sender information. By examining the packet details and Info column, I identified the source IP address associated with the suspicious network activity.


<img width="1470" height="953" alt="S3_SMTP_FROM" src="https://github.com/user-attachments/assets/050dd0c4-f3ee-4ff6-a1fe-31a57729a50f" />


**Finding:** `10.10.1.4`

This IP address provided the starting point for tracing the suspicious activity back to a specific computer within the network.

### Step 4: Network-to-Host Correlation (DHCP Logs)

After identifying the source IP address, I examined the DHCP log file and reviewed events around the incident timestamp. I used the source IP address identified in Wireshark to determine which host device was assigned that address.

I reviewed DHCP events occurring before the incident timestamp of April 19, 2023, at 12:50 PM. Specifically, I focused on the six events occurring around 12:11 PM and located the event recorded at 12:11:27 PM.

<img width="1470" height="329" alt="DHCP_LOG" src="https://github.com/user-attachments/assets/c4eaa21b-183a-48f9-9c6c-37ed04eae3eb" />

By examining the DHCP address assignment, I determined that the suspicious IP address `10.10.1.4` was assigned to the host computer `USER2`.

**Finding:** `USER2`

This allowed me to narrow the investigation from a network IP address to a specific endpoint within the company's environment.

### Step 5: Host-to-User Attribution (Security Logs)

After identifying `USER2` as the host device associated with the suspicious activity, I opened the provided `Security_log.rtf` file to examine user authentication events.

<img width="1470" height="860" alt="SECURITY_LOG" src="https://github.com/user-attachments/assets/392dd806-5c3e-4f91-b9c7-2534cd3bfd3d" />

The security log contained four events documenting logon and logoff activity across two host computers. The first two events represented a login session on `USER1`, associated with Jane Doe. The last two events represented a login session on `USER2`, associated with John Doe.

By comparing the login session timestamps with the incident time of 12:50 PM, I determined that John Doe was actively logged into `USER2` when the suspicious emails were sent.

**Finding:** John Doe was the user logged into the identified host device during the incident.

## Investigation Findings

The investigation successfully connected the suspicious network activity to a specific host device and user account.

| Investigation Detail | Finding |
|---|---|
| Incident Date | April 19, 2023 |
| Incident Time | 12:50 PM |
| Source IP Address | `10.10.1.4` |
| Identified Host Device | `USER2` |
| User Account | John Doe |
| PCAP File | `SMTP.pcap` |
| DHCP Log | `DHCP.txt` |
| Security Log | `Security_log.rtf` |

## Key Takeaways & Skills Developed

Throughout this lab, I strengthened my understanding of network forensics, endpoint identification, and security log analysis.

Key skills developed included:

- Applying Wireshark display filters to isolate SMTP traffic.
- Identifying source IP addresses within packet captures.
- Understanding how DHCP assigns IP addresses to network devices.
- Analyzing DHCP logs to correlate IP addresses with hostnames.
- Reviewing system security logs to identify user logon and logoff sessions.
- Correlating timestamps across multiple sources of digital evidence.
- Tracing suspicious network activity back to a host device and associated user account.
- Understanding how SOC analysts investigate potential insider threats and account impersonation.

## Conclusion

This lab demonstrated the importance of correlating multiple sources of digital evidence during a cybersecurity investigation. By analyzing SMTP traffic in Wireshark, I identified the suspicious source IP address (`10.10.1.4`). I then examined the DHCP logs and determined that the IP address was assigned to the host computer `USER2`. Finally, I reviewed the system security logs and correlated the active login session with the incident timestamp to identify John Doe as the user logged into the device when the suspicious emails were sent.

Overall, this lab strengthened my practical understanding of network traffic analysis, DHCP log correlation, and user attribution. It also reinforced an important cybersecurity principle: identifying the source of suspicious network activity requires more than an IP address. Connecting network traffic, device assignments, and user authentication records provides a more complete picture of a security incident.

---

**CodePath Cybersecurity Bootcamp | Unit 1 – It Wasn't Me: Insider Threat Investigation | Personal Technical Documentation**
