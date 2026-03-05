# usteer-ng

usteer-ng is a client steering daemon written for OpenWrt and the a fork of the famous [usteer steering deamon](https://openwrt.org/packages/pkgdata/usteer).

Its goal is to optimize roaming/band steering behavior of wireless clients (STAs) in a ESS consisting of multiple BSS / APs.

## Functions

 - Synchronization of Neighbor Reports between multiple APs
 - Policy-based decisions for probe- / association- / authentication requests received from STAs
 - Requesting clients to roam to a different BSS based on SNR / signal-level
 - Channel-load based client steering to different BSS
 - Band steering to higher bands if possible
 - Using different level of aggressiveness

## Installation

usteer-ng is available as OpenWrt package and can be installed on devices running OpenWrt 21.02+ using opkg or 25.12+ using apk. Usteer-ng conflicts to Usteer as it actually based on the same code. Also it does not really make sense to have two Wifi controllers installed.

Download package matching your router architecture, place it to e.g. /root/usteer/ and use the following command to remove usteer and install usteer-ng with luci-app-usteer. You can find all [assets here](https://github.com/NilsRo/usteer-ng/releases).
```
opkg update;opkg remove --force-depends usteer;opkg install /root/usteer/*.ipk luci-app-usteer
```

You can find the complete documentation for setting everything up and getting it running here:[OpenWRT documentation](https://openwrt.org/docs/guide-user/network/wifi/usteer).

## Submitting patches

usteer-ng is fully managed in Github. Please contribute via Pull-Requests to the "development" branch.