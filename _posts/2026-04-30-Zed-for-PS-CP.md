---
title: "Zed 에디터로 PS/CP를 즐기기"

category: "팁과 정보"
tags: ["zed", "c++", "cpp", "wsl", "ps", "cp"]
---

이 글에서는 다음과 같은 내용을 다룹니다.

- 윈도우에서 WSL을 통한 PS/CP 환결 설정
- 윈도우에서 Zed를 사용한 C++ 컴파일 설정

## 다양한 IDE 및 에디터
윈도우 환경에서 PS/CP를 즐길 때 주로 아래 에디터 혹은 IDE를 사용합니다. 에디터와 IDE의 차이점은 컴파일러와 빌드 환경의 제공 여부입니다. IDE는 컴파일러와 빌드환경을 제공하고 에디터의 경우에는 그렇지 않습니다.

- Visual Studio (IDE)
- Code::Blocks (IDE)
- Dev-C++ (IDE)
- Visual Studio Code (Editor)

추천하지 않습니다. Visual Studio는 개발 환경을 구성하기 쉽고 강력한 디버깅 기능을 제공하며 윈도우즈 앱을 개발한다면 표준적인 IDE입니다. 다만, PS/CP에서 사용하기에는 너무 방대한 기능과 많은 리소스를 사용합니다. 즉, 너무 IDE가 너무 무겁습니다.

초보자에게 추천합니다. Code::Blocks와 Dev-C++에 경우에는 설치 후 즉시 사용 가능이라는 간편함을 제공하여 초보자에게 추천되지만 현대적이지 못한 UI, 부족할 수도 있는 기능, 불안정한 유지개발, 버전에 따라 최신 C++ 기능을 사용하지 못하는 문제가 존재합니다.

중급자 이상에게 추천합니다. Visual Studio Code는 Electron 기반으로 제작된 앱으로써 가볍고 확장성이 뛰어나며 다양한 커스터마이징을 제공합니다. CPH-Helper와 같이 PS/CP에 유용한 확장이 존재합니다. 반면, 초기 설정이 필요하고 많은 확장을 설치할 경우 느려질 수 있습니다. PS/CP를 위해 사용할 경우 AI 기능을 비활성화하기 번거로울 수 있습니다.

### Zed를 추천하는 이유
이 글에서 소개할 Zed는 완전 Rust로 개발되어 빠른 속도와 쾌적한 환경을 제공하는 에디터입니다. 지속적인 유지개발과 업데이트가 계속되고 있으며 AI 기능을 제공하면서도 쉽게 On-Off 할 수 있는 장점이 있습니다. 다만 아직 개발 중인 에디터로 미완성 기능이나 일부 버그가 있을 수 있으며 초기 설정이 필요합니다. 빠른 속도와 쾌적한 개발 환경을 원하신다면 Zed를 추천합니다.

## 다양한 빌드 환경
윈도우 환경에서 C++를 컴파일, 빌드하기 위해서는 아래와 같은 환경을 사용합니다.
- MSVC (Visual Studio에서 사용)
- MinGW (Code::Blocks, Dev-C++에서 사용)
- MSYS2
- WSL (Windows Subsystem for Linux)

MSVC는 Visual Studio에서 사용하는 C++ 빌드 도구로 마이크로소프트에서 개발하였습니다. Visual Studio을 실치하여 C++로 개발할 경우 MSVC를 사용하게됩니다.

MinGW, MSYS2은 윈도우 환경에서 Linux 개발 도구를 사용할 수 있게 해주는 개발 도구 모음입니다.

### PS에서 WSL 사용을 추천하는 이유
WSL은 Windows Subsystem for Linux의 약자로 윈도우 환경에서 Linux 환경을 제공합니다. WSL을 사용하면 아래와 같은 이점을 얻을 수 있습니다.

- 채점 서버와 비슷한 환경 구성
- 컴파일러 등 다양한 개발 도구의 쉬운 설치
- Linux CLI의 명령어 사용
- Visual Studio Code, Zed와 높은 연동성

다만, WSL을 원활하게 사용하기 위해서는 Linux에 대한 이해가 필요합니다.

## WSL을 설치하는 방법
WSL을 설치하는 방법은 간단합니다. **관리자** 모드에서 PowerShell을 열고 `wsl --install`을 입력한 후 재부팅하면 설치할 수 있습니다.

```powershell
wsl --install
```

재부팅 후 wsl을 실행하여 아이디와 비밀번호를 입력해 프로필을 생성합니다.

아래를 입력하여 패키지를 업데이트합니다.
```bash
sudo apt update && sudo apt -y upgrade && sudo apt -y autoremove
```

아래를 입력하여 개발 도구와 GDB를 설치합니다.
```bash
sudo apt-get install build-essential gdb -y
```

WSL 설치가 완료되었습니다.

## Zed 설치 및 설정
[Zed 홈페이지](https://zed.dev/)에서 Zed를 설치합니다.

wsl을 실행하여 코드를 작성할 폴더를 생성합니다.
```bash
mkdir {원하는 폴더명 ex. baekjoon}
```

zed를 실행합니다.
```bash
zed {폴더명}
```

### 환경설정
zed가 실행되었다면 `ctrl+shift+p`를 눌러 명령어 팔레트를 열어 `zed: open tasks`을 검색합니다. 아래 내용을 대괄호 안에 추가합니다.

```json
{
  "label": "C++: Build active file",
  "command": "g++",
  "args": [
    "-std=gnu++20",
    "-O2",
    "-Wall",
    "-Wextra",
    "-o",
    "$ZED_DIRNAME/$ZED_STEM",
    "$ZED_FILE"
  ],
  "cwd": "$ZED_DIRNAME",
  "use_new_terminal": false,
  "allow_concurrent_runs": false,
  "reveal": "always"
},
{
  "label": "C++: Build active file (Debug)",
  "command": "g++",
  "args": [
    "-std=gnu++20",
    "-O0",
    "-g",
    "-Wall",
    "-Wextra",
    "-o",
    "$ZED_DIRNAME/$ZED_STEM",
    "$ZED_FILE"
  ],
  "cwd": "$ZED_DIRNAME",
  "use_new_terminal": false,
  "allow_concurrent_runs": false,
  "reveal": "always"
},
{
  "label": "C++: Run active file",
  "command": "$ZED_DIRNAME/$ZED_STEM",
  "cwd": "$ZED_DIRNAME",
  "use_new_terminal": false,
  "allow_concurrent_runs": false,
  "reveal": "always"
}
```

`ctrl+shift+p`를 눌러 명령어 팔레트를 열어 `zed: open debug tasks`을 검색합니다. 아래 내용을 대괄호 안에 추가합니다.
```json
{
  "label": "Debug active C++ file",
  "adapter": "CodeLLDB",
  "request": "launch",
  "build": {
    "command": "g++",
    "args": [
      "-std=gnu++20",
      "-O0",
      "-g",
      "-Wall",
      "-Wextra",
      "-o",
      "$ZED_DIRNAME/$ZED_STEM",
      "$ZED_FILE"
    ],
    "cwd": "$ZED_DIRNAME"
  },
  "program": "$ZED_DIRNAME/$ZED_STEM",
  "cwd": "$ZED_DIRNAME"
}
```

`ctrl+shift+p`를 눌러 명령어 팔레트를 열어 `zed: open keymap`을 검색합니다. 아래 내용을 대괄호 안에 추가합니다.

```json
{
  "context": "Workspace",
  "bindings": {
    // "shift shift": "file_finder::Toggle"
    "shift-b shift-b": ["task::Spawn", {"task_name": "C++: Build active file (Debug)"}],
    "shift-r shift-r": ["task::Spawn", {"task_name": "C++: Run active file"}],
  },
}
```

### 필수 옵션

`ctrl+,`을 입력하여 설정에 들어가 `auto save`를 검색합니다. `Auto Save Mode`를 `On Focus Change`혹은 선호하는 설정으로 변경합니다.

## 테스트

`ctrl-n`을 활용해 `main.cpp`파일을 생성하여 아래를 입력합니다.

```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    cout << "Hello, Zed\n";
}
```

만약 한글 입력이라면 실행이 안 될 수 있습니다.
`shift+b`를 연속해서 2번 눌러 컴파일을 할 수 있습니다.
`shift+r`를 연속해서 2번 눌러 실행을 할 수 있습니다. 컴파일 후에 실행이 가능합니다.

`F4` 혹은 `F5`을 눌러 `Debug active C++ File`을 실행합니다. Zed는 자동으로 CodeLLDB를 설치할 것입니다.

## 결론

WSL과 Zed의 설치와 설정이 완료되었습니다. Zed의 빠른 속도를 즐기세요.
