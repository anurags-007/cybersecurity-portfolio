# Wireshark Display Filters Cheat Sheet

## 1. HTTP Requests

```
http.request
```

Shows only client → server HTTP requests (GET, POST).

---

## 2. DNS Traffic

```
dns
```

Displays all DNS queries and responses (domain resolution).

---

## 3. TCP SYN Packets (New Connections)

```
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Identifies new TCP connection attempts.

---

## 4. Traffic for a Specific IP

```
ip.addr == 192.168.1.10
```

Shows all traffic where this IP is either source or destination.

---

## 5. Destination IP Filter

```
ip.dst == 8.8.8.8
```

Shows traffic going to a specific destination IP.

---

## 6. Source IP Filter

```
ip.src == 192.168.1.10
```

Shows traffic originating from a specific IP.

---

## 7. Search for Sensitive Data

```
frame contains "password"
```

Finds packets containing specific keywords (e.g., credentials in plain text).

---

## 8. HTTP POST Requests

```
http.request.method == "POST"
```

Shows data submissions (logins, form data, possible exfiltration).

---

## 9. DNS Queries Only

```
dns.flags.response == 0
```

Displays only DNS requests sent by clients.

---

## 10. TCP Retransmissions

```
tcp.analysis.retransmission
```

Detects retransmitted packets (network issues or suspicious behavior).

---

## Bonus Filters

### Filter by Domain

```
http.host contains "example.com"
```

Shows traffic for a specific domain.

### Suspicious User-Agent

```
http.user_agent contains "curl"
```

Helps identify scripted or automated traffic.

---

# Notes

* GET → retrieves data
* POST → sends data (important in attacks)
* DNS → resolves domain names to IPs
* Combine filters for deeper investigation

---
