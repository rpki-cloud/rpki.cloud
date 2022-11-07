# rpki.cloud
RPKI Cloud is a free public RPKI Validator Service

<img align="right" src="https://defensor.cloud/wp-content/uploads/2022/10/defensor-horizontal-blue-transparent.svg" height="150">

[![Slack](https://img.shields.io/badge/Slack-4A154B?style=for-the-badge&logo=slack&logoColor=white)](https://join.slack.com/t/defensorhq/shared_invite/zt-1ixt72cyv-voTkoGCbwaFpSNWfJwlPqQ)
[![Twitter](https://img.shields.io/twitter/follow/DefensorRPKI.svg?label=Follow&style=social)](https://twitter.com/DefensorRPKI)

RPKI Cloud is a free validator service for RPKI Route Origin Validation (ROV). The service is designed to be highly available with performance and security in mind.

### RPKI ROV made easy
Enabling RPKI ROV for your ASN can be categorized into two steps. (1) Reading: Use of ROV on peering routers to validate BGP routes received from external peers. (2) Writing: Generating and publishing ROA (Route Origin Authorization) digital certificates for associating your ASNs to the IP prefixes owned by your organization. This project focuses on step 1. Typically, organizations would be required to setup multiple validation (relying party) servers to retreive ROAs from various repoistories on the Internet and perform the cryptographic validation which then deliver the VRP (validated ROA payload) to the edge routers via RTR.

Similar to a public DNS resolution service like Google's 8.8.8.8 or Cloudflare's 1.1.1.1, the RPKI Cloud project is a free, cloud based service.  It does the work of retrieving ROAs for repositories around the world and performing cryptographic validation.  The service provides an open, highly available RTR feed on port 3323 which your edge routers can subscribe to. 

### Free
Let's face it. Setting up and maintaining highly available ROV servers is hard which contributes to the limited adoption of ROV. RPKI ROV has been designed to make the Internet a more secure place by verifying that an IP prefix is being announced by the correct ASN (trust but verify!). The RPKI Cloud service aims to make ROV adoption simpler by providing a highly available and scalable feed and example ROV configuration required on your routers.

### Configuration
As a network operator, all you need to do is setup your external eBGP routers to grab the RPKI Cloud feed. The .config files in this repo provide examples of the ROV configuration for various types of commonly used edge routers. Find yours and configure accordingly.

The free RPKI validation feed is available at `r1.rpki.cloud`(IPv4 184.73.232.63, IPv6 2600:1f18:e99:5401:e875:17cd:7a52:5d6c) and `r2.rpki.cloud` (IPv4 52.52.161.24, IPv6 2600:1f1c:244:6000:e347:c4c8:56a2:d665) on port 3323

Example for Mikrotik:
```
/routing/bgp/rpki
add group=myRpkiGroup address=184.73.232.63 port=3323 refresh-interval=20
add group=myRpkiGroup address=52.52.161.24 port=3323 refresh-interval=20

/routing/filter/rule
add chain=bgp_in rule="rpki-verify myRpkiGroup"
add chain=bgp_in rule="if (rpki invalid) { reject } else { accept }"
```

ROV enables you to accept 'valid' and 'unknown' advertisements and reject 'invalid' advertisements. For your initial testing, you may decide to not 'reject' any routes and simply log and/or tag the 'invalid' routes with a community. A route is considered as 'invalid' if the origin ASN for a that particular IP prefix does not match per the available ROAs.  You can read more about RPKI here: https://rpki.readthedocs.io/

### Support

Community support is available on
[Slack](https://join.slack.com/t/defensorhq/shared_invite/zt-1ixt72cyv-voTkoGCbwaFpSNWfJwlPqQ),
[Twitter](https://twitter.com/DefensorRPKI). RPKI Cloud is
liberally licensed under the [Apache License](https://github.com/rpki-cloud/rpki.cloud/blob/main/LICENSE)

#### Contribute
Like the project? Please share this in your network (pun intended) and encourage more ASNs to perform ROV at their edge.

#### Sponsor a coffee
Contribute to make the internet a more secure place by sponsoring us a coffee below

https://www.buymeacoffee.com/rpkicloud

