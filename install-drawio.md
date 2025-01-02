# Install drawio

Download application

```
https://github.com/jgraph/drawio-desktop/releases/download/v24.7.17/drawio-amd64-24.7.17.deb
```



Install application

```
sudo dpkg -i drawio-amd64-24.7.17.deb
```

Verify installation

```
dpkg --list

ii  draw.io                                 24.7.17                             amd64        

dpkg -s draw.io

Package: draw.io
Status: install ok installed
Priority: optional
Section: default
Installed-Size: 368130
Maintainer: JGraph <support@draw.io>
Architecture: amd64
Version: 24.7.17
Depends: libgtk-3-0, libnotify4, libnss3, libxss1, libxtst6, xdg-utils, libatspi2.0-0, libuuid1, libsecret-1-0
Recommends: libappindicator3-1
Description: 
  draw.io desktop
License: Apache-2.0
Vendor: JGraph <support@draw.io>
Homepage: https://github.com/jgraph/drawio

```

Look for path

```
dpkg -L draw.io

/opt/drawio/drawio
```

Add variable to path

```
vi .bashrc
  export PATH=$PATH:/opt/drawio

```

Execute application

```
drawio &> /dev/null &
[1] 118490
```

