# Cybersecurity-Lab-Setup
A repository documenting the setup of a virtual cybersecurity lab environment.


**Week 01 | Practical Assignment WK1-PM1**

In this project I built a small virtual lab on my computer using VirtualBox and Kali Linux. I use it to practice networking, Linux basics, and safe, legal security testing. This repository shows how I set it up, how I tested it, and what I learned.

---

## 1. About This Project

I set up a safe practice lab where I can try security tools without risking my main computer. Everything runs inside a virtual machine (VM), so if something breaks, I can simply go back to a saved state.

## 2. Goals

1. Install VirtualBox.
2. Set up Kali Linux as a virtual machine.
3. Create a separate virtual network for the lab.
4. Learn what an IP address, subnet mask, gateway, and DNS are.
5. Check that the network works using basic Linux commands.
6. Make sure important security tools (like Nmap) are installed.
7. Save a snapshot so I can restore the VM later.
8. Write down each step with screenshots.

## 3. Tools Used

| Tool | What I used it for | Link |
|------|--------------------|------|
| Oracle VirtualBox | Running virtual machines | https://www.virtualbox.org/ |
| Kali Linux | Security practice system | https://www.kali.org/get-kali/ |
| 7-Zip | Extracting downloaded files | https://www.7-zip.org/download.html |
| GitHub | Saving and sharing my documentation | https://docs.github.com/ |

## 4. Lab Layout

```text
Host Computer (Windows)
        |
   VirtualBox
        |
   NAT Network (10.0.0.0/24)
        |
   Kali Linux VM (10.0.0.2)
```

## 5. My Lab Settings

| Item | Value |
|------|-------|
| Host operating system | Windows |
| Host RAM | 8 GB |
| VirtualBox version | 7.2.22 |
| Kali Linux version | 2026.2 |
| RAM given to Kali | 2 GB |
| Network type | NAT Network |
| Network name | NATNetwork |
| Network range | 10.0.0.0/24 |
| Kali IP address | 10.0.0.2 |
| Gateway | 10.0.0.1 |
| DNS server | 8.8.8.8 |
| Snapshot name | Kali-Linux-Static-ip-Internet-Working |

## 6. Steps I Followed

### Step 1: Get the computer ready
- Checked that I had enough RAM and disk space.
- Made sure virtualization was turned on.
- Downloaded the software from the official websites.

Download links: [VirtualBox](https://www.virtualbox.org/wiki/Downloads) | [Kali Linux](https://www.kali.org/get-kali/) 


### Step 2: Install VirtualBox
- Installed VirtualBox on Windows.
- Opened it to confirm it worked.

**Screenshot:**

![VirtualBox](images/step2-virtualbox.png)

### Step 3: Set up Kali Linux
- Downloaded the Kali VM and extracted it with 7-Zip.
- Added it to VirtualBox and set the memory.
- Started the VM.

**Screenshot:**

![Kali VM](images/step3-kali-vm.png)

### Step 4: Create the virtual network
- Created a NAT Network named `NATNetwork` with the range `10.0.0.0/24`.
- Connected the Kali network adapter to this network.
- Double-checked that the adapter was set to NAT Network.

**Screenshot:**

![Network](images/step4-network.png)

### Step 5: Set a static IP address in Kali
- Gave Kali a fixed IP address: `10.0.0.2`.
- Set the gateway to `10.0.0.1` and the DNS server to `8.8.8.8`.

![Ip address](images/step4-ipaddress.png)


### Step 6: Test the connection
- Checked the IP address, the route, and the gateway.
- Tested DNS and the internet.
- Confirmed that Nmap is installed.

- ```bash
ifconfig  
ip route
ping -c 4 [10.0.0.1]
nslookup google.com
ping -c 4 [8.8.8.8]

nmap

![Ip address-route-gateway](images/step7-iproute.png)
![DNS ](images/step7-dns.png)
![Nmap](images/step7-nmap.png)



### Step 7: Save a snapshot
- Took a VirtualBox snapshot named `Kali-Linux-Static-ip-Internet-Working`.
- I can go back to this point if I break something later.

**Screenshot:**

![Snapshot](images/step7-snapshot.png)

## 7. Test Results

| Test | Command | Result |
|------|---------|--------|
| IP address | `ip a` | Pass - `10.0.0.2/24` |
| Route | `ip route` | Pass - `default via 10.0.0.1` |
| Gateway reachable | `ping -c 4 10.0.0.1` | Pass - 4 sent, 4 received |
| DNS works | `nslookup google.com` | Pass - changed into ip address |
| Nmap installed | `nmap --version` | Pass - v7.99 |
| Snapshot saved | VirtualBox Snapshot Manager | Pass - `Kali-Linux-Static-ip-Internet-Working` |

**Overall result:** Completed. All tests passed.

## 8. Problems I Faced

## 8. Problems I Faced

### Problem 1: nmcli "unknown connection" error (spelling/case)

- **What happened:** Running `sudo nmcli connection modify "wired connection 1" ipv4.dad-timeout 0` returned `Error: unknown connection 'wired connection 1'`.
- **What I checked:** Ran `nmcli connection show` and saw the real name is `Wired connection 1` (capital W). nmcli names are case-sensitive.
- **How I fixed it:** Re-ran the command with the exact name:
  `sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0`
- **How I confirmed it worked:** No error returned, and `nmcli connection show "Wired connection 1" | grep dad-timeout` showed `ipv4.dad-timeout: 0`.

### Problem 2: NAT network IP not changed to 10.0.0.0

- **What happened:** I skipped the first step of setting the VirtualBox NAT network range to `10.0.0.0`, so the VM's network did not match the lab setup.
- **What I checked:** Reviewed the VirtualBox NAT network settings and the VM's IP with `ip a`. The address was not in the `10.0.0.x` range.
- **How I fixed it:** Changed the NAT network IP range to `10.0.0.0/24` in VirtualBox, then restarted the VM.
- **How I confirmed it worked:** `ip a` showed an address in the `10.0.0.x` range, and the network connection worked.

**Screenshot:**

![Problem 1](images/challenge1.png)

## 9. What I Learned

- **Virtual machines:** They let me test things safely, and I can undo mistakes.
- **IP addresses and subnet masks:** They decide which devices can talk to each other.
- **DNS:** It turns website names into IP addresses.
- **Snapshots:** They are a quick way to go back to a working setup.
- **Documentation:** Screenshots and test results make it easy for someone else to repeat my work.

## 10. Safety and Ethics

- Only test systems that I own or have permission to test.
- Keep the lab separate from my real network and important data.
- Check the network adapter settings before running any security tool.
- Download software only from official websites.
- Never upload passwords, private keys, or tokens to GitHub.
- Take a snapshot before making big changes.
- Use an isolated network when internet access is not needed.

## 11. References

1. [VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)
2. [Kali Linux Downloads](https://www.kali.org/get-kali/)
3. [7-Zip Downloads](https://www.7-zip.org/download.html)
4. [GitHub Repository Docs](https://docs.github.com/en/repositories)

## 12. Author Details

| Field | Details |
|-------|---------|
| Student name | Syeda Sarooj Kareem |
| Batch ID | [B084] |
| Training program | [Cybersecurity Internship] |
| Assignment | WK1-PM1 - Cybersecurity Lab Setup |
| Repository | https://github.com/Sarooj-kareem/Cybersecurity-Lab-Setup |
| Completion date | 30-10-2026 |

## Conclusion

This project gave me a working practice lab built with VirtualBox and Kali Linux. I learned how to set up a virtual network, give a machine a static IP, test connections, and save a snapshot.

It also showed me why it is important to check your setup before doing any security testing. This lab is a good base for learning network analysis, system hardening, and authorized penetration testing.

The exercise reinforced the importance of understanding system configuration before attempting security testing. It also highlighted the value of verification, troubleshooting, and recovery planning.

The lab provides a starting point for further learning in network analysis, system hardening, and authorized penetration testing.
