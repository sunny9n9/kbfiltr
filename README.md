## kbfiltr – PS/2 Keyboard Input Filter Driver (KMDF)

This project is a Windows kernel-mode keyboard input filter driver based on the KMDF kbfiltr sample.
It attaches between kbdclass.sys and i8042prt.sys and intercepts keyboard input at the PS/2 (i8042) path.

# Current Behavior
Hooks IOCTL_INTERNAL_KEYBOARD_CONNECT to intercept KEYBOARD_INPUT_DATA
Can drop, mutate, and inject keystrokes in real time
Demonstrates safe scan-code transformation and synthetic key injection
Uses a bounded, non-paged buffer for output packets
Operates per device instance (per keyboard stack)

# Scope and Limitations
PS/2 / i8042 keyboards only in other words, targets integrated laptop keyboards
USB / HID keyboards are not handled (for now)
Note : VM environments expose a single virtual PS/2 keyboard, so input source differentiation cannot be tested there

# Intended Purpose
Educational and experimental kernel driver
Demonstrates low-level keyboard filtering and modification
In old laptops, keys usually get stuck needing keyboard replacement or disconnection. This could map keys at runtime though keymappers exist.

# Warning 
Tested in VM, for short instances, verified logic and stability, not guranteed real world use reliability.

# Installation
The project must be built in Visual Studio 2022 (with WDK installed). A successful build produces a .sys driver file and a corresponding .inf file, which Windows uses to load the driver.

Before installing, Secure Boot must be disabled in BIOS/UEFI. If Secure Boot is left on, Windows will refuse to load the driver and keyboard input may stop working. If that happens, use on-screen keyboard to log in, open device manager and update keyboard driver to load default windows drivers back. Otherwise simply boot into recovery or safe mode and restore the default Windows keyboard driver. 

Installation is done through Device Manager, not by double-clicking files. The driver should be manually applied to the Standard PS/2 Keyboard device using the generated .inf file (“Update driver” → “Browse my computer” → “Have Disk”).

Windows will warn about unsigned or test drivers.
