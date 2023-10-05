0x08-networking 
In computer networking, localhost is a hostname that refers to the current computer used to access it. The name localhost is reserved for loopback purposes.[1] It is used to access the network services that are running on the host via the loopback network interface. Using the loopback interface bypasses any local network interface hardware.

Loopback
The local loopback mechanism may be used to run a network service on a host without requiring a physical network interface, or without making the service accessible from the networks the computer may be connected to. For example, a locally installed website may be accessed from a Web browser by the URL http://localhost to display its home page.

IPv4 network standards reserve the entire address block 127.0.0.0/8 (more than 16 million addresses) for loopback purposes.[2] That means any packet sent to any of those addresses is looped back. The address 127.0.0.1 is the standard address for IPv4 loopback traffic; the rest are not supported by all operating systems. However, they can be used to set up multiple server applications on the host, all listening on the same port number. In the IPv6 addressing architecture[3] there is only a single address assigned for loopback: ::1. The standard precludes the assignment of that address to any physical interface, as well as its use as the source or destination address in any packet sent to remote hosts.

Name resolution
The name localhost normally resolves to the IPv4 loopback address 127.0.0.1, and to the IPv6 loopback address ::1.

This resolution is normally configured by the following lines in the operating system's hosts file:

127.0.0.1    localhost
::1          localhost
The name may also be resolved by Domain Name System (DNS) servers, but there are special considerations[1] governing the use of this name:

An IPv4 or IPv6 address query for the name localhost must always resolve to the respective loopback address.
Applications may resolve the name to a loopback address themselves, or pass it to the local name resolver mechanisms.
When a name resolver receives an address (A or AAAA) query for localhost, it should return the appropriate loopback addresses, and negative responses for any other requested record types. Queries for localhost should not be sent to caching name servers.
To avoid burdening the Domain Name System root servers with traffic, caching name servers should never request name server records for localhost, or forward resolution to authoritative name servers.
When authoritative name servers receive queries for 'localhost' in spite of the provisions mentioned above, they should resolve them appropriately.
In addition to the mapping of localhost to the loopback addresses (127.0.0.1 and ::1), localhost may also be mapped to other IPv4 (loopback) addresses and it is also possible to assign other, or additional, names to any loopback address. The mapping of localhost to addresses other than the designated loopback address range in the hosts file or in DNS is not guaranteed to have the desired effect, as applications may map the name internally.

In the Domain Name System, the name .localhost is reserved as a top-level domain name, originally set aside to avoid confusion with the hostname localhost.[4] Domain name registrars are precluded from delegating domain names in the top-level .localhost domain.[1]
