# vlmcsd package for OpenWrt

## How to build

Everything should be done inside the OpenWrt build root.

Add this git repository into `feeds.conf` after default feeds from
`feeds.conf.default`:

```bash
cp feeds.conf.default feeds.conf
echo "src-git vlmcsd https://github.com/im-0/openwrt-package.vlmcsd.git" >>feeds.conf
```

Update and install feeds:

```bash
./scripts/feeds update -a
./scripts/feeds install -a
```

Enable package:

```bash
make menuconfig
# Network -> vlmcsd = M
```

Then build everything as usual:

```bash
make -j$(nproc) V=s world
```

And find the package:

```bash
ls -lah ./bin/packages/*/vlmcsd/vlmcsd_*.ipk
```

## How to install and configure

```bash
opkg install ./vlmcsd_*.ipk
```

`vlmcsd` itself does not require any additional configuration and is up and
running right after the installation.

You may configure `dnsmasq` so that Windows will find the activation server
automatically. To achieve that, add following into `/etc/config/dhcp`:

```text
config dnsmasq
        # ...
        option local '/YOURDOMAIN.LAN/'
        option domain 'YOURDOMAIN.LAN'
        # ...

# ...
config srvhost
        option srv '_vlmcs._tcp.YOURDOMAIN.LAN.'
        option target '192.168.123.45'
        option port '1688'
        option class '100'
        option weight '100'
```

Where `YOURDOMAIN.LAN` is your preferred domain name (choose any) and
`192.168.123.45` is the local IP address of your router where `vlmcsd` is
installed.

Restart `dnsmasq`:

```bash
/etc/init.d/dnsmasq restart
```

And make sure that Windows machine uses DHCP for network configuration.
