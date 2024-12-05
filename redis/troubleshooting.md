## troubleshooting during installation

###  01
In file included from adlist.c:34:
zmalloc.h:50:10: fatal error: jemalloc/jemalloc.h: No such file or directory
 #include <jemalloc/jemalloc.h>
          ^~~~~~~~~~~~~~~~~~~~~
compilation terminated.


make USE_JEMALLOC=no

### 02 
linux redis release.c:37:10: fatal error: release.h: No such file or directory
37 | #include “release.h”

Steps: 1.cd to the src directory in the file
2.chmod +x mkreleasehdr.sh
3.make

