```
,' ` " , . + ' * ` ' ` " `,
:                         ;
v___ ___ _____ _ _ _ _ ___^
|   | . |     | | | | |_ -|  
|_|_|___|_|_|_|___|___|___|  Qubes ProtonVPN RPC Services
;.`.'.`.'.`.'.`.'.`.'.`.'.:  
:.'.`.'.`.'.`.'.`.'.`.'.`.! 
:.`.'.`.'.`.'.`.'.`.'.`.'.: 
!''``''``''``''``''``''``';
`-->[github.com/nomuus]---^
```

# Qubes ProtonVPN RPC Services

This project provides [Qubes](https://www.qubes-os.org) RPC services for 
[ProtonVPN](https://protonvpn.com). It leverages the 
[Qubes qrexec framework]((https://www.qubes-os.org/doc/qrexec)) 
to call the official [ProtonVPN Linux CLI](https://github.com/ProtonVPN/linux-cli). 
It is intended for Qubes users that want to control a NetVM's active 
ProtonVPN connection from other Qubes domains. 

Access control is provided using built-in 
[Qubes RPC policies](https://www.qubes-os.org/doc/rpc-policy/) and 
[tags](https://www.qubes-os.org/doc/qrexec/#specifying-vms-tags-types-targets-etc) 
(viz. manual use of `qvm-tags` in dom0). Policy files are provided 
as the original format for backwards compatibility, and support 
for the [new format](https://www.qubes-os.org/news/2020/06/22/new-qrexec-policy-system/) 
introduced in Qubes 4.1 be added in a future update.


## Prerequisites

- Qubes 4.x
- Linux AppVM/TemplateVM/NetVM
- [ProtonVPN Linux CLI](https://github.com/ProtonVPN/linux-cli) installed on a NetVM
- ProtonVPN account **logged on and configured** on the NetVM
- Administrative experience to manually configure and setup NetVM and dom0


## Installation

In summary, one must copy RPC service files to the ProtonVPN NetVM and copy 
policy files in dom0. This is detailed in the [INSTALL](./INSTALL.md) documentation.


## Usage

Use will depend on the policy configuration. The policy provided with the 
project will provide access control for one NetVM to the many client VM's 
using it. 

From an AppVM tagged `protonmgr`, use the Qubes `qrexec-client-vm` tool to 
interact with a NetVM tagged `protonvpn`.


Without an argument:
```
/usr/bin/qrexec-client-vm PROTONVPN_VM_NAME rpc_service_name
```

With an argument:
```
/usr/bin/qrexec-client-vm PROTONVPN_VM_NAME rpc_service_name+Argument
```


### Complex Tagging
The concept of tagging in Qubes can allow for more complex cases, such as 
multiple NetVM's providing a ProtonVPN connection to different servers.

More granular tagging may be needed, for example,  if a group of AppVM's  
should only have access to one NetVM, but another AppVM group should only 
have access to another NetVM.

The solution would be to create multiple tags to control access, for 
example, AppVM's tagged `protonmgrA` and `protonmgrB`, and NetVM's tagged 
`protonvpnA` and `protonvpnB`, then modify the policy file(s) to assign 
the policy for the tags accordingly.


## Examples

#### Example 1
From the AppVM tagged `protonmgr`, instruct the `protonvpn` tagged disposable NetVM named `disp1355` to connect using the cached session to the fastest available server:

```
/usr/bin/qrexec-client-vm disp1355 protonvpn.Connect+fastest
```


#### Example 2

From the AppVM tagged `protonmgrSC`, instruct the `protonvpnSC` tagged NetVM named `sys-protonvpn-secure-core` to connect using the cached session to the fastest available secure core server:

```
/usr/bin/qrexec-client-vm sys-protonvpn-secure-core protonvpn.Connect+securecore
```

- *Note: In this example, the policy file(s) could be further modified
   to only allow tagged `protonmgrSC` AppVM's to access the `protonvpn.Connect`
   RPC service with a `securecore` argument, for example, within original policy 
   formatted file in dom0 `/etc/qubes-rpc/policy/protonvpn.Connect+securecore`.*


#### Example 3

From the AppVM tagged `protonmgr`, instruct the `protonvpn` tagged disposable NetVM named `disp7573` to connect using the cached session to the a server from country code `US` (United States):

```
/usr/bin/qrexec-client-vm disp7573 protonvpn.Connect+cc_us
```


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
