# 3FS build log 

## 1차 오류
```log
[ 66%] Building CXX object src/common/CMakeFiles/hf3fs_common_shared.dir/utils/SysvShm.cc.o
[ 66%] Building CXX object src/common/CMakeFiles/hf3fs_common_shared.dir/utils/Toml.cc.o
[ 66%] Building CXX object src/common/CMakeFiles/hf3fs_common_shared.dir/utils/TracingEvent.cc.o
[ 66%] Building CXX object src/common/CMakeFiles/hf3fs_common_shared.dir/utils/UtcTime.cc.o
[ 66%] Building CXX object src/common/CMakeFiles/hf3fs_common_shared.dir/utils/Uuid.cc.o
[ 66%] Building CXX object src/common/CMakeFiles/hf3fs_common_shared.dir/utils/coding.cc.o
[ 66%] Building CXX object src/common/CMakeFiles/hf3fs_common_shared.dir/utils/suicide.cc.o
[ 66%] Linking CXX shared library libhf3fs_common_shared.so
[ 66%] Built target hf3fs_common_shared
gmake: *** [Makefile:146: all] 오류 2
```
==> target hf3fs_common_shared

#### cmake --build build -j 16 -v -t hf3fs_common_shared

```log
Dependencies file "src/common/CMakeFiles/hf3fs_common_shared.dir/utils/suicide.cc.o.d" is newer than depends file "/home/jhyunlee/3fs/build/src/common/CMakeFiles/hf3fs_common_shared.dir/compiler_depend.internal".
Consolidate compiler generated dependencies of target hf3fs_common_shared
gmake[3]: 디렉터리 '/home/jhyunlee/3fs/build' 나감
/usr/bin/gmake  -f src/common/CMakeFiles/hf3fs_common_shared.dir/build.make src/common/CMakeFiles/hf3fs_common_shared.dir/build
gmake[3]: 디렉터리 '/home/jhyunlee/3fs/build' 들어감
gmake[3]: 'src/common/CMakeFiles/hf3fs_common_shared.dir/build'을(를) 위해 할 일이 없습니다.
gmake[3]: 디렉터리 '/home/jhyunlee/3fs/build' 나감
[100%] Built target hf3fs_common_shared
gmake[2]: 디렉터리 '/home/jhyunlee/3fs/build' 나감
/usr/bin/cmake -E cmake_progress_start /home/jhyunlee/3fs/build/CMakeFiles 0
gmake[1]: 디렉터리 '/home/jhyunlee/3fs/build' 나감

```

## 2차 오류 
```sh
$ cmake -S . -B build -DCMAKE_CXX_COMPILER=clang++-14 -DCMAKE_C_COMPILER=clang-14 -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_VERBOSE_MAKEFILE=ON
$ cmake --build build -j 16 -v 2>&1  | tee build.log
```
```log
In file included from /home/jhyunlee/3fs/src/fdb/FDBTransaction.cc:1:
In file included from /home/jhyunlee/3fs/src/fdb/FDBTransaction.h:10:
/home/jhyunlee/3fs/src/fdb/FDB.h:11:10: fatal error: 'foundationdb/fdb_c_types.h' file not found
#include "foundationdb/fdb_c_types.h"
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~
In file included from /home/jhyunlee/3fs/src/fdb/FDB.cc:1:
/home/jhyunlee/3fs/src/fdb/FDB.h:11:10: fatal error: 'foundationdb/fdb_c_types.h' file not found
#include "foundationdb/fdb_c_types.h"
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~
1 error generated.
gmake[2]: *** [src/fdb/CMakeFiles/fdb.dir/build.make:79: src/fdb/CMakeFiles/fdb.dir/FDB.cc.o] 오류 1
gmake[2]: *** 끝나지 않은 작업을 기다리고 있습니다....
In file included from /home/jhyunlee/3fs/src/fdb/HybridKvEngine.cc:5:
In file included from /home/jhyunlee/3fs/src/fdb/FDBContext.h:5:
/home/jhyunlee/3fs/src/fdb/FDB.h:11:10: fatal error: 'foundationdb/fdb_c_types.h' file not found
#include "foundationdb/fdb_c_types.h"
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~
In file included from /home/jhyunlee/3fs/src/fdb/FDBContext.cc:1:
In file included from /home/jhyunlee/3fs/src/fdb/FDBContext.h:5:
/home/jhyunlee/3fs/src/fdb/FDB.h:11:10: fatal error: 'foundationdb/fdb_c_types.h' file not found
#include "foundationdb/fdb_c_types.h"
         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~
1 error generated.
gmake[2]: *** [src/fdb/CMakeFiles/fdb.dir/build.make:107: src/fdb/CMakeFiles/fdb.dir/FDBTransaction.cc.o] 오류 1
1 error generated.
gmake[2]: *** [src/fdb/CMakeFiles/fdb.dir/build.make:121: src/fdb/CMakeFiles/fdb.dir/HybridKvEngine.cc.o] 오류 1
1 error generated.
gmake[2]: *** [src/fdb/CMakeFiles/fdb.dir/build.make:93: src/fdb/CMakeFiles/fdb.dir/FDBContext.cc.o] 오류 1
gmake[2]: 디렉터리 '/home/jhyunlee/3fs/build' 나감
gmake[1]: *** [CMakeFiles/Makefile2:4606: src/fdb/CMakeFiles/fdb.dir/all] 오류 2
gmake[1]: *** 끝나지 않은 작업을 기다리고 있습니다....
[ 60%] Linking CXX executable ../test_kv
```
==>  foundationdb 버젼을 6.3에서 7.1 이상으로 올린다.  
```sh
$ wget --no-check-certificate  https://github.com/apple/foundationdb/releases/download/7.1.67/foundationdb-clients_7.1.67-1_amd64.deb
$ wget --no-check-certificate  https://github.com/apple/foundationdb/releases/download/7.1.67/foundationdb-server_7.1.67-1_amd64.deb
$ sudo dpkg -i foundationdb-clients_7.1.67-1_amd64.deb  foundationdb-server_7.1.67-1_amd64.deb
```


