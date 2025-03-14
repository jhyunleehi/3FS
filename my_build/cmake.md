# cmake

## **CMake란?**  
CMake는 크로스 플랫폼 빌드 시스템을 생성하는 도구로, 소스 코드에서 플랫폼에 맞는 빌드 파일(예: Makefile, Ninja, Visual Studio 프로젝트 등)을 자동으로 생성하는 역할을 합니다.  

### **CMake의 주요 특징**  
- **크로스 플랫폼**: Windows, Linux, macOS 등 다양한 운영 체제에서 지원  
- **다양한 빌드 시스템 지원**: Make, Ninja, Visual Studio 등 다양한 빌드 시스템을 생성 가능  
- **간단한 설정 파일 (`CMakeLists.txt`)**: 프로젝트의 빌드 과정을 쉽게 정의  
- **의존성 관리**: 외부 라이브러리와의 연동이 쉬움  

---

## **CMake 사용 예제**  

### **1. 간단한 C++ 프로젝트 빌드**
#### **프로젝트 구조**
```plaintext
my_project/
│── CMakeLists.txt
│── src/
│   ├── main.cpp
│   ├── math.cpp
│   └── math.h
└── build/  (CMake 빌드 디렉토리)
```

#### **소스 코드 (`src/main.cpp`)**
```cpp
#include <iostream>
#include "math.h"

int main() {
    int a = 5, b = 3;
    std::cout << "Sum: " << add(a, b) << std::endl;
    return 0;
}
```

#### **헤더 파일 (`src/math.h`)**
```cpp
#ifndef MATH_H
#define MATH_H

int add(int a, int b);

#endif
```

#### **소스 코드 (`src/math.cpp`)**
```cpp
#include "math.h"

int add(int a, int b) {
    return a + b;
}
```

#### **CMake 설정 파일 (`CMakeLists.txt`)**
```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject)

# 실행 파일을 만들고 소스 파일 추가
add_executable(my_program src/main.cpp src/math.cpp)
```

---

## **2. 빌드 과정**
### **1) CMake 실행 및 빌드**
```sh
mkdir build
cd build
cmake ..
make
```
### **2) 실행**
```sh
./my_program
```
출력:
```plaintext
Sum: 8
```

---

## **CMake 추가 기능 예시**
### **1. 특정 C++ 버전 사용**
```cmake
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```

### **2. 라이브러리 추가 (`math`를 별도 라이브러리로 분리)**
```cmake
add_library(math_lib src/math.cpp)
target_include_directories(math_lib PUBLIC src)
target_link_libraries(my_program PRIVATE math_lib)
```

---

이제 CMake를 활용하여 더 복잡한 프로젝트도 쉽게 관리할 수 있습니다! 😊



# make 단계 발생하는 오류 
`[Makefile:146: all] Error 2` 오류는 `make` 실행 중에 발생한 문제로, 보통 빌드 과정에서 특정 명령이 실패했을 때 나타납니다. 이 오류의 원인을 찾기 위해 다음과 같은 방법으로 문제를 진단할 수 있습니다.  

---

### 1. **상세 오류 메시지 확인**
오류 메시지가 더 있는지 확인하세요. 보통 `Error 2` 앞쪽에 실패한 명령과 관련된 자세한 로그가 출력됩니다.  

```sh
make VERBOSE=1
```
또는  
```sh
make -j$(nproc) 2>&1 | tee build.log
```
위 명령을 사용하여 전체 빌드 로그를 파일(`build.log`)에 저장하고 분석할 수도 있습니다.  

---

### 2. **디버그 모드 활성화 (`make`의 `-d` 옵션 사용)**
Makefile 내부에서 어떤 과정이 실행되었는지 상세하게 확인하려면 `-d` 옵션을 추가하세요.  

```sh
make -d
```
출력된 로그에서 `Error 2`가 발생하기 전에 어떤 명령이 실행되었는지 확인하고, 실패한 부분을 분석하면 됩니다.  

---

### 3. **Makefile 146번째 줄 확인**
오류 메시지에서 `Makefile:146`이라고 되어 있으므로 `Makefile`의 146번째 줄을 직접 확인하여 문제가 있는지 살펴보세요.  

```sh
sed -n '146p' Makefile
```
또는 직접 `Makefile`을 열어 해당 줄을 확인하세요. 이 부분이 `all` 대상과 관련된 규칙을 포함하고 있을 가능성이 큽니다.  

---

### 4. **`CMakeLists.txt` 확인 및 Makefile 다시 생성**
CMake를 사용하여 Makefile을 생성한 경우, `CMakeLists.txt`에서 잘못된 설정이 있을 수도 있습니다.  

1. 기존 빌드 디렉토리를 삭제하고 다시 생성  
    ```sh
    rm -rf build
    mkdir build
    cd build
    cmake ..
    make
    ```
2. CMake 캐시를 삭제하고 다시 설정  
    ```sh
    cmake --fresh ..
    make
    ```
3. `CMakeLists.txt`에서 오류가 없는지 확인  
   - 존재하지 않는 파일을 컴파일하려 하거나  
   - 경로가 잘못 지정된 경우 발생할 수 있음  

---

### 5. **의존성 라이브러리 확인**
빌드 과정에서 필요한 라이브러리가 누락된 경우 발생할 수 있습니다. 다음 명령으로 필요한 라이브러리가 설치되어 있는지 확인하세요.  

```sh
ldd <실행파일_또는_라이브러리> | grep "not found"
```
또한, `pkg-config`를 사용하여 필요한 라이브러리가 제대로 설치되었는지 확인할 수도 있습니다.  

```sh
pkg-config --modversion <라이브러리_이름>
```

---

### 6. **`make` 병렬 빌드 문제 확인**
여러 개의 스레드(`-j` 옵션)를 사용하여 빌드할 때 `Error 2`가 발생할 수 있습니다.  

```sh
make -j1
```
병렬 빌드를 해제하고 실행하면, 오류가 어디에서 발생하는지 더 명확하게 확인할 수 있습니다.  

---

### 7. **컴파일러 버전 및 환경 확인**
컴파일러 버전이 맞지 않거나, `CMakeLists.txt`에서 특정한 컴파일러 옵션이 필요한 경우도 있습니다. 다음 명령으로 환경을 확인하세요.  

```sh
gcc --version  # 또는 clang --version
cmake --version
```

---

위 방법들을 순서대로 확인하면서 `Error 2`의 원인을 찾으면 문제를 해결할 수 있을 것입니다. 오류 메시지의 더 많은 정보를 공유해 주시면 추가로 분석해 드릴 수 있습니다. 😊


# cmake 빌드 로그
`[Makefile:146: all] Error 2` 오류는 빌드 과정에서 하나 이상의 오류가 발생했음을 나타내는 일반적인 메시지입니다. 이 오류는 특정 원인을 직접 알려주지는 않으며, 앞서 출력된 오류 메시지를 확인하는 것이 중요합니다.

### 오류 원인 확인 방법
1. **터미널에서 최근 로그 확인**  
   `cmake --build build -j 12` 실행 중에 출력된 로그를 위쪽으로 스크롤하면서, **실제 오류 메시지**가 어디에 있는지 확인하세요.  
   오류 메시지는 보통 다음과 같은 형식입니다.
   ```
   error: ‘XXX’ was not declared in this scope
   error: undefined reference to ‘YYY’
   fatal error: cannot find ‘ZZZ.h’
   ```

2. **CMake 빌드 로그 확인**  
   CMake는 `build` 디렉터리에 Makefile을 생성하므로, 빌드 중 발생한 오류를 기록한 로그 파일을 찾을 수도 있습니다.
   ```sh
   cd build
   make -j 12 2>&1 | tee build.log
   ```
   이후 `build.log` 파일을 열어 `error:` 또는 `fatal error:`를 검색해 보세요.

3. **CMake 다시 실행하여 의존성 확인**  
   CMake 설정에 문제가 있을 가능성도 있으므로, 기존 `build` 디렉터리를 삭제하고 다시 설정을 해보세요.
   ```sh
   rm -rf build
   mkdir build
   cd build
   cmake ..
   cmake --build . -j 12
   ```
   만약 여기서 CMake 설정 오류가 발생하면, 관련 오류 메시지를 확인하여 수정해야 합니다.

4. **Make 빌드 단계를 단계적으로 실행**  
   `-k` 옵션을 사용하여 오류가 발생해도 계속 빌드하도록 설정한 후, 더 많은 오류 메시지를 확인할 수도 있습니다.
   ```sh
   make -k -j 12
   ```
   또는 특정 타겟만 개별적으로 빌드해 보세요.
   ```sh
   make VERBOSE=1
   ```

5. **의존성 문제 확인**  
   만약 `undefined reference` 같은 링크 오류가 발생했다면, 라이브러리가 제대로 포함되지 않았을 가능성이 있습니다.
   - `CMakeLists.txt`에서 `target_link_libraries()`가 제대로 설정되었는지 확인하세요.
   - `ldd`를 사용하여 필요한 라이브러리가 제대로 연결되었는지 확인할 수도 있습니다.
     ```sh
     ldd build/your_executable
     ```

6. **GCC/Clang 컴파일러 버전 확인**  
   C++ 코드의 경우, 사용 중인 GCC/Clang 버전과 C++ 표준(`-std=c++17` 등)이 맞지 않을 수도 있습니다.
   ```sh
   g++ --version
   clang++ --version
   ```

위 방법들을 차례로 시도하면서 어디서 문제가 발생했는지 확인해 보시고, 특정 오류 메시지가 있으면 공유해 주시면 더 자세히 도와드릴 수 있습니다. 😊


# cmake 기본 사용 방법
### **CMake란?**
CMake는 **크로스 플랫폼 빌드 시스템**을 생성하는 도구로, 여러 플랫폼에서 C/C++ 프로젝트를 쉽게 빌드할 수 있도록 도와줍니다.  
기본적으로 CMake는 **Makefile**, **Ninja**, **Visual Studio 프로젝트 파일** 등을 생성하는 역할을 하며, 직접 컴파일하지 않고 빌드 시스템을 자동으로 설정해주는 역할을 합니다.

---

## **1. CMake 기본 사용 방법**
### **(1) 프로젝트 디렉터리 구조**
예제 프로젝트를 만든다고 가정해 보겠습니다.

```
my_project/
├── CMakeLists.txt       # CMake 설정 파일
├── src/
│   ├── main.cpp         # C++ 소스 코드
│   ├── math.cpp         # 추가 소스 코드
│   ├── math.h           # 헤더 파일
└── build/               # 빌드 디렉터리 (CMake 실행 후 생성됨)
```

---

## **2. CMakeLists.txt 작성**
CMake를 사용하려면 `CMakeLists.txt` 파일을 작성해야 합니다.

### **예제: C++ 프로젝트 CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 3.10)  # 최소 요구 CMake 버전
project(MyProject)                    # 프로젝트 이름 설정

set(CMAKE_CXX_STANDARD 17)             # C++17 표준 사용
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_executable(my_program src/main.cpp src/math.cpp)  # 실행 파일 생성
```

---

## **3. CMake 사용 예제**
### **(1) CMake 실행 및 빌드**
CMake는 **빌드 디렉터리를 따로 만들어서 실행**하는 것이 일반적입니다.

```sh
mkdir build        # 빌드 디렉터리 생성
cd build
cmake ..          # CMake 실행 (Makefile 생성)
cmake --build .   # 빌드 수행
```
이후 `build/` 폴더 안에 `my_program` 실행 파일이 생성됩니다.

---

### **(2) 실행 파일 실행**
빌드가 완료되면 실행 파일을 실행할 수 있습니다.
```sh
./my_program
```

---

## **4. CMake 고급 기능**
### **(1) 여러 소스 파일 자동 추가**
소스 파일이 많아질 경우, 직접 하나씩 나열하는 대신 `file(GLOB ...)`을 사용하면 편리합니다.
```cmake
file(GLOB SOURCES "src/*.cpp")   # src/ 폴더의 모든 .cpp 파일 가져오기
add_executable(my_program ${SOURCES})
```

### **(2) 라이브러리 추가**
만약 `math.cpp`를 별도의 라이브러리로 빌드하고 싶다면 다음과 같이 작성할 수 있습니다.

```cmake
add_library(math_lib src/math.cpp)   # 정적 라이브러리(math_lib) 생성
add_executable(my_program src/main.cpp)
target_link_libraries(my_program math_lib)  # 실행 파일에 라이브러리 링크
```

---

## **5. CMake의 주요 명령어 정리**
| 명령어 | 설명 |
|--------|------|
| `project(<name>)` | 프로젝트 이름 설정 |
| `cmake_minimum_required(VERSION X.Y)` | 최소 요구 CMake 버전 지정 |
| `add_executable(<name> <src>)` | 실행 파일 생성 |
| `add_library(<name> <src>)` | 라이브러리 생성 |
| `target_link_libraries(<target> <lib>)` | 실행 파일에 라이브러리 추가 |
| `set(CMAKE_CXX_STANDARD 17)` | C++ 표준 설정 |

---

## **6. CMake 디버깅 & 문제 해결**
### **(1) 빌드 로그 보기**
CMake 실행 시 자세한 로그를 보고 싶다면 `-DCMAKE_VERBOSE_MAKEFILE=ON` 옵션을 추가합니다.
```sh
cmake .. -DCMAKE_VERBOSE_MAKEFILE=ON
```

### **(2) 빌드 문제 해결**
- **Makefile 생성 오류**: `CMakeLists.txt`가 잘못되었을 가능성이 있음 → `cmake ..` 실행 로그 확인  
- **컴파일 오류**: `build` 디렉터리를 삭제 후 다시 빌드  
  ```sh
  rm -rf build && mkdir build && cd build
  cmake ..
  cmake --build .
  ```
- **라이브러리 링크 오류**: `target_link_libraries()` 설정 확인  

---

## **7. 결론**
CMake는 **C++ 프로젝트의 빌드 시스템을 자동으로 생성**해 주는 강력한 도구입니다.  
특히 **크로스 플랫폼 지원**, **의존성 관리**, **빌드 자동화** 등의 기능이 뛰어나므로, 규모가 큰 프로젝트에서 필수적으로 사용됩니다.

이제 CMake를 활용하여 빌드 시스템을 효율적으로 관리해 보세요! 🚀