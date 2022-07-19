# Qubes ProtonVPN RPC Services Installation

There are two main components that follow the [Qubes qrexec framework]((https://www.qubes-os.org/doc/qrexec)) 
requiring installation in the target NetVM(s) and within dom0. The NetVM services consist of scripts and the 
dom0 configurations consist of RPC policy files and manual tagging of desired domains.

Once configured, administration should primarily consist of adding and removing tags that control access 
to the RPC services.

## ProtonVPN Linux

For install instructions of the Linux CLI, see the official guide: https://protonvpn.com/support/linux-vpn-tool/

For source code of the official Linux CLI, see: https://github.com/ProtonVPN/linux-cli


## NetVM RPC services

ProtonVPN RPC services are implemented as per [Qubes documentation](https://www.qubes-os.org/doc/qrexec). 
The Qubes services call the [Official ProtonVPN Linux CLI](https://github.com/ProtonVPN/linux-cli) using 
the official `protonvpn-cli` command line tool. Services are intended to be executed on the NetVM from 
another domain using the Qrexec Framework.

Documentation for this project's RPC services are located [here](./SERVICES.md).

Each RPC service file should be placed in an appropriate directory 
as per [Qubes RPC administration documentation](https://www.qubes-os.org/doc/qrexec/#qubes-rpc-administration):

> In the target VM, a file in either of the following locations must exist, containing the file name of the program that will be invoked, or being that program itself – in which case it must have executable permission set (chmod +x):
> 
>    - `/etc/qubes-rpc/RPC_ACTION_NAME` when you make it in the template qube;
>    - `/usr/local/etc/qubes-rpc/RPC_ACTION_NAME` for making it only in an app qube.

### Steps

1) Make the script executable, for example:

   `chmod +x protonvpn.SERVICENAME`

2) Change ownership to root, for example,

   `sudo chown root:root protonvpn.SERVICENAME`

3) Move the script into an appropriate directory, for example:

   `sudo mv protonvpn.SERVICENAME /etc/qubes-rpc`
     
   Depending on the VM, it may or may not persist.


## Dom0 Policies

The provided policy files are templates and should be modified to suit environmental requirements.
This project provides default named policy files for each RPC service. Policy files are provided 
as the original format for backwards compatibility, and support 
for the [new format](https://www.qubes-os.org/news/2020/06/22/new-qrexec-policy-system/) 
introduced in Qubes 4.1 be added in a future update.

The basic process is to tag any VM providing a ProtonVPN connection (e.g. ProtonVPN NetVM or AppVM) 
using the tag `protonvpn` and any other VM's that will control the aforementioned connection
using `protonmgr`. Not all VM's need the `protonmgr` tag; only those required such capability.

More complex tagging, such as that discussed in the [README](./README.md), can be accomplished to 
suit the needs of the environment. For extended documentation on RPC policies, including tagging, refer to 
[Official Qubes RPC administration documentation](https://www.qubes-os.org/doc/qrexec/#qubes-rpc-administration).

### Steps

1) Place policy file(s) from the [./dom0/etc/qubes-rpc/policy](./dom0/etc/qubes-rpc/policy) directory 
   within host's actual dom0 `/etc/qubes-rpc/policy` directory.

   If desired create argument-specfic policies per arguments (e.g. `rpc_name+RPC_ARGUMENT`).

2) Modify each policy to suit environmental requirements.

   *The default will allow any `protonmgr` tagged VM's to call RPC services for `protonvpn` NetVM's.*

3) Tag the NetVM or VM's running ProtonVPN as `protonvpn`.

   Example:  `qvm-tag somevm add protonvpn`

4) Tag the VM's requiring the provided RPC capability as `protonmgr`.
   
   Not all VM's will need this capability, otherwise any VM can potentially 
   manipulate the ProtonVPN connection. This may or may not be desired.

   Example:  `qvm-tag othervm add protonmgr`


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
