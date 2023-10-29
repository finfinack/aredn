# AREDN-Packages

This is a repo to hold AREDN packages (OpenWrt) as a separate package feed. These include primarily Makefiles for custom packages.

## Building

In order to be able to compile/use these definitions, familiarize yourself with the OpenWrt build environment. Roughly in order:

- [Build system essentials](https://openwrt.org/docs/guide-developer/toolchain/buildsystem_essentials)
- [Build system setup](https://openwrt.org/docs/guide-developer/toolchain/install-buildsystem)
- [Build system usage](https://openwrt.org/docs/guide-developer/toolchain/use-buildsystem)
- [Building a single package](https://openwrt.org/docs/guide-developer/toolchain/single.package)

### Prepare config

This repository also contains suggested starting points for configs to compile the package with. See the `configs` folder. In order to use them, copy the `<architecture>.config` file into your build root directory as `.config`.

### Prepare feeds

Once you have a working build setup, you can include the AREDN packages as a feed:

Note: If feeds.conf already exists, you can ignore this step.

```
cd <your openwrt build/src root>
cp feeds.conf.default feeds.conf
```

Add the following in `feeds.conf`:
```
src-git aredn https://github.com/arednch/packages.git
# If you prefer a local copy instead:
# src-link aredn <folder where you have the aredn package definitions (this repo)>
```

Announce the new feed to the build system:
```
cd <your openwrt build/src root>
./scripts/feeds update aredn
./scripts/feeds install -a -p aredn
```

Note: When a new package is added, remember to rerun `./scripts/feeds update aredn` in order to make the build system aware of it.

Once this is set up, you should start to see packages from this feed show up in the available package list when running `make menuconfig`.

Specifically to set up a custom feed, find some more guidance here:
https://openwrt.org/docs/guide-developer/helloworld/chapter4

## Releases

Releases are created for the packages defined / built in this repo: https://github.com/arednch/packages/releases

The following always points to the latest release: https://github.com/arednch/packages/releases/latest

The stable download links for the latest release for each architecture:

- [ARM Cortex A7, ipq40xx (e.g. hAP AC3)](https://github.com/arednch/packages/releases/latest/download/arm_cortex-a7_neon-vfpv4.zip)
- [MIPS 24kc, ath79 (e.g. hAP AC Lite)](https://github.com/arednch/packages/releases/latest/download/mips_24kc.zip)
- [x86_64](https://github.com/arednch/packages/releases/latest/download/x86_64.zip)

### Compatibility

The releases are usually tested with the latest (stable) version of [AREDN firmware](https://www.arednmesh.org/content/current-software). Notably, the software packages are currently built and tested in particular for the following:

- MikroTik hAP AC3 (`ipq40xx`, `arm_cortex-a7_neon-vfpv4`)
- MikroTik hAP AC Lite (`ath79`, `mips_24kc`)

Note: While we believe these packages _should_ run on most other AREDN firmware versions and likely for many other platforms using the same chipset, we are _not_ testing these explicitly.