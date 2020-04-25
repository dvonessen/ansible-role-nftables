# ansible-role-nftables

This role configures the Linux netfilter through nftables commandline interface.

## Description

With the configuration of netfilter with nftables you will have a good tool to
secure your servers on the network. This role will disable and remove firewalld
and ufw. Because they are not compatible with netfilter.

In the default configuration, this role will create one `table` with three
`chains`: `input`, `output`, `forward`. For each of those `chains you can
configure `rulesets`.

### Input chain

The `input chain` by default:

- drops any non-matching packets
- configures connection tracking by accepting all packets which a established
  or related
- accepts traffic to interace lo (localhost)

### Output chain

The `output chain` by default:

- accepts all packets

### Forward chain

The `forward chain` by default:

- drops all packes

## Role Variables


| Name                           |             Default             | Description                                                                                      |
| :----------------------------- | :-----------------------------: | ------------------------------------------------------------------------------------------------ |
| nftables_default_configuration |             `true`              | If true configures the input chain as descript, connection tracking and accept on `lo` interface |
| nftables_config_input_policy   |             `drop`              | Default policy for the input chain.                                                              |
| nftables_config_input_rules    | [`tcp dport 22 counter accept`] | Rule which allows by default *SSH* from any address.                                             |
| nftables_config_forward_policy |             `drop`              | Default policy for the forward chain.                                                            |
| nftables_config_forward_rules  |               []                | Empty ruleset for forward chain.                                                                 |
| nftables_config_output_policy  |            `accept`             | Default policy for the output chain.                                                             |
| nftables_config_output_rules   |               []                | Empty ruleset for output chain.                                                                  |

## Role Tags

| Name                               | Description                                                 |
| ---------------------------------- | ----------------------------------------------------------- |
| nftables_all                       | Tag to run all tasks.                                       |
| nftables_install                   | Tag which installs the nftables package                     |
| nftables_configure                 | Tag which triggers the configuration steps of this role.    |
| nftables_disable_foreign_firewalls | Tag to disable foreign firewalls like *firewalld* or *ufw*. |

## Dependencies

**None**

## Example Playbook


```yaml
- name: All
  hosts: all
  debugger: on_failed
  roles:
    - role: ansible-role-nftables
      vars:
        nftables_config_input_rules:
          - tcp dport 22 counter accept
          - ip saddr 1.1.1.1 dport 80,443 counter accept
```

## License

MIT License

## Contributors

Daniel von Essen
