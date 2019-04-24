# Appname is Hostname

This is a dokku micro plugin that override the hostname used by Nginx to be
exactly the name of your application.

The full code of the plugin is (in `nginx-hostname` file):

```bash
#!/usr/bin/env bash
# Use the name of the app as hostname

set -eo pipefail; [[ $DOKKU_TRACE ]] && set -x

# This triggers receive $APP $SUBDOMAIN $VHOST as argument (in this order).
# Thus $1 contains the name of the app
echo "$1"
```

## Install

You can install it with:
```
$ dokku plugin:install https://github.com/NothingAG/appname-is-hostname
```

## Usage

The plugin does not provide any commands. After installing it, any new app will
use their application name as the hostname.

You will need to rebuild any previous app for the hostname to update.

There is no configuration of override possible. This the "micro" part in "micro
plugin".

## License

MIT License

Copyright (c) 2019 Nothing AG

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice (including the next
paragraph) shall be included in all copies or substantial portions of the
Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

