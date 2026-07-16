# Investigation Indicators

## Network Indicators

| Type | Value | Description |
|---|---|---|
| Infected IP | `10.2.28.88` | Internal Windows computer |
| Suspicious IP | `45.131.214.85` | External server from the SIEM alert |
| Suspicious URL | `http://45.131.214.85/fakeurl.htm` | Destination of repeated HTTP POST requests |
| Port | `443/TCP` | Port used for the communication |
| Protocol | `HTTP` | Protocol found in the traffic |
| User-Agent | `NetSupport Manager/1.3` | Software name found in the HTTP request |

## Host Information

| Type | Value |
|---|---|
| MAC address | `00:19:d1:b2:4d:ad` |
| Hostname | `DESKTOP-TEYQ2NR` |
| User account | `brolf` |
| Full name | `Gabriel Wyatt` |
| AD domain | `EASYAS123` |

## Suspicious Traffic

The infected host sent repeated HTTP POST requests to:

```text
http://45.131.214.85/fakeurl.htm
```

The TCP stream included:

```text
CMD=POLL
CMD=END
```

These values may show command-and-control communication.

## Important Note

These indicators come from a public training exercise.

They should be used only for learning and portfolio demonstration.
