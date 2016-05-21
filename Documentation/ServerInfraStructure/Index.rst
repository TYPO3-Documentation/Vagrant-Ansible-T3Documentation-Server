============================
Accessing the Infrastructure
============================

This text has been taken as a copy on 2016-05-22 from
https://forge.typo3.org/projects/team-server-public/wiki/Accessing_the_Infrastructure


*Target audience:*
*Community members that are responsible for a service that is running within
the typo3.org infrastructure.*

Currently the \*.typo3.org server infrastructure is undergoing changes towards a
new architecture. This has implications for all community members that need to
access any servers via SSH or responsible for a service that requires incoming
connections.

Summary
=======

Before you can connect to the server again, you need to

-  Install OpenVPN.

-  If you are using Windows, make sure that OpenVPN is run with administrator rights:
   Right-click the Desktop icon → Properties → Compatibility → Settings → Mark
   "Run this program as administrator".

-  Download our OpenVPN configuration file from ((add-link-here":here))
   and move it into the config/ subfolder inside the OpenVPN program folder
   (for example :file:`C:\Programme\OpenVPN\config`, :file:`/etc/openvpn/`).

-  Start OpenVPN using the Desktop icon.

-  Connecting and disconnecting can be done by right-clicking the symbol in the
   task bar.


Overview
========

-  KVM is used for virtualization.

-  All servers are located in one data center (at Hetzner).

-  All VMs have only private IP addresses. While they have unregulated internet
   access via NAT, they cannot be reached directly from outside except using VPN.

-  Nginx is used as HTTPS proxy in front of all HTTP-based services.

-  *haproxy* is used to make non-HTTP services available from public.


VPN Access
==========

-  VPN access to the internal network is provided via OpenVPN.
   You need to install an OpenVPN client and an OpenVPN configuration file
   you receive from us.

-  VPN clients receive an address from the 10.187.0.0/16 range. This means that
   once the VPN is established there will be a connection on your local machine
   with - for example - the IP 10.187.0.10.

-  Infrastructure IP range is 10.186.0.0/16. The route is pushed via VPN.

   | 10.186.0.x - network infrastructure
   | 10.186.1.x - physical servers
   | 10.186.2.x - VMs

-  *You can test connectivity* by pinging 10.186.0.1.

-  Host names often resolve to internal IP addresses (srvXXX.typo3.org -> 10.186.2.XXX).
   You can use these hostnames once connected via VPN.


SSH Accounts
============

User accounts are managed by the TYPO3 Server Team.
They are provisioned via Chef cookbooka.
Public SSH keys will be automatically deployed.
Attention: Do not modify :file:`~/.ssh/authorized_keys` - it will be overwritten.
If you have a new SSH key, please contact the `TYPO3 Server Team <admin@typo3.org>`.


DNS Lad Balancing
=================

Frontend servers work redundantly and use DNS load balancing.
All frontend servers (usually 2) run the HTTPS proxy as well as *haproxy*.


HTTPS Proxy
===========

All applications are running behind Nginx-based HTTPS proxies.
There is no need for your application to set up HTTP.
HTTPS is mandatory. All HTTP requests will be automatically redirected.
The proxy adds some headers (Strict-Transport-Security and friends) automatically.
Please don't duplicate them in your application.

*Attention:* Verify the correct setup:

-  Refer to https://securityheaders.io/

-  Refer to https://www.ssllabs.com/

-  See the `"add_header" section <https://github.com/TYPO3-cookbooks/site-proxytypo3org/search?utf8=%E2%9C%93&q=add_header`__
   in the `Chef cookbook <https://github.com/TYPO3-cookbooks/site-proxytypo3org>`__

-  Make sure that your application logs the correct end-user's IP address.
   Respect the usual proxy headers (see the
   `"proxy_set_header" part <https://github.com/TYPO3-cookbooks/site-proxytypo3org/search?utf8=%E2%9C%93&q=proxy_set_header>`__
   of the `Chef cookbook <https://github.com/TYPO3-cookbooks/site-proxytypo3org>`__).



Public Access to non-HTTP Ports
===============================

TCP services that are not based on HTTP (for example Git) can be forwarded too.
