# PENETRATION TESTING REPORT
**FOOTPRINTING & NETWORK SCANNING PHASES**  
`W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS`

| Details | Information |
| :--- | :--- |
| **Pentester Name (Cybersecurity Professional)** | Ako Vagi |
| **Program / Batch** | B084-Networkwalks |
| **Date** | 13 November 2026 |
| **Modules Completed** | `W2-PM1` (Multiple Kali Tools)<br>`W2-PM5` (Zenmap Scanning) |
| **Client / Target** | 1. Networkwalks (`networkwalks.com`)<br>2. Authorized Local LAN Network (`10.63.227.0/24`) |
| **Permission Secured from Client?** | Yes (as declared for the assigned practical) |
| **Phases Covered** | **Phase 1:** Reconnaissance & Footprinting – Completed<br>**Phase 2:** Scanning & Network Discovery – Completed<br>**Phases 3–5:** In Progress |

---

## 1. Liability Disclaimer

I have performed these activities only on systems and devices where I had secured written permission or on devices and systems that I own myself. All these materials are for educational and research purposes only.

Do not use anything from this report to break the law. The instructor, the authors, and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility.

Misuse can lead to criminal charges, heavy fines, loss of your job, and a permanent criminal record. In most countries, unauthorized access is a crime even when nothing is damaged.

---

## 2. Introduction

This report covers the footprinting, reconnaissance, and network scanning activities performed as part of Week 2 of my ongoing Cybersecurity and Ethical Hacking internship program at Networkwalks.

The first practical exercise (`W2-PM1`) focused on gathering publicly available information about the `networkwalks.com` target domain using six Kali Linux tools: **WHOIS**, **WhatWeb**, **Nslookup**, **cURL**, **WAFW00F**, and **DNSRecon**. This involved identifying its IP address, examining DNS records, fingerprinting web technologies, inspecting HTTP response headers, identifying its Web Application Firewall (WAF), and obtaining domain registration information.

The second practical exercise (`W2-PM5`) focused on active network discovery on an authorized local subnet (`10.63.227.0/24`) using **Zenmap** (Nmap GUI) to identify live hosts, inspect IP and MAC addresses, and generate a visual network topology.

The findings were documented with terminal outputs and screenshots as evidence, together with explanations of their relevance from a cybersecurity and penetration testing perspective. The main objective of these practicals was to understand how penetration testers gather target intelligence and map active network hosts before proceeding to vulnerability assessment and further security testing.

---

## 3. Tools Used

The table below lists each tool used in this report and its purpose.

| Tool | Purpose |
| :--- | :--- |
| **Kali Linux** | Operating system used to perform external reconnaissance activities. |
| **WHOIS** | Retrieves domain registration information, registrar details, registration dates, privacy protection status, and name servers. |
| **WhatWeb** | Identifies web technologies, CMS platforms, plugins, page titles, contact emails, and server information. |
| **Nslookup** | Resolves a domain name into its corresponding IP address using DNS. |
| **cURL (`curl -I`)** | Retrieves HTTP response headers to examine server responses, caching layers, security policies, and exposed endpoints. |
| **WAFW00F (`v2.4.2`)** | Identifies whether a Web Application Firewall protects the target website. |
| **DNSRecon** | Enumerates DNS records, including SOA, NS, A, MX, TXT, and SRV records, as well as DNS server software versions. |
| **Zenmap (Nmap `7.993` GUI)** | Graphical interface for Nmap used to perform host discovery (Ping Scan) and generate network topology maps. |
| **Windows CMD** | Used to identify local IP address and subnet configuration (`ipconfig`). |

---

## 4. Activities Performed

### 4.1 Footprinting & Reconnaissance (`W2-PM1`)

I performed footprinting and reconnaissance against the `networkwalks.com` domain on 10 November 2026 using six Kali Linux tools: **WHOIS**, **WhatWeb**, **Nslookup**, **cURL**, **WAFW00F**, and **DNSRecon**. Each tool was used to gather specific information about the target website and its infrastructure.

#### 1. WHOIS – Domain Information Gathering
* **Command Used:** `whois networkwalks.com`
* **Results Identified:**
  * **Domain Name:** `NETWORKWALKS.COM` (Registry Domain ID: `2452319255_DOMAIN_COM-VRSN`)
  * **Registrar:** `GoDaddy.com, LLC` (IANA ID: `146`, Abuse Contact: `abuse@godaddy.com`)
  * **Domain Creation Date:** `6 November 2019` (`2019-11-06T22:51:46Z`)
  * **Updated Date:** `12 November 2025` (`2025-11-12T10:08:43Z`)
  * **Registry Expiry Date:** `6 November 2027` (`2027-11-06T22:51:46Z`)
  * **Registrant & Tech Privacy:** `Registration Private` via `Domains By Proxy, LLC` (Tempe, Arizona, US)
  * **Name Servers:** `NS6135.HOSTGATOR.COM` and `NS6136.HOSTGATOR.COM` (with `NS29.DOMAINCONTROL.COM` and `NS30.DOMAINCONTROL.COM` also listed in the registrar WHOIS block)
  * **DNSSEC:** `unsigned`
  * **Domain Status Restrictions:** `clientDeleteProhibited`, `clientRenewProhibited`, `clientTransferProhibited`, and `clientUpdateProhibited`
* **Observation:** WHOIS provided useful information about the domain's registration lifecycle, registrar, and hosting infrastructure. While WHOIS privacy (`Domains By Proxy, LLC`) masks personal registrant contact details and client lock codes protect against unauthorized domain transfers, the unsigned DNSSEC status indicates an area for security hardening.

#### 2. WhatWeb – Web Technology Fingerprinting
* **Command Used:** `whatweb networkwalks.com`
* **Results Identified:**
  * **HTTP Redirect Behavior:** `http://networkwalks.com` returned `301 Moved Permanently`, redirecting to `https://networkwalks.com/` (`200 OK`)
  * **Web Server & Location:** `Apache` hosted in the `UNITED STATES [US]`
  * **Content Management System:** `WordPress 7.1.4` (fingerprinted via MetaGenerator)
  * **JavaScript & Frontend Libraries:** `jQuery 3.7.1`, `Bootstrap 7.1.4`, Google Tag Manager, and external scripts
  * **WordPress Plugin:** `WordPress Download Manager 3.3.76`
  * **Site Metadata & Contact Info:** Page title `Networkwalks Academy`, `Open-Graph-Protocol[website]`, `Frame`, contact email `info@networkwalks.com`, and `__wpdm_client` cookie
* **Observation:** WhatWeb helped identify the technologies powering the website as well as public contact metadata (`info@networkwalks.com`). This information is useful during reconnaissance because attackers and security professionals can use technology fingerprints to identify software that may require further security assessment. Reported technology versions should be independently verified before being treated as confirmed installed versions.

#### 3. Nslookup – DNS Resolution
* **Command Used:** `nslookup networkwalks.com`
* **Results Identified:**
  * **DNS Server:** `8.8.8.8` (`8.8.8.8#53`)
  * **Answer Type:** Non-authoritative answer
  * **Domain Name:** `networkwalks.com`
  * **Resolved IP Address:** `192.232.216.135`
* **Observation:** Nslookup successfully resolved the domain name to its corresponding IPv4 address (`192.232.216.135`). This identifies the network address associated with the web service and provides a starting point for understanding its hosting infrastructure.

#### 4. cURL – HTTP Header Inspection
* **Command Used:** `curl -I https://networkwalks.com`
* **Results Identified:**
  * **HTTP Protocol & Status:** `HTTP/2 200`
  * **Web Server & Caching Headers:** `server: Apache`, `x-nginx-cache: WordPress`, and `x-endurance-cache-level: 0`
  * **Content Type:** `text/html; charset=UTF-8`
  * **WordPress Cookie:** `__wpdm_client` configured with `path=/; domain=networkwalks.com; secure; HttpOnly` flags
  * **Security Policies:** `referrer-policy: no-referrer-when-downgrade` and `permissions-policy` configuring `private-state-token-redemption` and `private-state-token-issuance` (referencing Google, gstatic, reCAPTCHA, Cloudflare Challenges, and hCaptcha)
  * **WordPress REST API Links:** `<https://networkwalks.com/wp-json/>` (`rel="https://api.w.org/"`) and `<https://networkwalks.com/wp-json/wp/v2/pages/53>`
* **Observation:** The website returned an `HTTP/2 200` response. While the session cookie properly enforces `Secure` and `HttpOnly` attributes, the HTTP headers expose the underlying Apache web server, Nginx/Endurance caching configuration, and WordPress REST API endpoints (`/wp-json/`).

#### 5. WAFW00F – Web Application Firewall Detection
* **Command Used:** `wafw00f networkwalks.com`
* **Results Identified:**
  * **Tool Version:** `WAFW00F v2.4.2`
  * **Target Checked:** `https://networkwalks.com`
  * **Detected WAF:** `ModSecurity (SpiderLabs)`
  * **Number of Requests:** `2`
* **Observation:** The results confirmed that the website uses `ModSecurity (SpiderLabs)` as its Web Application Firewall. A WAF provides an additional layer of security by inspecting incoming web requests and helping detect or block malicious traffic. However, detecting a WAF does not establish how effectively its ruleset is configured or whether it prevents every type of web attack.

#### 6. DNSRecon – DNS Enumeration
* **Command Used:** `dnsrecon -d networkwalks.com`
* **Results Identified:**
  * **DNSSEC Query:** Returned an error (`No answer for DNSSEC query for networkwalks.com`)
  * **SOA Record:** `ns6135.hostgator.com` (`50.87.144.87`)
  * **Name Servers & BIND Versions:** `ns6135.hostgator.com` (`50.87.144.87`) and `ns6136.hostgator.com` (`192.232.216.131`), both disclosing BIND version `"9.16.23-RH"`
  * **A Record:** `networkwalks.com` -> `192.232.216.135`
  * **MX Record:** `mail.networkwalks.com` -> `192.232.216.135`
  * **TXT Records:** SPF record (`v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all`) and Google site verification token (`google-site-verification=rr04eRmqHoWY3XemnizDNVK4q75X-Ij-mjgEeg-UsYI`)
  * **SRV Records:** Eight `_autodiscover._tcp.networkwalks.com` records pointing to `cpanelemaildiscovery.cpanel.net` on port `443` across eight IPv4 addresses (`184.94.203.9`, `184.94.203.11`, `184.94.203.14`, `184.94.203.15`, `184.94.204.9`, `184.94.204.11`, `184.94.204.14`, `184.94.204.15`)
* **Observation:** DNSRecon successfully enumerated the target's DNS, mail, and hosting infrastructure, confirming shared hosting/cPanel mail autodiscovery (`cpanelemaildiscovery.cpanel.net`), SPF email authentication policies, BIND DNS software versions (`9.16.23-RH`), and the absence of DNSSEC.

---

### 4.2 Network Scanning with Zenmap (`W2-PM5`)

The second practical activity (`W2-PM5`) involved using **Zenmap** (Nmap `7.993` GUI) to perform host discovery and network mapping on my own authorized local network (`10.63.227.0/24`).

* **Status:** Completed
* **Scan Timestamp:** 12 November 2026 at 16:49 (`+1000`)
* **Target Subnet:** `10.63.227.0/24` (256 IP addresses scanned)
* **Scan Profile:** Ping Scan
* **Command Executed:** `nmap -sn 10.63.227.0/24`
* **Scan Duration & Summary:** Completed in `8.50 seconds` with **2 active hosts discovered** out of 256 IP addresses scanned.

#### Discovered Hosts Summary

| Host IP Address | Status | Latency | MAC Address | Vendor / Role Notes |
| :--- | :--- | :--- | :--- | :--- |
| `10.63.227.114` | Up | `0.0071s` | `76:A7:88:7E:2C:18` | Vendor reported as `Unknown` (locally administered / randomized MAC address on the local subnet). |
| `10.63.227.37` | Up | Local Host | Not displayed by Nmap | Local scanning interface (`localhost`), directly linked to `10.63.227.114` in the topology diagram. |

#### Network Topology & Observations
* **Topology Mapping:** Using Zenmap's Topology view, the network map confirmed `localhost` (`10.63.227.37`) connected directly to the active peer/gateway node `10.63.227.114`, and the topology diagram was exported for documentation.
* **Observation:** Performing a Ping Scan (`-sn`) efficiently identified active devices on the `10.63.227.0/24` subnet without sending intrusive port-scan probes. Because `10.63.227.37` is the local scanning system itself, Nmap reports its status as up without listing a remote ARP MAC entry, whereas `10.63.227.114` responded over the local network with low latency (`0.0071s`) and exposed its hardware/randomized MAC address (`76:A7:88:7E:2C:18`).

---

## 5. Risk Analysis / Impact

Based on the information collected during the footprinting, reconnaissance, and local network scanning activities, I identified the following potential risks and security observations:

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :--- | :--- | :--- | :--- | :--- |
| **1** | Web technology and plugin versions exposed | WhatWeb identified `Apache`, `WordPress 7.1.4`, `jQuery 3.7.1`, `Bootstrap 7.1.4`, and `WordPress Download Manager 3.3.76`. | Attackers may use exposed CMS and plugin versions to research known vulnerabilities or target specific components. | **Medium** |
| **2** | Server IP address identifiable | Nslookup and DNSRecon resolved `networkwalks.com` and `mail.networkwalks.com` to `192.232.216.135`. | Reveals the hosting network address of the web and mail services, enabling direct infrastructure profiling. | **Low** |
| **3** | HTTP headers and REST API endpoints exposed | cURL revealed `Apache`, `x-nginx-cache: WordPress`, `x-endurance-cache-level: 0`, and `/wp-json/` REST API links. | Exposes server stack details and API routes that can assist automated enumeration. | **Low** |
| **4** | WAF technology identifiable | WAFW00F `v2.4.2` identified `ModSecurity (SpiderLabs)`. | Reveals the defensive WAF product in place, which may allow attackers to tailor evasion techniques. | **Low** |
| **5** | DNS infrastructure and BIND versions exposed | DNSRecon enumerated SOA, NS, MX, TXT (SPF), and 8 SRV records, plus BIND version `"9.16.23-RH"` on both name servers. | Exposes name server software versions and mail/cPanel architecture (`cpanelemaildiscovery.cpanel.net`). | **Medium** |
| **6** | Domain registration metadata and contact email public | WHOIS showed registrar/expiry details (`GoDaddy.com, LLC`, expires `2027-11-06`) while WhatWeb extracted `info@networkwalks.com`. | Although registrant PII is protected by `Domains By Proxy, LLC`, public administrative timelines and contact emails can assist social engineering. | **Low** |
| **7** | DNSSEC not enabled | Both WHOIS (`DNSSEC: unsigned`) and DNSRecon (`No answer for DNSSEC query`) confirmed DNSSEC is disabled. | Without cryptographic DNSSEC signing, the domain lacks protection against DNS spoofing and cache poisoning attacks. | **Medium** |
| **8** | Active hosts and MAC address discoverable on local subnet | Zenmap Ping Scan (`nmap -sn 10.63.227.0/24`) discovered 2 active hosts (`10.63.227.37` and `10.63.227.114`) and exposed MAC `76:A7:88:7E:2C:18`. | Devices on the same local broadcast domain respond to ICMP/ARP host discovery, allowing connected peers to map active hosts. | **Low** |

**Risk Level Key:**
* **Critical** – Severe security impact.
* **Medium** – Potential security concern requiring further investigation.
* **Low** – Limited security impact or information exposure.

> **Disclaimer:** The risks above are observations from the reconnaissance and host discovery exercises, not confirmed vulnerabilities. No exploitation or vulnerability validation was performed. Therefore, the presence of a software version, IP address, DNS record, WAF fingerprint, or active LAN host does not automatically mean that the target is vulnerable; further authorized security testing would be required to confirm any actual vulnerabilities.

---

## 6. Recommendations

Based on the observations from the footprinting, reconnaissance, and network scanning activities, I recommend the following security improvements:

1. **Minimize publicly exposed technology fingerprints:** Remove or suppress unnecessary generator meta tags (`WordPress`, `WordPress Download Manager`) and restrict unauthenticated access to `/wp-json/` REST API endpoints where not required.
2. **Keep software updated:** WordPress core, plugins (such as WordPress Download Manager), themes, JavaScript libraries (`jQuery`, `Bootstrap`), and Apache/BIND server software should be regularly updated and reviewed against current security advisories.
3. **Harden HTTP response headers:** Suppress verbose server and cache headers (`Server: Apache`, `x-endurance-cache-level`, `x-nginx-cache`) and add defense-in-depth security headers such as `Strict-Transport-Security` (HSTS), `Content-Security-Policy` (CSP), and `X-Content-Type-Options: nosniff`.
4. **Review DNS records and suppress BIND version disclosure:** Configure name servers (`ns6135.hostgator.com` and `ns6136.hostgator.com`) to disable `version.bind` responses (`9.16.23-RH`) and periodically audit TXT and SRV records.
5. **Properly configure and monitor the WAF:** Keep `ModSecurity (SpiderLabs)` enabled and properly configured, with OWASP Core Rule Set (CRS) rules regularly reviewed and updated to protect against malicious web requests.
6. **Enable DNSSEC:** Work with the registrar (`GoDaddy.com, LLC`) and DNS host (`HostGator`) to enable DNSSEC signing and DS record validation to protect against DNS spoofing and cache poisoning.
7. **Maintain domain registrar protections:** Keep WHOIS privacy protection (`Domains By Proxy, LLC`) and registrar lock statuses (`clientTransferProhibited`, `clientDeleteProhibited`, `clientUpdateProhibited`) active alongside multi-factor authentication on registrar accounts.
8. **Perform regular network discovery:** Periodically scan internal networks (`10.63.227.0/24`) to identify active devices and maintain an accurate asset inventory of connected systems.
9. **Investigate unknown devices:** Verify any unexpected IP or unrecognized MAC address (`76:A7:88:7E:2C:18`) discovered during LAN scanning to confirm whether it is an authorized device using MAC randomization or an unauthorized endpoint.
10. **Perform security testing with authorization:** Reconnaissance, scanning, and vulnerability assessments must only be performed against systems and networks where explicit written authorization has been provided.

---

## 7. Conclusion

During Week 2 of my Cybersecurity and Ethical Hacking internship at Networkwalks, I completed practical activities covering external footprinting and reconnaissance (`W2-PM1`) as well as local network discovery (`W2-PM5`).

In the footprinting activity, I used WHOIS to collect domain registration and privacy details, WhatWeb to fingerprint WordPress and plugin versions, Nslookup to resolve the target IPv4 address (`192.232.216.135`), cURL to inspect `HTTP/2` headers and caching layers, WAFW00F to detect the `ModSecurity (SpiderLabs)` Web Application Firewall, and DNSRecon to enumerate SOA, NS, MX, TXT, and SRV records for `networkwalks.com`. In the network scanning activity, I used Zenmap (`nmap -sn 10.63.227.0/24`) to discover active hosts (`10.63.227.37` and `10.63.227.114`), inspect latency and MAC address details, and generate a visual network topology.

These exercises reinforced that structured reconnaissance and host discovery are essential foundational stages of penetration testing. Combining multiple tools provides complementary visibility across external web infrastructure and internal network topology, enabling accurate documentation, risk assessment, and actionable security recommendations. The next step in my learning journey is to build upon these findings as I progress into Phases 3–5 within an authorized scope.

---

## 8. Evidences Collected

The following terminal outputs and screenshots provide evidence of the practical activities performed during the footprinting, reconnaissance, and network scanning phases:

### Evidence 1: cURL – HTTP Header Inspection
* **Command:** `curl -I https://networkwalks.com`

```text
HTTP/2 200 
permissions-policy: private-state-token-redemption=(self "[https://www.google.com](https://www.google.com)" "[https://www.gstatic.com](https://www.gstatic.com)" "[https://recaptcha.net](https://recaptcha.net)" "[https://challenges.cloudflare.com](https://challenges.cloudflare.com)" "[https://hcaptcha.com](https://hcaptcha.com)"), private-state-token-issuance=(self "[https://www.google.com](https://www.google.com)" "[https://www.gstatic.com](https://www.gstatic.com)" "[https://recaptcha.net](https://recaptcha.net)" "[https://challenges.cloudflare.com](https://challenges.cloudflare.com)" "[https://hcaptcha.com](https://hcaptcha.com)")
link: [https://networkwalks.com/wp-json/](https://networkwalks.com/wp-json/); rel="[https://api.w.org/](https://api.w.org/)", [https://networkwalks.com/wp-json/wp/v2/pages/53](https://networkwalks.com/wp-json/wp/v2/pages/53); rel="alternate"; title="JSON"; type="application/json", [https://networkwalks.com/](https://networkwalks.com/); rel=shortlink
set-cookie: __wpdm_client=61e4670f41cf6ec404642d0aac38834f; path=/; domain=networkwalks.com; secure; HttpOnly
referrer-policy: no-referrer-when-downgrade
x-endurance-cache-level: 0
x-nginx-cache: WordPress
content-type: text/html; charset=UTF-8
date: Tue, 10 Nov 2026 11:24:18 GMT
server: Apache
```

### Evidence 2: DNSRecon – DNS Enumeration
* **Command:** `dnsrecon -d networkwalks.com`

```text
2026-11-10T06:41:24.437609-0500 INFO Starting enumeration for domain: networkwalks.com
2026-11-10T06:41:24.439559-0500 INFO std: Performing General Enumeration against: networkwalks.com...
2026-11-10T06:41:28.449518-0500 ERROR No answer for DNSSEC query for networkwalks.com
2026-11-10T06:41:31.204404-0500 INFO     SOA ns6135.hostgator.com 50.87.144.87
2026-11-10T06:41:33.765568-0500 INFO     NS ns6135.hostgator.com 50.87.144.87
2026-11-10T06:41:34.515988-0500 INFO     Bind Version for 50.87.144.87 "9.16.23-RH"
2026-11-10T06:41:34.516358-0500 INFO     NS ns6136.hostgator.com 192.232.216.131
2026-11-10T06:41:35.203275-0500 INFO     Bind Version for 192.232.216.131 "9.16.23-RH"
2026-11-10T06:41:36.532584-0500 INFO     MX mail.networkwalks.com 192.232.216.135
2026-11-10T06:41:36.852326-0500 INFO     A networkwalks.com 192.232.216.135
2026-11-10T06:41:37.717637-0500 INFO     TXT networkwalks.com v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all
2026-11-10T06:41:37.717994-0500 INFO     TXT networkwalks.com google-site-verification=rr04eRmqHoWY3XemnizDNVK4q75X-Ij-mjgEeg-UsYI
2026-11-10T06:41:38.164924-0500 INFO Enumerating SRV Records
2026-11-10T06:41:41.586691-0500 INFO     SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.203.15 443
2026-11-10T06:41:41.587206-0500 INFO     SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.203.9 443
2026-11-10T06:41:41.587336-0500 INFO     SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.204.9 443
2026-11-10T06:41:41.587440-0500 INFO     SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.204.11 443
2026-11-10T06:41:41.587591-0500 INFO     SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.204.14 443
2026-11-10T06:41:41.587733-0500 INFO     SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.203.14 443
2026-11-10T06:41:41.587888-0500 INFO     SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.204.15 443
2026-11-10T06:41:41.588130-0500 INFO     SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.203.11 443
2026-11-10T06:41:41.591033-0500 INFO 8 Records Found
2026-11-10T06:41:41.591824-0500 INFO Completed enumeration for domain: networkwalks.com
```

### Evidence 3: Nslookup – DNS Resolution
* **Command:** `nslookup networkwalks.com`

```text
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
Name:   networkwalks.com
Address: 192.232.216.135
```

### Evidence 4: WAFW00F – Web Application Firewall Detection
* **Command:** `wafw00f networkwalks.com`

```text
~ WAFW00F : v2.4.2 ~
The Web Application Firewall Fingerprinting Toolkit

[*] Checking [https://networkwalks.com](https://networkwalks.com)
[+] The site [https://networkwalks.com](https://networkwalks.com) is behind ModSecurity (SpiderLabs) WAF.
[~] Number of requests: 2
```

### Evidence 5: WhatWeb – Web Technology Fingerprinting
* **Command:** `whatweb networkwalks.com`

```text
[http://networkwalks.com](http://networkwalks.com) [301 Moved Permanently] Apache, Cookies[__wpdm_client], Country[UNITED STATES][US], HTTPServer[Apache], HttpOnly[ermissions-policy,x-redirect-by,upgrade,referrer-policy,x-endurance-cache-level,x-nginx-cache]
[https://networkwalks.com](https://networkwalks.com) [200 OK] Apache, Bootstrap[7.1.4], Cookies[__wpdm_client], Country[UNITED STATES][US], Email[info@networkwalks.c135], JQuery[3.7.1], MetaGenerator[WordPress 7.1.4,WordPress Download Manager 3.3.76], Open-Graph-Protocol[website], Script[4684NR-IPIB&aionrules,text/javascript], Title[Networkwalks Academy], UncommonHeaders[permissions-policy,link,upgrade,referrer-policy,x-endurance-cache
[https://networkwalks.com/](https://networkwalks.com/) [200 OK] Apache, Bootstrap[7.1.4], Country[UNITED STATES][US], Email[info@networkwalks.com], Frame, Google-Tag-.4,WordPress Download Manager 3.3.76], Open-Graph-Protocol[website], Script[4684NR-IPIB&amp;pidnVar2=50511&amp;prtVar2=7&amp;scvVar2=12,aademy], UncommonHeaders[permissions-policy,link,upgrade,referrer-policy,x-endurance-cache-level,x-nginx-cache], WordPress[7.1.4]
```

### Evidence 6: WHOIS – Domain Registration Information
* **Command:** `whois networkwalks.com`

```text
Domain Name: NETWORKWALKS.COM
Registry Domain ID: 2452319255_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.godaddy.com
Updated Date: 2025-11-12T10:08:43Z
Creation Date: 2019-11-06T22:51:46Z
Registry Expiry Date: 2027-11-06T22:51:46Z
Registrar: GoDaddy.com, LLC (IANA ID: 146)
Domain Status: clientDeleteProhibited / clientRenewProhibited / clientTransferProhibited / clientUpdateProhibited
Registrant/Tech Organization: Domains By Proxy, LLC (Registration Private)
Name Servers: NS6135.HOSTGATOR.COM, NS6136.HOSTGATOR.COM, NS29.DOMAINCONTROL.COM, NS30.DOMAINCONTROL.COM
DNSSEC: unsigned
```

### Evidence 7: Zenmap – Local Network Ping Scan (`W2-PM5`)
* **Command:** `nmap -sn 10.63.227.0/24`

```text
Starting Nmap 7.993 ( [https://nmap.org](https://nmap.org) ) at 2026-11-12 16:49 +1000
Nmap scan report for 10.63.227.114
Host is up (0.0071s latency).
MAC Address: 76:A7:88:7E:2C:18 (Unknown)
Nmap scan report for 10.63.227.37
Host is up.
Nmap done: 256 IP addresses (2 hosts up) scanned in 8.50 seconds
```

### Evidence 8: Zenmap – Local Network Topology Diagram (`W2-PM5`)
* **Topology View:** Displays `localhost` (`10.63.227.37`) linked directly to active peer node `10.63.227.114`.
* *(Upload `zenmap.png` and `network topology.png` to your repository and link them here using `![Zenmap Output](zenmap.png)` and `![Network Topology](network%20topology.png)`.)*

---

**-End-**

### Author & Project Information
* **Author:** Ako Vagi — Cybersecurity Professional (`B084`)
* **Program:** Cybersecurity & Ethical Hacking Program at Networkwalks
* **Week:** 02
* **Modules:** `W2-PM1` (Multiple Kali Tools) & `W2-PM5` (Zenmap Scanning)
* **Project:** Footprinting & Network Scanning
* **Repository:** [NETWORKWALKS-WK2-B084-PENETRATION-TESTING-REPORT](https://github.com/ako123-dev/NETWORKWALKS-WK2-B084-PENETRATION-TESTING-REPORT.git)
* **LinkedIn:** https://lnkd.in/p/gXHZd5jv
