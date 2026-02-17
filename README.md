# luci-app-zerotier

LuCI interface for ZeroTier / ZeroTier’s LuCI management UI

*   A LuCI management interface used to join ZeroTier networks
*   Uses scripts to dynamically implement NAT functionality (multi-subnet interconnection), making it more flexible and convenient
*   Suitable for official OpenWrt and <https://github.com/coolsnowwolf/lede>

This project was copied from <https://github.com/coolsnowwolf/luci>

Because the original project <https://github.com/coolsnowwolf/luci/pull/230>, it was cloned out into this separate project.

## Changelog

### v2.2

*   Added `srcnat` configuration to enable subnet interconnection in bypass-router mode
*   Removed the function that restarts the ZeroTier service
*   Added tabs in the configuration page to separate General and Advanced settings

### v2.1

*   The configuration page now supports all available options

### v2.0

Starting from 2.0, this package only serves as an auxiliary for the ZeroTier package

*   You must use the official `packages/zerotier` to start the ZeroTier service
*   The helper script only provides enabling and disabling of NAT

### v1.1

*   Supports official OpenWrt 22.03+ fw4 nftables
*   Supports official OpenWrt Chinese localization `po/zh_Hans`
*   Supports compilation even when not placed inside the luci directory
*   When using the official `imagebuilder`, resolves the <https://github.com/coolsnowwolf/luci/pull/172>
*   Fixed some issues:
    *   `restart` / `reload` not working as expected
    *   When static routes exist, stopping the service fails to remove src nat rules

## Depends

*   zerotier
*   luci-compat (For official OpenWrt LuCI)

## Compile

```shell
# Enter the OpenWrt SDK directory, recommended to use Docker, for example:
docker run -it -v $PWD/bin:/builder/bin openwrt/sdk:x86-64-22.03.5 bash

# Update feeds
#   - Fetch feeds/luci/luci.mk
#   - Fetch build info for dependency zerotier (feeds/packages/net)
#   - Fetch feeds/luci/applications directory
./scripts/feeds update -a

# Copy to the appropriate directory, for example:
git clone --depth=1 https://github.com/zhengmz/luci-app-zerotier.git feeds/luci/applications/luci-app-zerotier

# Load
./scripts/feeds update -f luci
./scripts/feeds install -p luci -f luci-app-zerotier
make defconfig

# Compile
make package/luci-app-zerotier/compile

# Results
# Stored in bin/packages/x86_64/luci directory
luci-app-zerotier*.ipk
luci-i18n-zerotier-zh-cn*.ipk
```

## Usage

```shell
# It conflicts with the zerotier service; disabling is recommended. Two methods (Only for v1.1)

# 1. Use the disable command
/etc/init.d/zerotier disable

# 2. Add the parameter when customizing firmware
DISABLED_SERVICES="zerotier"
```

