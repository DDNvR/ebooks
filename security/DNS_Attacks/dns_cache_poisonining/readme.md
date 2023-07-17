dns poisoning attack


Introduction
SAD DNS is a revival of the classic DNS cache poisoning attack (which no longer works since 2008) leveraging novel network side channels that exist in all modern operating systems, including Linux, Windows, macOS, and FreeBSD. This represents an important milestone --- the first weaponizable network side channel attack that has serious security impacts. The attack allows an off-path attacker to inject a malicious DNS record into a DNS cache (e.g., in BIND, Unbound, dnsmasq).

DNS is a universal protocol on the Internet, which translates the human-readable domain names into computer-readable IP addresses. Almost every networking application relies on it. More specifically, SAD DNS attack allows an attacker to redirect any traffic (originally destined to a specific domain) to his own server and then become a man-in-the-middle (MITM) attacker, allowing eavesdropping and tampering of the communication.

Click to check if your DNS server is affected.
Please be patient while the page is loading, which means your DNS server is being checked.
(We will NOT attack your DNS server by any means.)

Publications

DNS Cache Poisoning Attack Reloaded: Revolutions with Side Channels(Distinguished Paper Award) [PDF] [Slides] [Video]
Keyu Man, Zhiyun Qian, Zhongjie Wang, Xiaofeng Zheng, Youjun Huang, Haixin Duan
In Proceedings of ACM Conference on Computer and Communications Security (CCS`20), November 9-13, 2020, Virtual Event, USA.


DNS Cache Poisoning Attack: Resurrections with Side Channels [PDF] [Slides] [Video]
Keyu Man, Xin'an Zhou, Zhiyun Qian
In Proceedings of ACM Conference on Computer and Communications Security (CCS`21), November 15-19, 2021, Virtual Event, Republic of Korea.


Q&A
Am I affected by the vulnerability?

Likely, as long as you are using a vulnerable DNS service (e.g., 8.8.8.8 or 1.1.1.1). Most public resolvers have been checked to be vulnerable. If you are using private DNS services (i.e., those provided by your ISP or your organization), we do not have sufficient data but there is a good chance that it is vulnerable as well. Refer to this question for more details.


What applications are affected?

Any networking application using DNS to retrieve the IP address of peers/servers are affected. Besides, vulnerable/affected DNS softawre include but not limited to BIND, Unbound and dnsmasq.

How widespread is this?
According to our measurement, 35% of open resolvers are vulnerable to the attack. We also found 12/14 public resolvers and 4/6 routers made by well-known brands are vulnerable. In theory, any DNS server running the newer version of popular operating systems without blocking outgoing ICMPs (only Windows blocks it by default) is also vulnerable. Refer to this question for the versions of the vulnerable operating system.
Can I notice that I'm under the attack of SAD DNS?
No, the attack happens transparently to the end-user. Only your ISP or DNS providers can potentially detect it (e.g., through IDS). Refer to this question.
What makes SAD DNS unique?
Unlike a bug that affects a certain piece of software, SAD DNS leverages fundamental flaws (e.g., network side channels) in the networking stack of operating systems. Furthermore, this represents the first weaponizable network side channel attack against high profile network applications.
Is this a design flaw of the protocols?
Mostly implementation. Specifically, the global rate limit of outgoing ICMP messages described in RFC1812 4.3.2.8  that introduces the side channel.
Has SAD DNS been abused in the wild?
We do not know yet but we have responsibly disclosed the vulnerability months before publishing the paper. It is likely many servers are patched already.
Has SAD DNS been patched?
Yes, we have worked with the Linux kernel security team and developed a patch  that randomizes the ICMP global rate limit to introduce noises to the side channel. Please refer to Security Advisories for more recent updates.
What versions of the operating system were affected?
Linux 3.18-5.10
Windows Server 2019 (version 1809) and newer (we did not test older versions)
macOS 10.15 and newer (we did not test older versions)
FreeBSD 12.1.0 and newer (we did not test older versions)
The patch  for Linux is integrated into 5.10 and backported to many stable versions. However, we don't know how and when Windows/macOS/FreeBSD will patch this vulnerability.
Why is it called SAD DNS?
SAD DNS = Side channel AttackeD DNS.
Can IDS detect this attack?
Possibly. The following may be effective:
Detect the timing pattern of the traffic: the attack sends a burst of packets every 50ms.
Detect UDP port scanning.
Detect wrong TxIDs for incoming DNS responses: the attack needs to brute force TxID but normal DNS responses are unlikely to present the wrong TxID value.
Can NAT gateway protect the resolver?
No, ironically NAT would even make patched DNS servers vulnerable again because the NAT gateway itself is subject to port scanning. Please consider patch  the NAT gateway as well.
Is this a man-in-the-middle(MITM) attack?
No, this is a totally off-path attack. The attacker is not required to sniff the traffic between the DNS servers, but a key requirement is that the attacker has the IP spoofing capability to impersonate the legitimate server. IP Spoofing is still feasible today.
How to fix SAD DNS?
There are three kinds of actions that could be taken to mitigate the attack:
Destroy the side channel
Disable outgoing ICMP
Randomize ICMP global rate limit (used by Linux )
Add more secrets to DNS messages
DNSSEC
0x20 encoding
DNS cookie
Reduce the attack window
Reduce the timeout for outstanding queries
Does DNSSEC mitigate the attack?
Yes and no, the server must implement strict DNSSEC check (i.e., refuse the responses that break the trust chain) to prevent the off-path attacks. However, since DNSSEC is still under development and servers need to accept such responses (i.e., only DNSSEC aware but not DNSSEC validate) when visiting a misconfigured domain.
Does DNS-over-HTTPS(DoH) mitigate the attack?
No, currently DoH only encrypts the traffic between the DNS client and DNS resolvers. However, SAD DNS attacks the link between resolvers and authoritative name servers, which is not protected by DoH.
Is it easy to get a host that is capable of IP spoofing?
Yes, according to CAIDA , 30.5% ASes don’t block packets with spoofed source IP addresses in 2019. In practice, an attacker needs to find only one node that can spoof IPs to carry out such an attack. In fact, such kind of bullet-proof-hosting service can be easily found with low prices($50) by simply Googling.
Proof of Concept
Code release (SADDNS 2.0 source is now publicly available!)
