# pfSense
pfSense is a free, open-source firewall and router operating system based on FreeBSD. It is designed to be installed on physical hardware or a virtual machine to create a dedicated, enterprise-grade network security gateway.
# Instalation of pfSense
We have to make account on Netgate, and but it for 0 $.
Then we select what type we want
i chosed a iso file because i want to intall it on my local lab in oracle vbox

First we need to pick our adapters, we need WAN and LAN.
I picked 1 bridged where i put WAN and 1 internal network on which i put LAN network.
Then i had to make room for disk space and istalled.
# Configuration of pfSense
Automaticlly i get and ip address from my router on first adapter
WAN - em0 -> v4/DHCP - 192.168.18.147/24 
LAN - em1 - v4 - 192.168.1.1/24 - This one i set in installation
# lesson learned
I wanted to see if my local computer see this machine so i pinged 18.30 to 18.147 and reqest timed out.
That was weird but to my suprise when i entered the firewall logs i noticed that there was connection for ICMP but was refused
