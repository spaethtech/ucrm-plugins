# Local Development Environment (PhpStorm)


### Settings / PHP / Debug

> All settings should be left as their defaults, except those mentioned here.

Under `External Connections`, be sure to check:
- `Ignore external connections through unregistered server configurations`

### Settings / PHP / Servers

Create a new server, using the following settings:

| Setting           | Value                        | Notes                                                |
|-------------------|------------------------------|------------------------------------------------------|
| Name              | `localhost`                  |                                                      |
| Host              | `192.168.50.10`              | OR per Vagrantfile, see [Vagrant](vagrant.md) guides |
| Port              | `80`                         |                                                      |
| Debugger          | `Xdebug`                     |                                                      |
| Use path mappings | &check;                      |                                                      |
| ./environment/src | `/data/ucrm/data/plugins`    | See below                                            |
| ./environment/www | `/usr/src/ucrm/web/_plugins` | See below                                            |


### Triggers

The default Xdebug configuration sets the debug mode to `trigger` on the `PHPSTORM` session value. There are several
ways to make this work, but the preferred method is as follows:
- Install the Xdebug Helper extension
  - for [Chrome](https://chrome.google.com/webstore/detail/xdebug-helper/eadndfjplgieldjbigjakmdgkmoaaaoc?hl=en)
  - for [Firefox](https://addons.mozilla.org/en-US/firefox/addon/xdebug-helper-for-firefox/)
  - or your preferred browser, if available
- Configure the extension with the following:
  - Trigger Value (IDE Key): `PHPSTORM` (unless it ws changed in `xdebug.ini`)

You can now simply choose `Debug`, `Profile`, `Trace`, or `Disable` from the extension menu as you navigate the pages to
trigger the desired mode while the PhpStorm is listening for connections.

### Debugging

You can now toggle the `Start Listening for PHP Debug Connections` button in the top toolbar.

Set breakpoints in the Plugin code at `./environment/src` where needed.

And when the pages are requested in the browser, the breakpoints should be hit and your IDE should have the desired
information. Debug as desired!

> _**IMPORTANT:** As these breakpoints are in the deployed code, and your impulse will likely be to alter the code
> during the process, **DO NOT** forget to make the actual changes in your local source code._
> 
> _Additional tools to automate this process are forthcoming._