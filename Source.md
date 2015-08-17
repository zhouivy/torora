See BuildTororaonWindows for instructions on building Torora and WebKit in Windows.
## Requirements ##

Qt 4.5 or later is a strict requirement for building Torora. This is because only Qt 4.5
and greater support HTTP/HTTPS proxying properly. <b> If you build Torora with versions<br>
of Qt prior to 4.5 you will find that HTTP browsing over Tor works fine, but HTTPS<br>
doesn't work at all. </b>

## Until further notice you should always build Torora against WebKit trunk. ##

### Download and build WebKit trunk: ###

Follow the instructions at http://trac.webkit.org/wiki/QtWebKit

Quick and dirty for Linux:
```
    cd $HOME
    git clone git://git.webkit.org/WebKit.git WebKit
    cd WebKit
    ./Tools/Scripts/build-webkit --qt --makeargs=-j5 --no-video --no-svg
```

### Build Torora: ###
Note: WEBKITDIR should be set to the directory you created above.
```
    git clone git://github.com/mwenge/torora.git
    cd /path/to/torora/source
    cd torora
    export QT_WEBKIT=webkit_trunk
    export WEBKITDIR=$HOME/WebKit
    qmake "CONFIG-=debug" "DEFINES+=TORORA" -r
    make clean
    make
```

### Test/Troubleshoot: ###
  * Ensure your Torora binary is built against the webkit trunk:
```
robert@mwenge:~/Development/torora$ ldd torora
        linux-gate.so.1 =>  (0xb7f39000)
        libQtWebKit.so.4 => /home/robert/Development/WebKit/WebKitBuild/Debug/lib/libQtWebKit.so.4 (0xb5408000)
```
  * The file webkittrunk.pri applies the paths you select, if you built webkit with '--release' then ensure the appropriate section in webkittrunk.pri looks like:
```
    isEmpty(WEBKITBRANCH) {
        CONFIG(debug):WEBKITBUILD = $$WEBKITDIR/WebKitBuild/Debug/lib
        CONFIG(release):WEBKITBUILD = $$WEBKITDIR/WebKitBuild/Release/lib
    } else {
        CONFIG(debug):WEBKITBUILD = $$WEBKITDIR/WebKitBuild/$$WEBKITBRANCH/Debug/lib
        CONFIG(release):WEBKITBUILD = $$WEBKITDIR/WebKitBuild/$$WEBKITBRANCH/Release/lib
    }
```

  * If you built with '--debug' then it should be:
```
    isEmpty(WEBKITBRANCH) {
        CONFIG(release):WEBKITBUILD = $$WEBKITDIR/WebKitBuild/Release/lib
        CONFIG(debug):WEBKITBUILD = $$WEBKITDIR/WebKitBuild/Debug/lib
    } else {
        CONFIG(release):WEBKITBUILD = $$WEBKITDIR/WebKitBuild/$$WEBKITBRANCH/Release/lib
        CONFIG(debug):WEBKITBUILD = $$WEBKITDIR/WebKitBuild/$$WEBKITBRANCH/Debug/lib
    }
```