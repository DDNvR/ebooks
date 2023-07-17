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
