# ADS-B Quick Start Installation Guide
_last edited: 2023/11/22_

This guide will give you a quick and easy to setup system to demodulate and decode ADS-B messages using this Rust software!
For a good technical intro into what ADS-B is and how it is decoded, see the following [guide](https://mode-s.org/decode/).

# Hardware

## SDR (Software Defined Radio)
If your focus is on ease of use, as well as value(~$25) you should use an [rtlsdr](https://www.rtl-sdr.com/about-rtl-sdr/) Software Defined Radio.
A simple one with a filter and amplifier can be bought from [adsbexchange](https://store.adsbexchange.com/products/adsbexchange-com-r820t2-rtl2832u-0-5-ppm-tcxo-ads-b-sdr-w-amp-and-1090-mhz-filter-software-on-industrial-microsd).

![adsb-exchange rtlsdr](https://cdn.shopify.com/s/files/1/1618/5293/products/BlueSDRantennaandmicroSD_1024x1024@2x.png?v=1643391183)

## Antenna
I bought the [adsb-exchange antenna](https://store.adsbexchange.com/products/5-5dbi-1090-978-antenna), and it serves me well. Make sure you don't have objects obstructing the view
from the sky and you should be fine. If you want a super cheap solution, I have seen people talk about a can-tenna.

![adsb-exchange antenna](https://cdn.shopify.com/s/files/1/1618/5293/products/IMG_9964_1_1024x1024@2x.jpg?v=1570168212)

## Connections

You will need a N-Type Male SMA to SMA Male connection cable. You can also find these on [store.adsbexchange.com](https://store.adsbexchange.com/collections/all). This will allow you to connect the antenna directly to the `rltsdr`.
The `rtlsdr` should be plugged into your computer's USB port.

# Software

### [dump1090_rs](https://github.com/rsadsb/dump1090_rs)
`dump1090_rs` performs the demodulating of the 1090Mhz signal from an SDR (Software Defined Radio),
as well as forwarding the bytes to our application.

### [radar](https://github.com/rsadsb/adsb_deku)
`radar` parses the ADS-B messages into data that can be displayed on a map.

![image](https://github.com/rsadsb/adsb_deku/raw/master/media/peek_2022_08_19.gif)

## Linux dependencies
You will need to install the `soapysdr` library, plus the `soapysdr` driver for your SDR(rtlsdr).
Searching for `soapysdr-rtlsdr` and `soapysdr` should lead you on the right path.
The `soapysdr` library can be foud as `libsoapysdr-dev` on ubuntu based systems.

## From Releases (`x86_64`)
Binaries can be downloaded from the latest releases of our software on github.
The following is an example for `x86_64`.
```
# download dump1090_rs
> wget https://github.com/rsadsb/dump1090_rs/releases/download/v0.7.1/dump1090_rs-v0.7.1-aarch64-unknown-linux-gnu.tar.gz
> tar -xf dump1090_rs-v0.7.1-aarch64-unknown-linux-gnu.tar.gz

# download radar
> wget https://github.com/rsadsb/adsb_deku/releases/download/v2023.11.22/radar-x86_64-unknown-linux-gnu.tar.gz
> tar -xf radar-x86_64-unknown-linux-gnu.tar.gz
```

`ARM` releases are also available:
- [dump1090_rs](https://github.com/rsadsb/dump1090_rs/releases/tag/v0.7.1)
- [radar](https://github.com/rsadsb/adsb_deku/releases/tag/v2023.11.22)


## (Optional) Install from source for max performance

<details>
  <summary>Instructions</summary>

#### Native CPU features
Building from source is recommended, since many of the DSP(Digital Signal Processing) algorithms benefit from
cpu features such as AVX(Advanced Vector Extensions) on x86 architectures.
These are currently only detected at compile time and is the reason for `RUSTFLAGS="-C target-cpu=native"` in the following install commands.

#### Install Compiler
Goto [rustup.rs](https://rustup.rs) and follow instructions for installing `rust` and `cargo`.
The current msrv(minimum supported rust version) is `1.64.0`.

### Install soapysdr library
You will need the soapysdr library. For example, this is `libsoapysdr-dev` on ubuntu based platforms.

### Clang
Install `libclang`, found as `libclang-dev` on ubuntu.

### [dump1090_rs](https://github.com/rsadsb/dump1090_rs) from master branch
```shell
RUSTFLAGS="-C target-cpu=native" cargo install --git https://github.com/rsadsb/dump1090_rs
```

### [radar](https://github.com/rsadsb/adsb_deku) from master branch

```shell
RUSTFLAGS="-C target-cpu=native" cargo install --git https://github.com/rsadsb/adsb_deku rsadsb_apps --bin radar
```
</details>

## Running!
Here is a sample usage of the software. Both applications have `--help` output for more options.
The `--lat` and `--long` will be the location of your antenna, and the `--locations` will print on the radar screen.
See [Raspberry Pi in-car ADS-B Display - with Rust!](rasp-pi-display.md) for a blog on using this for an in-car ADS-B display.
```text
# run demodulation with default settings for rtlsdr
> dump1090_rs

# run radar
> radar --disable-lat-long --lat="50.0" ---long="50.0" --locations "(one, 100.0,-50.0)" "(two, 50.0, -100.0)"
```

## Questions/Help
* Open up a discussion on [rsadsb/adsb_deku](https://github.com/rsadsb/adsb_deku/discussions) for `radar` help.
* Open up a discussion on [rsadsb/dump1090_rs](https://github.com/rsadsb/dump1090_rs/discussions) for `dump1090_rs` help.
* You can also email me @ `wcampbell1995[dot]gmail[dot]com`.
