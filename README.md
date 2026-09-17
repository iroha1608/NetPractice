*This project has been created as part of the 42 curriculum by nsato.*

<table>
	<thead>
    	<tr>
      		<th style="text-align:center">English</th>
      		<th style="text-align:center"><a href="README_ja.md">Japanese</a></th>
    	</tr>
  	</thead>
</table>

<h1>
	 NetPractice
</h1> <H2>
	Discover the basics of networking.
</H2>


## 📖*Table of Contents*
1. [💡Description](#1-description)
	1. [Submission details](#1-1-submission-details)
	2. [Goal](#1-2-goal)
2. [✅Instructions](#2-instructions)
3. [🌈Resources](#3-resources)
	1. [Additional Sections](#3-1-additional-sections)
	2. [References](#3-2-references)
	3. [Use of AI](#3-3-use-of-ai)


## 💡1. Description

NetPractice is a hands-on networking project with ten progressive levels designed to teach the basics of computer networking.
Through interactive problem solving, you troubleshoot and configure non-functioning network diagrams and learn
TCP/IP addressing, subnet masks, default gateways, routing and the OSI model.
This browser-based training gives practical experience in network administration and prepares you for real-world system administration and network operation tasks. (quoted from the subject)

### 1-1. Submission details

- Submit via a Git repository. Only files within the repository will be evaluated.  
- **Place configuration files for 10 levels (one file per level) in the root directory of the repository.**  
- Place `README.md` in the root directory of the repository as well.  
- Name the files to match the actual exported files so that it is clear which file corresponds to which level.  

**Expected repository structure**:
```
.
├── README.md
├── level1.json
├── level2.json
├── level3.json
├── level4.json
├── level5.json
├── level6.json
├── level7.json
├── level8.json
├── level9.json
└── level10.json
```

### 1-2. Goal

To enable students to treat TCP/IP addressing not as a set of “procedures to be memorized” but as “rules that can be derived.” Specifically, the goal is for students to be able to independently determine the following:

- Determine the network to which an interface belongs based on a given IP address and subnet mask (`IP address AND subnet mask`).
- Determine whether two interfaces can communicate directly with each other.
- Determine which address to use as the next hop (default gateway) when a host needs to exit its own network.
- Trace how a router selects a route based on the destination IP address, using the routing table’s lookup rules.

## ✅2. Instructions

### 2-1. How to run
1. Download the file attached to the project page and extract it into a folder of your choice.  
2. In that folder, run `run.sh`. It starts a local web server and opens the dedicated page in your web browser.  

```bash
./run.sh
```

3. If `run.sh` does not work properly, start the server manually from the same folder. Because of browser security restrictions, the pages must be delivered over HTTP instead of being opened directly as local files.  

```bash
python3 -m http.server 49242
```

Then open `http://localhost:49242` in your browser (the port number can be changed).  

### 2-2. Training and evaluation

- **Training** tab: enter your intra login in the input field and start. Work through Level 1 to Level 10 in order.  

- **Evaluation** tab: generates a random configuration. Used for the peer-evaluation and for timed practice runs.  

### 2-3. Working through a level

1. The goal of the level and the current status are displayed at the top of the screen.  

2. Edit the white fields to fix the configuration.  

3. Press **[Check again]** to verify it. If it fails, read the logs at the bottom right of the page. They show where the packet stopped (e.g. a gateway is not set, an IP address is invalid, the destination matches no route).  

4. Once the status is OK, export the configuration with **[Get my config]** and place the file at the root of the repository.  

5. Press **[Next level]** to move on to the next level.  

### 2-4. Submission

- **10 exported configuration files (one per level) must be placed at the root of the repository**, together with this `README.md`.  

- Export the configuration before leaving a level: it can no longer be retrieved once you have moved on to the next one.  

## 🌈3. Resources

### 3-1. Additional Sections

- **TCP/IP addressing**  
  A logical address used to identify computers and devices on a network. IPv4 and IPv6 both exist, but this project only deals with IPv4.  
  An IPv4 address is 32 bits, written in decimal as four octets of 8 bits each. The bit weights inside an octet are `128 64 32 16 8 4 2 1`. An address is only fully determined once it is converted back into its bit representation, so binary ⇄ decimal conversion is the basis of every calculation.  

- **Subnet masks (CIDR)**  
  A mask consists of a run of 1s starting from the left (the network part) followed by 0s (the host part), and the 1 → 0 boundary occurs exactly once.
  The `/n` notation means that the first n bits are 1 (`/24` = `255.255.255.0`, `/26` = `255.255.255.192`, `/30` = `255.255.255.252`).
  The network an address belongs to is given by `IP AND mask`. In other words, **the mask alone determines the network boundary**.  

- **Network address / broadcast address / host range**  
  An address whose host part is all 0s is the network address, and one whose host part is all 1s is the broadcast address; neither can be assigned to an interface.
  The number of assignable hosts is `2^(32 - n) - 2`.
  A segment that only needs two devices, such as a router-to-router link, fits in a `/30`, the smallest usable subnet.

- **Default gateway**  
  The address a packet is handed to when its destination lies outside the local network.
  It must be the IP address of a router interface that sits in the same network as the sender.
  Pointing it at a distant address does not work, since reaching that address would itself require a way to get there first.

- **Routers and switches**  
  A switch only forwards frames within a single network and has neither an IP address nor a routing table.
  A router connects different networks and decides the next hop by matching the destination IP against its routing table.
  Therefore, devices attached to the same switch must belong to the same network.  

- **Routing table**  
  Each entry has the form `destination network / mask => gateway`.
  A match is decided by `destination IP AND entry mask == entry destination network`.
  Once an interface is given an IP address and a mask, the route to its directly connected network exists automatically, so only networks that are not directly connected need an entry.
  `0.0.0.0/0` is the default route, which matches every destination; it is used when there is only one way out.
  Communication requires routes in **both the forward and the return direction**: one side alone is not enough.  

- **Private address ranges**  
  `10.0.0.0/8`, `172.16.0.0/12` and `192.168.0.0/16` are not routed on the Internet.
  In levels that require communication with the Internet node, the segment concerned must be given public addresses.  

- **OSI layers**  
  This project deals mainly with layer 3 (the network layer), which provides the logical division by IP address and decides forwarding between networks.
  A switch operates at layer 2 (the data link layer) and is limited to forwarding frames within a single network.
  The distinction "same network, so it is delivered directly / different network, so it goes through a router" is exactly this division of roles between the two layers.


### 3-2. References

- Base specification of IPv4  
[RFC 791: STD 5: Internet Protocol](https://www.rfc-editor.org/info/rfc791/)  
- Original description of subnetting  
[RFC 950: STD 5: Internet Standard Subnetting Procedure](https://www.rfc-editor.org/info/rfc950/)  
- Private IP address ranges  
[RFC 1918: BCP 5: Address Allocation for Private Internets](https://www.rfc-editor.org/info/rfc1918/)  
- Specification of the CIDR notation  
[RFC 4632: BCP 122: Classless Inter-domain Routing (CIDR): The Internet Address Assignment and Aggregation Plan](https://www.rfc-editor.org/info/rfc4632/)  
- Subnetting exercises, cheat sheet and a calculator to check your answers  
[Subnetting Questions](https://subnettingpractice.com/)  
- Practice tool that generates random IP/CIDR pairs and asks for the network ID, broadcast address and host range  
[SubnetIPv4.com](https://subnetipv4.com/)  
- Verification module with explanations  
[SubnetCalculator.info Practice Module](https://www.subnetcalculator.info/PracticeModule)  


[The ultimate introduction: private and public IP addresses](https://atmarkit.itmedia.co.jp/ait/articles/1412/25/news029.html) (in Japanese)  
[Basics of IP addresses: what is the difference between IPv4 and IPv6?](https://www.securewave.co.jp/blog/035) (in Japanese)


### 3-3. Use of AI
**Claude**
- Asking questions about networking fundamentals.
- Explaining practice exercises.
- Suggesting sources to understand the concepts.
- Translating the README.
