# Splunk SPL Queries — Quick Reference
 
> 5 essential SPL queries for SOC / Blue Team analysts. Simple, memorable, ready to use.
 
---
 
## 1. Get ALL events from main index
 
```spl
index=main
```
 
**Remember:** "Main index = main storage. No filter = everything."
Default time range is last 24 hours unless you change it manually.
 
---
 
## 2. Filter by sourcetype = syslog
 
```spl
index=main sourcetype=syslog
```
 
**Remember:** "Syslog = Linux/Unix system logs + network device logs."
Use this to narrow down noise and focus only on OS-level events.
 
---
 
## 3. Count events by Host
 
```spl
index=main
| stats count by host
| sort - count
```
 
**Remember:** "Who is talking the most?"
High event count from one host = possible misconfiguration or active threat.
 
---
 
## 4. Top 10 Source IPs
 
```spl
index=main
| top limit=10 src_ip
| table src_ip count percent
```
 
**Remember:** "Who is knocking the door the most?"
Useful for detecting reconnaissance, brute force, or DDoS source IPs.
 
---
 
## 5. 'Error' events in the Last 24 Hours
 
```spl
index=main earliest=-24h latest=now
| search error
| table _time host sourcetype _raw
| sort - _time
```
 
**Remember:** "Recent errors, newest first."
Go-to query for quick triage during an incident. Shows raw log + time + source.
 
---
 
## Cheat Sheet
 
| # | What you want | Key command |
|---|--------------|-------------|
| 1 | All events | `index=main` |
| 2 | Only syslog | `sourcetype=syslog` |
| 3 | Events per host | `stats count by host` |
| 4 | Top talkers (IPs) | `top limit=10 src_ip` |
| 5 | Recent errors | `earliest=-24h \| search error` |
 
---
 
*Reference sheet for Splunk SPL — SOC Analyst use*
 