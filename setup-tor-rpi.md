# Setup Tor and Snowflake on a Raspberry Pi

arm64 is the red headed step child of the Tor ecosystem - supported but not loved.

## Setup Tor

```bash
sudo apt install tor obfs4proxy
```

## Clone and build Snowflake manually for ARM

```bash
git clone https://gitlab.torproject.org/tpo/anti-censorship/pluggable-transports/snowflake.git
cd snowflake/proxy
go build
sudo cp proxy /usr/local/bin/snowflake-client
```

## Edit `~/.tor/torrc`

```bash
UseBridges 1
ClientTransportPlugin snowflake exec /usr/loca/bin/snowflake-client
Bridge snowflake 192.0.2.3:1
```

then

```bash
sudo systemctl restart tor
```

## Configure Firefox

Tor Browser doesn't support ARM. But it's trivial to setup Firefox.

1. Open `about:preferences`
2. Scroll to `Network Settings`, click `Settings...`
3. Set proxy to `SOCKS5 localhost 9050`
4. Check `Proxy DNS when using SOCKS5`

Check that you're properly configured at [check.torproject.org](https://check.torproject.org)
