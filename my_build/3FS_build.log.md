# 3FS build log 

## cmake 정상로그
* cmake 준비
```log
jhyunlee@happy:~/3fs$ cmake -S . -B build -DCMAKE_CXX_COMPILER=clang++-14 -DCMAKE_C_COMPILER=clang-14 -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DCMAKE_VERBOSE_MAKEFILE=ON
-- CMAKE_BUILD_TYPE: RelWithDebInfo
-- ENABLE_ASSERTIONS: OFF
-- OVERRIDE_CXX_NEW_DELETE: OFF
-- SAVE_ALLOCATE_SIZE: OFF
-- Sanitizer not enabled. 
-- Version: 10.0.1
-- Build type: RelWithDebInfo
-- ZSTD VERSION: 1.5.5
-- CMAKE_INSTALL_PREFIX: /usr
-- CMAKE_INSTALL_LIBDIR: lib/x86_64-linux-gnu
-- ZSTD_LEGACY_SUPPORT defined!
-- ZSTD_MULTITHREAD_SUPPORT is enabled
-- current platform: Linux 
-- Found Boost: /usr/lib/x86_64-linux-gnu/cmake/Boost-1.74.0/BoostConfig.cmake (found suitable version "1.74.0", minimum required is "1.51.0") found components: context filesystem program_options regex system thread 
-- Found gflags from package config /usr/lib/x86_64-linux-gnu/cmake/gflags/gflags-config.cmake
-- Found gflags as a dependency of glog::glog, include=/usr/include, libs=gflags::gflags_shared
-- Found libevent: /usr/lib/x86_64-linux-gnu/libevent.so
-- Could NOT find BZip2 (missing: BZIP2_LIBRARIES BZIP2_INCLUDE_DIR) 
-- Found LZ4: /usr/lib/x86_64-linux-gnu/liblz4.so
CMake Warning (dev) at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:438 (message):
  The package name passed to `find_package_handle_standard_args` (ZSTD) does
  not match the name of the calling package (Zstd).  This can lead to
  problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  third_party/folly/CMake/FindZstd.cmake:32 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  third_party/folly/CMake/folly-deps.cmake:114 (find_package)
  third_party/folly/CMakeLists.txt:141 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Could NOT find ZSTD (missing: ZSTD_LIBRARY) 
CMake Warning (dev) at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:438 (message):
  The package name passed to `find_package_handle_standard_args` (SNAPPY)
  does not match the name of the calling package (Snappy).  This can lead to
  problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  third_party/folly/CMake/FindSnappy.cmake:31 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  third_party/folly/CMake/folly-deps.cmake:121 (find_package)
  third_party/folly/CMakeLists.txt:141 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Could NOT find SNAPPY (missing: SNAPPY_LIBRARY SNAPPY_INCLUDE_DIR) 
CMake Warning (dev) at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:438 (message):
  The package name passed to `find_package_handle_standard_args` (LIBDWARF)
  does not match the name of the calling package (LibDwarf).  This can lead
  to problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  third_party/folly/CMake/FindLibDwarf.cmake:25 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  third_party/folly/CMake/folly-deps.cmake:128 (find_package)
  third_party/folly/CMakeLists.txt:141 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

CMake Warning (dev) at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:438 (message):
  The package name passed to `find_package_handle_standard_args` (LIBIBERTY)
  does not match the name of the calling package (Libiberty).  This can lead
  to problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  third_party/folly/CMake/FindLibiberty.cmake:22 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  third_party/folly/CMake/folly-deps.cmake:132 (find_package)
  third_party/folly/CMakeLists.txt:141 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Could NOT find LIBIBERTY (missing: LIBIBERTY_LIBRARY LIBIBERTY_INCLUDE_DIR) 
CMake Warning (dev) at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:438 (message):
  The package name passed to `find_package_handle_standard_args` (LIBAIO)
  does not match the name of the calling package (LibAIO).  This can lead to
  problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  third_party/folly/CMake/FindLibAIO.cmake:22 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  third_party/folly/CMake/folly-deps.cmake:136 (find_package)
  third_party/folly/CMakeLists.txt:141 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

CMake Warning (dev) at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:438 (message):
  The package name passed to `find_package_handle_standard_args` (LIBURING)
  does not match the name of the calling package (LibUring).  This can lead
  to problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  third_party/folly/CMake/FindLibUring.cmake:22 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  third_party/folly/CMake/folly-deps.cmake:140 (find_package)
  third_party/folly/CMakeLists.txt:141 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Could NOT find LIBURING (missing: LIBURING_LIBRARY LIBURING_INCLUDE_DIR) 
CMake Warning (dev) at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:438 (message):
  The package name passed to `find_package_handle_standard_args` (LIBSODIUM)
  does not match the name of the calling package (Libsodium).  This can lead
  to problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  third_party/folly/CMake/FindLibsodium.cmake:22 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  third_party/folly/CMake/folly-deps.cmake:144 (find_package)
  third_party/folly/CMakeLists.txt:141 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Could NOT find LIBSODIUM (missing: LIBSODIUM_LIBRARY LIBSODIUM_INCLUDE_DIR) 
CMake Warning (dev) at /usr/share/cmake-3.22/Modules/FindPackageHandleStandardArgs.cmake:438 (message):
  The package name passed to `find_package_handle_standard_args` (LIBUNWIND)
  does not match the name of the calling package (LibUnwind).  This can lead
  to problems in calling code that expects `find_package` result variables
  (e.g., `_FOUND`) to follow a certain pattern.
Call Stack (most recent call first):
  third_party/folly/build/fbcode_builder/CMake/FindLibUnwind.cmake:22 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  third_party/folly/CMake/folly-deps.cmake:163 (find_package)
  third_party/folly/CMakeLists.txt:141 (include)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Setting FOLLY_USE_SYMBOLIZER: ON
-- Setting FOLLY_HAVE_ELF: 1
-- Setting FOLLY_HAVE_DWARF: TRUE
-- arch x86_64-linux-gnu matches x86_64, building SSE4.2 version of base64
-- compiler has flag pclmul, setting compile flag for /home/jhyunlee/3fs/third_party/folly/folly/hash/detail/ChecksumDetail.cpp;/home/jhyunlee/3fs/third_party/folly/folly/hash/detail/Crc32CombineDetail.cpp;/home/jhyunlee/3fs/third_party/folly/folly/hash/detail/Crc32cDetail.cpp
-- Could NOT find uring (missing: uring_LIBRARIES uring_INCLUDE_DIR) 
-- Enabling RTTI in all builds
-- ROCKSDB_PLUGINS: 
-- ROCKSDB PLUGINS TO BUILD 
fatal: 어떤 태그도 '7c0838e65e7db1dd10b7a8411553c04f2d89fa61'와(과) 정확히 일치하지 않습니다
-- JNI library is disabled
-- scn version: 1.1.3
-- SCN_PEDANTIC: OFF
-- SCN_WERROR: OFF
-- pybind11 v2.10.0 
-- 
-- 
-- Library base name: mimalloc
-- Version          : 2.1
-- Build type       : relwithdebinfo
-- C Compiler       : /usr/bin/clang-14
-- Compiler flags   : -Wall;-Wextra;-Wno-unknown-pragmas;-fvisibility=hidden;-Wstrict-prototypes;-Wpedantic;-Wno-static-in-inline;-ftls-model=initial-exec
-- Compiler defines : 
-- Link libraries   : /usr/lib/x86_64-linux-gnu/libpthread.a;/usr/lib/x86_64-linux-gnu/librt.a
-- Build targets    : shared;static;object;tests
-- 
-- Found Boost: /usr/lib/x86_64-linux-gnu/cmake/Boost-1.74.0/BoostConfig.cmake (found version "1.74.0") found components: filesystem system program_options 
-- Found clang-format at /usr/bin/clang-format-14
fatal: 이름이 없습니다. 아무것도 설명할 수 없습니다.
fatal: 이름이 없습니다. 아무것도 설명할 수 없습니다.
-- Git Commit hash: 3c100b90 3c100b90ce31b37da6f089555ba60bdfe07ec974
-- Git Commit Date & Timestamp: 20250313 1741837783
-- Git Commit Tag & Seq Num: 250228 1
-- Enabled IPO for target: hf3fs_common_shared
-- Enabled IPO for target: meta_main
-- Enabled IPO for target: admin_cli
-- Enabled IPO for target: hf3fs_api_shared
-- Enabled IPO for target: storage_main
-- Enabled IPO for target: mgmtd_main
-- Enabled IPO for target: monitor_collector_main
-- Enabled IPO for target: hf3fs-admin
-- Enabled IPO for target: hf3fs_fuse_main
-- Enabled IPO for target: simple_example_main
-- Enabled IPO for target: migration_main
-- Enabled IPO for target: storage_bench
-- Configuring done
-- Generating done
-- Build files have been written to: /home/jhyunlee/3fs/build
```
### build 결과 
```sh
jhyunlee@happy:~/3fs/build/bin$ ls -l
합계 3026828
-rwxrwxr-x 1 jhyunlee jhyunlee 363910048  3월 15 00:31 admin_cli
-rw-rw-r-- 1 jhyunlee jhyunlee 752450790  3월 15 00:36 bin.tar.gz
-rwxrwxr-x 1 jhyunlee jhyunlee 148480056  3월 15 00:12 hf3fs-admin
-rwxrwxr-x 1 jhyunlee jhyunlee 209267640  3월 15 00:12 hf3fs_fuse_main
-rwxrwxr-x 1 jhyunlee jhyunlee 284511088  3월 15 00:12 meta_main
-rwxrwxr-x 1 jhyunlee jhyunlee 178916176  3월 15 00:13 mgmtd_main
-rwxrwxr-x 1 jhyunlee jhyunlee 172360792  3월 15 00:07 migration_main
-rwxrwxr-x 1 jhyunlee jhyunlee 105220144  3월 14 23:45 monitor_collector_main
-rwxrwxr-x 1 jhyunlee jhyunlee 174745408  3월 15 00:07 simple_example_main
-rwxrwxr-x 1 jhyunlee jhyunlee 396960840  3월 15 00:28 storage_bench
-rwxrwxr-x 1 jhyunlee jhyunlee 312654048  3월 15 00:12 storage_main
```

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


