# Configuration

See: `man gflow` when gflow is installed or [file/doc/README.md](file/doc/README.md)

## Global section

```
[global]
ip = /bin/ip
fping = /usr/bin/fping
multipath = no
cache_file = /var/tmp/gflow.json
configured = yes
```

| Option       | Description                                                                                                                               |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------|
| ip           | path of the `ip` command which is used to manage routing table. Default: ip.                                                              |
| fping        | path of the `fping`command which is used to check IP addresses. Default: fping.                                                           |
| multipath    | whether to use the first (smallest id) or all available gateway(s) as default one(s). Default: no.                                        |
| cache_file   | file path where gflow is storing current states of configured gateways to be able to detect flips. Default: none.                         |
| configured   | Let this variable to `no` until the configuration file is properly setup, preventing doing bad things to the default route. Default: no.  |

## Gateways configuration

```
[gateway:XXX]
address = 192.168.0.1
remotes = 80.67.169.12 80.67.169.40
score = 2
weight = 2
;up = echo "gateway ${GFLOW_GATEWAY} is up"
;down = echo "gateway ${GFLOW_GATEWAY} is down"
;change = echo "gateway ${GFLOW_GATEWAY} event : ${GFLOW_EVENT}"
```

| Option    | Description                                                                                                                 |
| --------- | ----------------------------------------------------------------------------------------------------------------------------|
| XXX       | id (integer) of the gateway. Unique identifier to store/retrieve gateway state to/from cache file.                          |
| address   | The address of the gateway if available. Let this field empty in case of PtP connection. Default: empty.                    |
| iface     | Interface of PtP connection. Address of gateway will be guessed from routing table.                                         |
| reachable | Set to `no` if the gateway is not *pingable*, test is then skipped and successful. Default: yes.                            |
| remotes   | One or more (space separated) IP address(es) which is(are) usually *pingable* **via** this gateway. Default: empty.         |
| score     | Number of tests to pass in order to consider gateway as alive. Default: empty, all tests must be passed.                    |
| weight    | Weight of the route if `multipath` option is enabled. Refer to `ip-route` manual for details.                               |
| up        | A script to run when the gateway is going up.                                                                               |
| down      | A script to run when the gateway is going down.                                                                             |
| change    | A script to run when the gateway state is changing.                                                                         |

*Notes:*
* For each remote, a route is added **via the gateway** before sending an ICMP request to the remote. This route is then removed.
* The following variables are available in script's environment:
    * GFLOW_GATEWAY
    * GFLOW_REMOTES
    * GFLOW_EVENT
    * GFLOW_IFACE
    * GFLOW_WEIGHT
* Don't setup the default route in scripts, this is already done by gflow.
