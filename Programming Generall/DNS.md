#dns #internet #protcol #web

**<font color="#ffff00">DNS</font>** (**Domain Name System**) is a hierarchical and decentralized naming system for Internet connected resources.
in simple term it would translate name to ip and the process is called <font color="#92d050">DNS lookup</font>.

![[Pasted image 20230125162904.png]]
<font color="#ffff00">TLD</font> (**Top-Level Domain**): such as org com net
<font color="#ffff00">SLD</font> (**Secondary Level Domain**): The label located right before the *TLD* is also called a Secondary Level Domain .

A <font color="#00b0f0">DNS record</font> is a database record used to <u>map a URL to an IP address.</u> DNS records are stored in DNS servers and work to help users connect their websites to the outside world. When the URL is entered and searched in the browser, that URL is forwarded to the DNS servers and then directed to the specific Web server.
![[Pasted image 20230125163757.png]]

## Different types of DNS records are as follows:  
-   Name Server (NS) Record:
	- **Describes a name server for the domain that permits DNS lookups within several zones**.<u> Every primary as well as secondary name server must be reported via this record</u>.
-   Mail Exchange (MX) Record:
	- **Permits mail to be sent to the right mail** servers located in the domain. Other than IP addresses, MX records include fully-qualified domain names.
-   Address (A) Record:
	- **Used to map a host name to an IP address**. Generally, A records are IP addresses. If a computer consists of multiple IP addresses, adapter cards, or both, it must possess multiple address records.
-   Canonical Name (CNAME) Record:
	- Can be used to set an **alias for the host name**
-   Text (TXT) Record:
	- Permits the **insertion of arbitrary text** into a DNS record. These records add SPF records into a domain.
-   Time-to-Live (TTL) Record:
	- Sets the period of data, which is ideal when a recursive DNS server queries the domain name information
-   Start of Authority (SOA) Record:
	- Declares the most **authoritative host for the zone**. <u>Every zone file should include an SOA record</u>, which is generated automatically when the user adds a zone.
-   Pointer (PTR) Record:
	- Creates a pointer, which maps an IP address to the host name in order to do reverse lookups.

#### SOA DNS Record
A start of authority record is a type of resource record in the Domain Name System containing administrative information about the zone, especially regarding zone transfers, the usage is that if zone get's updated it would update the other name servers to update.