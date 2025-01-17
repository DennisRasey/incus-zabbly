# Incus builds

Incus package builds provided by Zabbly.

There are three repositories available:

* `lts-6.0` (Incus 6.0.x LTS)
* `stable` (latest release of Incus)
* `daily` (untested daily builds)

## Availability

Those packages are built for:

* Ubuntu 20.04 LTS (`focal`)
* Ubuntu 22.04 LTS (`jammy`)
* Ubuntu 24.04 LTS (`noble`)
* Debian 11 (`bullseye`) (`amd64` only)
* Debian 12 (`bookworm`)

Unless otherwise mentioned, packages are built for both `amd64` (Intel/AMD 64bit) and `arm64` (Arm 64bit).

NOTE: It is often possible to use those packages on other non-LTS Ubuntu releases by picking the closest LTS release prior to the Ubuntu version being run.

## Installation

All commands should be run as root.

### Repository key

Packages provided by the repository are signed. In order to verify the integrity of the packages, you need to import the public key. First, verify that the fingerprint of [`key.asc`](https://pkgs.zabbly.com/key.asc) matches `4EFC 5906 96CB 15B8 7C73  A3AD 82CC 8797 C838 DCFD`:

```sh
curl -fsSL https://pkgs.zabbly.com/key.asc | gpg --show-keys --fingerprint
```

or if your system has wget instead of curl use

```sh
wget -q -O - https://pkgs.zabbly.com/key.asc | gpg --show-keys --fingerprint
```

You should get a return that is:

```sh
pub   rsa3072 2023-08-23 [SC] [expires: 2025-08-22]
      4EFC 5906 96CB 15B8 7C73  A3AD 82CC 8797 C838 DCFD
uid                      Zabbly Kernel Builds <info@zabbly.com>
sub   rsa3072 2023-08-23 [E] [expires: 2025-08-22]
```

If so, make sure the directory /etc/apt/keyrings exists:

```sh
mkdir -p /etc/apt/keyrings/
```

and save the key locally with either curl:

```sh
curl -fsSL https://pkgs.zabbly.com/key.asc -o /etc/apt/keyrings/zabbly.asc
```

or wget:

```sh
wget -O /etc/apt/keyrings/zabbly.asc https://pkgs.zabbly.com/key.asc
```


### 6.0 LTS repository

On any of the distributions above, you can add the package repository at `/etc/apt/sources.list.d/zabbly-incus-lts-6.0.sources`.

Run the following command to add the 6.0 LTS repository:

```sh
sh -c 'cat <<EOF > /etc/apt/sources.list.d/zabbly-incus-lts-6.0.sources
Enabled: yes
Types: deb
URIs: https://pkgs.zabbly.com/incus/lts-6.0
Suites: $(. /etc/os-release && echo ${VERSION_CODENAME})
Components: main
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/zabbly.asc

EOF'
```

### Stable repository

On any of the distributions above, you can add the package repository at `/etc/apt/sources.list.d/zabbly-incus-stable.sources`.

Run the following command to add the stable repository:

```sh
sh -c 'cat <<EOF > /etc/apt/sources.list.d/zabbly-incus-stable.sources
Enabled: yes
Types: deb
URIs: https://pkgs.zabbly.com/incus/stable
Suites: $(. /etc/os-release && echo ${VERSION_CODENAME})
Components: main
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/zabbly.asc

EOF'
```

### Daily repository

On any of the distributions above, you can add the package repository at `/etc/apt/sources.list.d/zabbly-incus-daily.sources`.

Run the following command to add the daily repository:

```sh
sh -c 'cat <<EOF > /etc/apt/sources.list.d/zabbly-incus-daily.sources
Enabled: yes
Types: deb
URIs: https://pkgs.zabbly.com/incus/daily
Suites: $(. /etc/os-release && echo ${VERSION_CODENAME})
Components: main
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/zabbly.asc

EOF'
```

### Installing Incus

Update your repository list with:

```sh
apt-get update
```

Then to install Incus, run:

```sh
apt-get install incus
```

### Other packages

The repository also includes the following packages:

 - `incus-client`, a package containing only the CLI tool, useful when only managing remote servers
 - `incus-ui-canonical`, a package containing a rebranded version of the LXD web interface for use with Incus

### Setting up the UI

When using `incus-ui-canonical`, you will need to have Incus listen on the network.
This is done either by enabling it during `incus admin init` or by setting a listener through `incus config set`.

For example:
```
incus config set core.https_address :8443
```

After that, you can access the UI through https://localhost:8443, accept the self-signed certificate and follow the login instructions.

As `incus-ui-canonical` is a re-branded version of the LXD UI, you can find useful information in [their documentation](https://documentation.ubuntu.com/lxd/en/latest/howto/access_ui/).

## Support
Community support for Incus is provided at https://discuss.linuxcontainers.org

Commercial support for those Incus packages is provided by Zabbly, details at https://zabbly.com/incus

You can also help support the work on Incus and on those packages through:

 - [Github Sponsors](https://github.com/sponsors/stgraber)
 - [Patreon](https://patreon.com/stgraber)
 - [Ko-Fi](https://ko-fi.com/stgraber)

## Repository

This repository gets actively rebased as new releases come out, DO NOT expect a linear git history.
