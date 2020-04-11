---
layout: post
title:	"VTK 설치하기 (Ubuntu)"
date: 2020-04-11 14:20:00 +0900
category: VTK
tags: [VTK, 영상처리, Image Processing]
---

Ubuntu(18.04)에서 VTK를 설치하는 방법을 정리하였습니다.  OS를 리눅스로 정했기 때문에, QT를 함께 사용하여 디스플레이 하도록 하겠습니다.



###### CMake 설치

먼저 ccmake(Curses based GUI for CMake)를 설치합니다. cmake는 사용하고 있는 운영체제에 맞게 빌드할 수 있도록 makefile 또는 Visual studio 솔루션 파일 등을 생성해 줍니다.

> sudo apt install cmake-curses-gui

​     

###### QT 설치

QT를 설치합니다. 저는 Stable version인 5.12.7을 사용하도록 하겠습니다. git이나 online 설치파일은 다운로드 속도가 너무 느려서, offline 설치파일(1.3GB)을 다운로드해서 진행하였습니다. 왠만하면 설치파일로 진행하는게 편하고 깔끔한 것 같아요. 

QT를 설치하고 PATH에 qt 경로를 추가합니다.

> vim ~/.bashrc
>
> export PATH=/home/userid/Qt5.12.7/5.12.7/gcc_64/bin:/home/userid/Qt5.12.7/Tools/QtCreator/bin:~~~
>
> source ~/.bashrc	// 환경설정 적용

​     

###### VTK 설치

VTK를 다운받습니다. 역시 git에서 소스코드를 받아도 되지만, 저는 stable version을 선호하기 때문에, [VTK](https://vtk.org)에 접속해서 Latest Release의 Source 압축 파일을 다운받았습니다. 버전은 8.2.0입니다. 압축파일을 아래와 같이 풉니다.

> mkdir $HOME/projects/VTK
>
> tar zxvf VTK-8.2.0.tar.gz -C $HOME/projects/VTK

​     

build 디렉토리를 하나 만들고 진입합니다.

> mkdir $HOME/projects/VTK/VTK-build
>
> cd mkdir $HOME/projects/VTK/VTK-build

​     

cmake를 실행합니다.

> ccmake $HOME/projects/VTK/VTK-8.2.0

'**c**'를 눌러서 configuration을 합니다. 빌드 설정을 만들어주는 것이고, 끝나면 아래와 같이 뜹니다. 

![](\assets\cmake.png)

​    

빌드 옵션이 정말 많은데, '**t**'를 누르면 advanced mode로 바뀌면서 더 많이 나옵니다. 몇 개만 언급해 보겠습니다.

- BUILD_SHARED_LIBS - ON
- BUILD_TESTING - OFF
- CMAKE_BUILD_TYPE - Debug / Release / RelWithDebInfo 중 택 1
- CMAKE_INSTALL_PREFIX - make install 실행 시, 라이브러리가 설치되는 곳.
- VTK_Group_Qt - ON

​     

다시 '**c**'를 눌러서 configuration을 해봅니다. QT 경로가 정상적으로 잡혀있으면 QT Path가 셋팅됩니다. '**g**'(Generation)를 눌러 최종 Makefile을 생성합니다. 



드디어 **make**를 입력해서 빌드를 합니다. 빌드가 성공하면! (sudo) **make install**을 입력해서 빌드 결과물을 'CMAKE_INSTALL_PREFIX' 위치에 복사합니다. 

​     