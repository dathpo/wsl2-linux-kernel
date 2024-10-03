# Readme (fork)

The default WSL2 Linux kernel comes without Bluetooth support. The aim of this fork is to provide a kernel image with Bluetooth support.

To build a custom kernel image:

1. Open WSL, run `uname -r` and note down the kernel version
2. Within the WSL kernel git repository, check out the branch that matches the version. The branches start with `linux-msft-wsl-`
3. Cherry-pick and push the commit that adds a CI pipeline YAML file and enables Bluetooth options
4. Once the CI pipeline is complete, download the job artifact, which contains the kernel image, `bzImage`
5. Copy `bzImage` to `C:\bzImage`
6. Create a file, `C:\Users\<USER>\.wslconfig` and write the following to it:

```text
[wsl2]
kernel=C:\\bzImage
```

7. In Windows, disable Bluetooth to allow the adapter to be attached to WSL
8. In WSL, install the dependencies to use Bluetooth: `sudo apt install dbus rfkill bluez`
9. From PowerShell, run `wsl --shutdown` to force loading new kernel
10. Run `usbipd list` to find the Bluetooth adapter
11. Run `usbipd attach -b <BUS_ID> --wsl -a` on the Bluetooth adapter to attach it to WSL

# Introduction

The [WSL2-Linux-Kernel][wsl2-kernel] repo contains the kernel source code and
configuration files for the [WSL2][about-wsl2] kernel.

# Reporting Bugs

If you discover an issue relating to WSL or the WSL2 kernel, please report it on
the [WSL GitHub project][wsl-issue]. It is not possible to report issues on the
[WSL2-Linux-Kernel][wsl2-kernel] project.

If you're able to determine that the bug is present in the upstream Linux
kernel, you may want to work directly with the upstream developers. Please note
that there are separate processes for reporting a [normal bug][normal-bug] and
a [security bug][security-bug].

# Feature Requests

Is there a missing feature that you'd like to see? Please request it on the
[WSL GitHub project][wsl-issue].

If you're able and interested in contributing kernel code for your feature
request, we encourage you to [submit the change upstream][submit-patch].

# Build Instructions

Instructions for building an x86_64 WSL2 kernel with an Ubuntu distribution are
as follows:

1. Install the build dependencies:  
   `$ sudo apt install build-essential flex bison dwarves libssl-dev libelf-dev`
2. Build the kernel using the WSL2 kernel configuration:  
   `$ make KCONFIG_CONFIG=Microsoft/config-wsl`

# Install Instructions

Please see the documentation on the [.wslconfig configuration
file][install-inst] for information on using a custom built kernel.

[wsl2-kernel]:  https://github.com/microsoft/WSL2-Linux-Kernel
[about-wsl2]:   https://docs.microsoft.com/en-us/windows/wsl/about#what-is-wsl-2
[wsl-issue]:    https://github.com/microsoft/WSL/issues/new/choose
[normal-bug]:   https://www.kernel.org/doc/html/latest/admin-guide/bug-hunting.html#reporting-the-bug
[security-bug]: https://www.kernel.org/doc/html/latest/admin-guide/security-bugs.html
[submit-patch]: https://www.kernel.org/doc/html/latest/process/submitting-patches.html
[install-inst]: https://docs.microsoft.com/en-us/windows/wsl/wsl-config#configure-global-options-with-wslconfig
