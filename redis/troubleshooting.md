## troubleshooting during installation

###  01
```
In file included from adlist.c:34:
zmalloc.h:50:10: fatal error: jemalloc/jemalloc.h: No such file or directory
 #include <jemalloc/jemalloc.h>
          ^~~~~~~~~~~~~~~~~~~~~
compilation terminated.
```
Steps: 
```
make USE_JEMALLOC=no
```

### 02 
```
linux redis release.c:37:10: fatal error: release.h: No such file or directory
37 | #include “release.h”
```
Steps: 
```
cd /src
chmod +x mkreleasehdr.sh
make
```
