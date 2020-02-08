# runit-utils

## About

Utilities to support system integration and extend functionality of [runit-init](https://github.com/cloux/runit) and [runit-base](https://github.com/cloux/runit-base). Formerly part of [aws-devuan](https://github.com/cloux/aws-devuan) and [sin/runit-init](https://github.com/cloux/sin/tree/master/modules/runit-init) repositories, these utilities are now being developed separately for easier distribution, updates and packaging.

---
## Installation

Runit-utils are installed automatically as a part of runit-init system by the [sin/runit-init](https://github.com/cloux/sin/blob/master/modules/runit-init/install) module:

```
sin runit-init
```

To install manually without [sin](https://github.com/cloux/sin), copy the files to the appropriate locations:

```
git clone https://github.com/cloux/runit-utils
cd runit-utils
sudo cp -ufv --preserve=mode -t /sbin/ support/*
cd compat
sudo cp -ufv --preserve=mode -t /sbin/ shutdown.runit insserv.runit
sudo cp -ufv --preserve=mode -t /usr/sbin/ invoke-rc.d.runit start-statd.runit update-rc.d.runit
sudo cp -ufv --preserve=mode -t /bin/ pidof.runit
```

### System Integration

All scripts are integrated into the system by the [runit-base autorun](https://github.com/cloux/runit-base/blob/master/etc/runit/autorun/runit-init) after boot, when runit-init is PID1 process. This autorun integration is required in order to allow for seamless upgrade from SysVinit to runit-init with no breakup in the OS functionality.

---
## Usage

The [compat scripts](https://github.com/cloux/runit-utils/tree/master/compat) do not have a manpage, they are compatible replacements for sysvinit interfaces:

 * invoke-rc.d ([SysVinit manpage](https://manpages.debian.org/testing/init-system-helpers/invoke-rc.d.8.en.html)) - executes service actions, required by dpkg
 * update-rc.d ([SysVinit manpage](https://manpages.debian.org/testing/init-system-helpers/update-rc.d.8.en.html)) - install and remove service links, required by dpkg
 * insserv ([SysVinit manpage](https://manpages.debian.org/testing/insserv/insserv.8.en.html))  - boot sequence organizer, used internally by update-rc.d
 * pidof ([SysVinit manpage](https://linux.die.net/man/8/pidof)) - find the process ID of a running program, replaces `pidof` from the sysvinit-utils	
 * shutdown - takes no parameters, equivalent to `shutdown now` from sysvinit-core package
 * start-statd - required internally by nfsmount from nfs-common package

The [support scripts](https://github.com/cloux/runit-utils/tree/master/support) to enhance and simplify runit service management:

 * svactivate - include and start services in Runit supervisor
	 - Syntax: `svactivate SERVICE [SERVICE ...]` where SERVICE is any directory in _/etc/sv/_
 * svdeactivate - stop services and disable supervision
	 - Syntax: `svdeactivate SERVICE [SERVICE ...]` where SERVICE is any directory in _/etc/service/_
 * svstat - show status of supervised services
	 - Equivalent to `sv status SERVICE` or `sv status /etc/service/*` when run without parameter
	 - Glob matching works if escaped: `svstat a\* '*s'` returns all services whose names start with 'a' and also services whose names ends with 's' (acpid, agetty-1, dbus). 

---
<a href="http://www.wtfpl.net"><img src="http://www.wtfpl.net/wp-content/uploads/2012/12/wtfpl-badge-2.png" align="right"></a>
## License

This work is free. You can redistribute it and/or modify it under the terms of the Do What The Fuck You Want To Public License, Version 2, as published by Sam Hocevar. See http://www.wtfpl.net for more details. If you feel that releasing this work under WTFPL is not appropriate, just do WTF you want to. If you feel that using some of this code might be violating some other license, don't use the code (see [Disclaimer](#disclaimer)).

---
## Author

This repository is maintained by _cloux@rote.ch_

### Disclaimer

I do not claim fitness of this project for any particular purpose and do not take any responsibility for its use. You should always choose your system and all of its components very carefully, if something breaks it's on you. See [license](#license).

### Contributing

I will keep this project alive as long as I can. This is however a private project, so my support is fairly limited. Any help with further development, testing, and bugfixing will be appreciated. If you want to report a bug, please either [raise an issue](https://github.com/cloux/runit-utils/issues), or fork the project and send me a pull request.

---