# CVE-2024-1086

Proof-of-concept exploit for CVE-2024-1086, working on most Linux kernels between (including) v5.14 and (including) v6.6, including (but not limited to) Debian, Ubuntu, and KernelCTF. The success rate is typically around 99,4% (n=1000) to 93% (n=1000). 

The only requirements are that user namespaces are enabled (kconfig `CONFIG_USER_NS=y`), those user namespaces are unprivileged (sh command `sysctl kernel.unprivileged_userns_clone` = 1), and nf_tables is enabled (kconfig `CONFIG_NF_TABLES=y`). By default, these are all enabled on Debian, Ubuntu, and KernelCTF. Other distro's have not been tested, but may work as well.

**Note (details in blogpost):**
- the exploit may be unstable on systems with a WiFi adapter, surrounded by high-usage WiFi networks. When testing, please turn off WiFi adapters through BIOS.
- the exploit does not work on Ubuntu with 6.5 kernels
- the affected versions lower limit is caused by the exploit, but the underlying vulnerability has been in the kernel since v3.15, so if you're below v5.15 make sure you upgrade in case makes an N-day for your specific version.

## details

A full write-up of the exploit can be found in the blogpost: ["Flipping Pages: An analysis of a new Linux vulnerability in nf_tables and hardened exploitation techniques"](https://pwning.tech/nftables/) on my blogsite.

## patch

For the fix/mitigation, check the [CVE-2024-1086 description](https://nvd.nist.gov/vuln/detail/CVE-2024-1086).

## disclaimer

The programs and scripts ("programs") in this software directory/folder/repository ("repository") are published, developed and distributed for educational/research purposes only. I ("the creator") do not condone any malicious or illegal usage of the programs in this repository, as the intend is sharing research and not doing illegal activities with it. I am not legally responsible for anything you do with the programs in this repository.
