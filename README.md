# RMK for ID75

[RMK](https://rmk.rs/) firmware port for the ID75 keyboard. The firmware is built with the [Vial](https://get.vial.today/) support. RGB lightning doesn't work.

## Hardware Overview

System-on-chip: RP2040  
Flash memory: Winbond W25Q16BV (2MB)  

![keyboard_hw](https://github.com/user-attachments/assets/ee8fb935-0c0b-430f-b24c-f3158e7c3134)

## Flashing

1. Unplug the USB cable.
2. Press and hold the `BOOTSEL` button.
3. Connect the USB cable.
4. Release the `BOOTSEL` button.
5. A new thumb driver will appear on the PC.
6. Copy the `id75.uf2` file from the [Releases](https://github.com/akazantsev/id75/releases) onto it.

## Notes

At the moment, QMK does not work reliably on this PCB. It will initialize correctly only once in 10 attempts.
It appears there is a bug in ChibiOS, which QMK uses under the hood, related to the RP2040 interrupts,
that prevents QMK from starting up correctly.

## Development

Install Rust with [rustup](https://rustup.rs/).

Download the toolchain for RP2040:

```bash
rustup target add thumbv6m-none-eabi
```

Install the dependencies:

```bash
cargo install flip-link cargo-make
cargo install probe-rs-tools --locked
```

Build an ELF executable:

```bash
cargo build --release
```

Convert ELF into uf2:

```bash
cargo make uf2 --release
```

It will create the `id75.uf2` file in the project's directory that you can flash onto the keyboard.

### Raspberry Pi Debug Probe

You can also build, flash, and run the firmware in one step with a debug probe.

Install the udev rules for the debug probe on Linux as specified [here](https://probe.rs/docs/getting-started/probe-setup/).
I didn't have to add the `plugdev` user group on Fedora. If everything is set up and connected correctly, you should
be able to build, flash and run the firmware with this command:

```bash
cargo run --release
```
