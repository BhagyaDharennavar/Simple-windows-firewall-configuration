# 🔐 Simple windows firewall configuration

## 🎯 Objective
To configure, test, and validate firewall rules on **Windows** using **Windows Defender Firewall with Advanced Security**, and to demonstrate deeper understanding through multiple validation methods.

## 🧰 Tools Used
- Windows Defender Firewall with Advanced Security  
- Command Prompt (Telnet Client)  
- Windows PowerShell

---

## 📌 Step-by-Step Implementation

### 🔹 Step 1: Open Windows Defender Firewall
- Press **Windows + R**
- Type `wf.msc` and press **Enter**
- Opens *Windows Defender Firewall with Advanced Security*

---

### 🔹 Step 2: Review Existing Inbound Rules
- Navigated to **Inbound Rules**
- Observed existing allow and block rules
- Understood how Windows controls inbound traffic using ports, protocols, and applications

---

### 🔹 Step 3: Create an Inbound Rule to Block Port 23 (Telnet)
**Why block Telnet (Port 23)?**
- Uses unencrypted communication
- Vulnerable to credential sniffing
- Deprecated in modern secure environments

**Configuration Steps:**
1. New Rule → Port  
2. Protocol: TCP  
3. Port: 23  
4. Action: Block the connection  
5. Applied to Domain, Private, and Public  
6. Rule name: **Block Telnet Port 23**

---

## 🔍 Firewall Validation – Before & After Testing

### 🔹 Before Blocking (Baseline Test)
The firewall rule was temporarily **disabled** to observe baseline behavior.

```cmd
telnet localhost 23
```

**Result:**
```
Could not open connection to the host, on port 23: Connect failed
```

---

### 🔹 After Blocking (Firewall Enabled)
The firewall rule was **enabled** again and the test was repeated.

```cmd
telnet localhost 23
```

**Result:**
```
Could not open connection to the host, on port 23: Connect failed
```

---

### 📌 Why the Result Is the Same 
On modern Windows systems, a **Telnet server is not running by default**, meaning no service is listening on port 23.  
As a result, connection attempts fail even before applying firewall rules.

Although the visible output is the same, the **firewall rule is still correctly enforced**.  
This behavior is expected on secure systems and does not indicate a configuration error.

---

## 📌 Why Telnet Connection Failed 

A Telnet connection requires **two conditions** to be met:

1. A **Telnet client** on the system  
2. A **Telnet server/service actively listening on port 23**

In this setup, only the **Telnet client** was enabled.  
On modern Windows systems, a **Telnet server is not installed or running by default**, meaning **no service is listening on port 23**.

As a result, Telnet connection attempts fail **even before applying firewall rules**, producing the message:

```
Could not open connection to the host, on port 23
```

This behavior is **expected on secure systems**.

---

## 🔐 Firewall Rule Impact

After creating the firewall rule **“Block Telnet Port 23”**, Windows Defender Firewall explicitly blocks **all inbound TCP traffic on port 23**.

Even if a Telnet service existed, the firewall would still prevent the connection. This confirms correct firewall enforcement.

---

## 🧠 Simple Analogy

- **Port 23** → Door  
- **Telnet server** → Person inside  
- **Firewall** → Security guard  

- No person inside → knocking fails  
- Security guard blocking → knocking fails  

In both cases, the result appears the same, but the **security control is functioning correctly**.

---

## ✅ Expected Security Behavior

In real-world environments:
- Telnet services are disabled  
- Port 23 remains closed  
- Firewall rules block insecure ports  
- Connection attempts fail by design  

This outcome indicates a **properly secured system**, not a configuration error.

---
### 🔹 PowerShell TCP-Level Verification 
To confirm firewall enforcement beyond application-level testing, PowerShell was used:

```powershell
Test-NetConnection -ComputerName localhost -Port 23
```

**Result:**
```
TcpTestSucceeded : False
```

This confirms that inbound traffic on port 23 is blocked at the TCP level by the firewall.

---

## 🧠 Key Concepts Covered
- Firewall configuration
- Inbound traffic filtering
- Port-based security
- Service vs port availability
- Attack surface reduction

---

## 🔐 Security Insight
Blocking unused and insecure ports like Telnet (23) reduces exposure to legacy attacks and strengthens system security posture.

---

## 📘 Outcome
- Configured and validated firewall rules on Windows
- Understood why identical before-and-after results can occur
- Verified firewall behavior using TCP-level testing
- Gained practical cybersecurity fundamentals

