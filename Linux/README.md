# INDEX

- [INDEX](#index)
- [Why do Linux and Windows show different time in a Dual Boot?](#why-do-linux-and-windows-show-different-time-in-a-dual-boot)

# Why do Linux and Windows show different time in a Dual Boot?

A computer has two main clocks: a **system** clock and a **hardware** clock.

A hardware clock which is also called RTC (real time clock) or CMOS/BIOS clock. This clock is outside the operating system, on your computer’s motherboard. It keeps on running even after your system is powered off.

The system clock is what you see inside your operating system.

When your computer is powered on, the hardware clock is read and used to set the system clock. Afterwards, the system clock is used for tracking time. If your operating system makes any changes to system clock, like changing time zone etc, it tries to sync this information to the hardware clock.

By default, Linux assumes that the time stored in the hardware clock is in UTC, not the local time. On the other hand, Windows thinks that the time stored on the hardware clock is local time. That’s where the trouble starts.

Refer [this](./linux-windows-time-diff.sh) executable shell script for resolving this issue.
