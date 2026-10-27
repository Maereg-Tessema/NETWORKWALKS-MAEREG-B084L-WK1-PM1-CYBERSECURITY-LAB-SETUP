# My Cybersecurity Lab Setup (VirtualBox + Kali Linux)

This is my Week 1 project for the Networkwalks Cybersecurity program. I built a small virtual lab on my own laptop where I can safely practice security tools later in the course.

## What I built

- A Kali Linux 2026.3 virtual machine running in VirtualBox
- A private NAT Network called NatNetwork (10.0.0.0/24)
- Kali with a fixed IP address, 10.0.0.2/24, and working internet
- Copy/paste and drag-and-drop between Windows and Kali
- My Windows Downloads folder shared with Kali
- A snapshot so I can go back to a working state if I break something

## How I did it

**1. Installed VirtualBox and 7-Zip**

I installed VirtualBox on my Windows laptop. I also installed 7-Zip, because the Kali download comes as a compressed .7z file.

<img width="959" height="539" alt="Screenshot 2026-10-27 155708" src="https://github.com/user-attachments/assets/ee90074a-a44e-4d68-bb4b-98f5bfb9f777" />





**2. Created the NAT Network**

In VirtualBox I went to Tools > Network > NAT Networks and created NatNetwork. VirtualBox suggests 10.0.2.0/24 by default, so I changed the prefix to 10.0.0.0/24 and kept DHCP on.

<img width="534" height="535" alt="nat" src="https://github.com/user-attachments/assets/444301da-86ea-4daa-886a-c830497f05db" />

**3. Downloaded and imported Kali**

I downloaded the pre-built VirtualBox image from kali.org, extracted it with 7-Zip, and opened the .vbox file to import it.

<img width="959" height="539" alt="Screenshot 2026-10-27 155708" src="https://github.com/user-attachments/assets/c48de918-3bc7-4c6d-bf11-c32c7324c9e6" />


**4. Changed the VM settings**


- Network: Adapter 1 attached to NAT Network (NatNetwork), Promiscuous Mode set to Allow All
- General > Advanced: Shared Clipboard and Drag'n'Drop set to Bidirectional
- Shared Folders: added my Windows Downloads folder with Auto-mount on

<img width="959" height="539" alt="Screenshot 2026-10-27 161328" src="https://github.com/user-attachments/assets/1e047c97-2786-4c0a-b9d1-de7c8836d820" />

<img width="959" height="538" alt="Screenshot 2026-10-27 161505" src="https://github.com/user-attachments/assets/7fe10b3e-235f-4464-a50d-e7933ad155e9" />


<img width="959" height="539" alt="Screenshot 2026-10-27 161621" src="https://github.com/user-attachments/assets/96829006-aaf0-4425-ae9a-138b89bb6b80" />



**5. Set the IP address**

I gave Kali a manual IPv4 address: 10.0.0.2/24, gateway 10.0.0.1, DNS 8.8.8.8.

<img width="959" height="459" alt="Screenshot 2026-10-27 162333" src="https://github.com/user-attachments/assets/6fd9ad20-ee0c-480d-a4c4-2ef335900da4" />


**6. Tested everything**

I ran `ip a` to confirm the address, then pinged 8.8.8.8 and google.com. Both worked with 0% packet loss. The shared folder showed up on the Kali desktop as sf_Downloads.

<img width="957" height="482" alt="Screenshot 2026-10-27 162606" src="https://github.com/user-attachments/assets/1fb47067-b50b-4b1d-aa3e-6f9130085da6" />


**7. Took a snapshot**

I saved a snapshot called "kali working 10.0.0.2" once everything worked.

<img width="959" height="538" alt="Screenshot 2026-10-27 163009" src="https://github.com/user-attachments/assets/c367919d-9d1c-4fef-9b40-23582ba7c3d0" />

## Problems I ran into

**Problem: No IP address and no internet in Kali**


When I tried to bring the connection up, I got the error "IP configuration could not be reserved." The interface only had an IPv6 address and the pings failed. I also made a couple of typing mistakes in the commands (a misspelled word and a lowercase "w" in "Wired connection 1"), so I learned that Linux commands are case-sensitive. What fixed it was setting the address with nmcli and turning off the duplicate-address check:

<img width="355" height="400" alt="problem" src="https://github.com/user-attachments/assets/e71a23c6-3a3d-4579-b000-9b931e1047e3" />



`sudo nmcli connection modify "Wired connection 1" ipv4.method manual ipv4.addresses 10.0.0.2/24 ipv4.gateway 10.0.0.1 ipv4.dns 8.8.8.8 ipv4.dad-timeout 0`

After restarting the connection, `ip a` showed 10.0.0.2/24 and the pings worked.

## What I learned

- How a NAT Network lets several virtual machines share a private network and still reach the internet
- Why it's worth keeping VM files in a simple folder path
- How to read an error message and fix network problems from the terminal
- Snapshots are a safe way to experiment
