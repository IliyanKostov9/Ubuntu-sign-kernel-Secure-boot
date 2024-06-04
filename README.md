# Quick guide

```bash
# checkout this repo and enter the sbin folder in the terminal
cd sbin
sudo bash mok-setup.sh

# Default the rest of the options to 'Yes', until you stop at this option
# In Choose the extendedKeyUsage extension, choose option 4
Choose the extendedKeyUsage extension (default: 4)
0: 1.3.6.1.4.1.2312.16      - Kernel OIDs
1: 1.3.6.1.4.1.2312.16.1    - X.509 extendedKeyUsage restriction set
2: 1.3.6.1.4.1.2312.16.1.1  - Firmware signing only        [Sign linux firmware]
3: 1.3.6.1.4.1.2312.16.1.2  - Module signing only          [Sign DKMS modules i.e. graphics drivers]
4: 1.3.6.1.4.1.2312.16.1.3  - Kexecable image signing only [Sign kernels]
Key Usage: 4

# Again choose the default option, until you see the message below:
# For this choose Y
Required for validating signed kernel images (default: N): Y

# Afterwards, let's sign our certificate we have jsut created to the kernel we want to use.
# In this example the kernel we'll use for demonstration is `vmlinuz-6.5.0-34-generic`. Please change it to your specific kernel version.
# Beforehand we can verify that the kernel isn't signed (actually it is by Canonical, but still we need to manually sign it with our own cert)
sudo sbverify --list /boot/vmlinuz-6.5.0-34-generic
# Next is the signing part:
sudo sbsign --key "/var/lib/shim-signed/mok/MOK-Kernel.priv" --cert "/var/lib/shim-signed/mok/MOK-Kernel.pem" --output "/boot/vmlinuz-6.5.0-34-generic" "/boot/vmlinuz-6.5.0-34-generic"
# Verify that the kernel is signed
sudo sbverify --list /boot/vmlinuz-6.5.0-34-generic
# You should now see your signature, along with the sign key information, that you provided earlier, for example (ST,L,C)
```

After that reboot your PC:

1. You'll see a blue screen for the MOK
2. Choose option 2 `Enroll MOK`
3. Choose **continue**
4. Choose **Yes**
5. Type the password
6. Now *reboot*
7. Remember to choose the kernel version in the GRUB menu
8. If you are able to boot with that kernel, then congrats!
9. You can additionally check if you are currently using the kernel by doing: `uname -r`

## Acknowledgment

This repo is a [fork]<https://github.com/berglh/ubuntu-sb-kernel-signing>, so please support the original authors! :)

- [berglh]<https://github.com/berglh>
