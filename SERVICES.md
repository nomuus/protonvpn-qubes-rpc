# Qubes ProtonVPN RPC Services

The following Qubes RPC services are provided to interact with ProtonVPN using the 
[Official ProtonVPN Linux CLI](https://github.com/ProtonVPN/linux-cli).

For other details see the project [README](./README.md) and [INSTALL](./INSTALL.md) documentation.

## RPC Service List

- [protonvpn.ConfigList](./netvm/etc/qubes-rpc/protonvpn.ConfigList)
  
  Service command: `/usr/bin/protonvpn-cli config --list`

  Argument: *not applicable*

  See also: `/usr/bin/protonvpn-cli config --help`

  Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.ConfigList`


- [protonvpn.Connect](./netvm/etc/qubes-rpc/protonvpn.Connect)

   Connect using cached session with no argument or listed Argument.

   Service command: `/usr/bin/protonvpn-cli c $argument`
   
   Argument: *none*, `fastest`, `random`, `secure-core` / `sc`, `p2p`, `tor`

   See also: `/usr/bin/protonvpn-cli c --help`

   Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.Connect+random`


- [protonvpn.Disconnect](./netvm/etc/qubes-rpc/protonvpn.Disconnect)

   Disconnects the active ProtonVPN connection.

   Service command: `/usr/bin/protonvpn-cli d`

   Argument: *not applicable*

   See also: `/usr/bin/protonvpn-cli --help`

   Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.Disconnect`


- [protonvpn.Killswitch](./netvm/etc/qubes-rpc/protonvpn.Killswitch)

   Sets ProtonVPN kill switch capability.

   Service command: `/usr/bin/protonvpn-cli ks $argument`

   Argument `on`, `off`, `permanent`

   See also: `/usr/bin/protonvpn-cli ks --help`

   Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.Killswitch+on


- [protonvpn.Netshield](./netvm/etc/qubes-rpc/protonvpn.Netshield)

   Sets the netshield security capabilitities.

   Service command: `/usr/bin/protonvpn-cli ns $argument`

   Argument: `off`, `malware`, `ads-malware` / `malware-ads-trackers`

   See also: `/usr/bin/protonvpn-cli ns --help`

   Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.Netshield+malware`


- [protonvpn.Protocol](./netvm/etc/qubes-rpc/protonvpn.Protocol)

   Sets the default connection protocol.

   Service command: `/usr/bin/protonvpn-cli config -p $argument`

   Argument: `tcp`, `udp`

   See also: `/usr/bin/protonvpn-cli config -p --help`

   Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.Protocol+tcp`


- [protonvpn.Reconnect](./netvm/etc/qubes-rpc/protonvpn.Reconnect)

   Reconnect using previous cached session.

   Service command: `/usr/bin/protonvpn-cli r`
   
   Argument: *not applicable*

   See also: `/usr/bin/protonvpn-cli --help`

   Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.Reconnect`


- [protonvpn.Status](./netvm/etc/qubes-rpc/protonvpn.Status)

   Display the current session status.

   Service command: `/usr/bin/protonvpn-cli s`
   
   Argument: *not applicable*

   See also: `/usr/bin/protonvpn-cli --help`

   Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.Status`


- [protonvpn.VpnAccelerator](./netvm/etc/qubes-rpc/protonvpn.VpnAccelerator)

   Sets the VPN performance accelerator capability.

   Service command: `/usr/bin/protonvpn-cli config --vpn-accelerator $argument`

   Argument: `enable`, `disable`

   See also: `/usr/bin/protonvpn-cli config --vpn-accelerator --help`

   Example usage:  `/usr/bin/qrexec-client-vm NETVM_NAME protonvpn.VpnAccelerator+enable`


## License

Copyright (C) 2022 nomuus

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.

Original source code available at <https://github.com/nomuus/protonvpn-qubes-rpc>.
