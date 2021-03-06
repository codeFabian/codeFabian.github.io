---
layout: post
title: React-native AndroidStudio 초기설정
subtitle: RN, AndroidStudio
type: "Devlog"
createDate: 2021-01-03
devlog: true
text: true
author: "codeFabian"
post-header: false
header-img: ""
order: 1
---

<img src="https://user-images.githubusercontent.com/46562138/103461753-0a4fc380-4d64-11eb-84ca-16a11c51ba1a.png" width="500" height="300"/>

## 시작

회사에서 퇴근 후에 사이드프로젝트를 진행하고 있다.  
React-native 를 사용하여 모바일 애플리케이션을 만들 예정이며, 구글 플레이스토어에 배포를 할 예정이다.

개발OS 는 MAC이고, 안드로이드 용을 개발할 예정이기 때문에 안드로이드스튜디오를 다운받아야 했다.

[안드로이드 스튜디오 설치](https://developer.android.com/studio)

코드스테이츠 수강생으로 Final Project 에서 React-native 를 사용하여 X-code 개발환경을 통해 IOS용 앱 개발 경험이 있었기 때문에, 이번에는 안드로이드 용으로 개발하고자 하였다.

안드로이드 스튜디오 개발환경을 처음 경험했기 때문에, **JDK 설치** , **SDK 설정** , **Android_HOME환경변수구성** 등에 대해 아는 바가 없었고, 이 부분으로 인해 상당한 삽질을 경험하게 되었다.  
3시간의 삽질을 끝으로 간신히 애뮬레이터에서 앱이 실행되는 것을 확인할 수 있었는데, 사실 다시한번 **"공식문서를 제대로 읽자"** 라고 리마인드를 한 하루이기도 하였다.

## 초기세팅 방법

터미널에서 `Homebrew` 를 사용하여 아래의 내용들을 설치합니다.

**Node & Watchman**  
<code>brew install node</code>  
<code>brew install watchman</code>

**Java Development Kit**  
`brew install --cask adoptopenjdk/openjdk/adoptopenjdk8`

---

다음은 **안드로이드 스튜디오 SDK 설정** 입니다.

안드로이드 스튜디오를 실행시킨 후, 오른쪽 하단을 보면 configure 부분을 클릭하고 SDK manager 을 엽니다.  
SDK Platforms 에서 오른쪽 하단의 **show Package Details** 를 선택한 뒤에 다음 리스트를 찾아 선택합니다.

- Android SDK Platform 28
- Intel x86 Atom System Image
- Google APIs Intel x86 Atom System Image
- Google APIs Intel x86 Atom_64 System Image

전부 선택하였다면 오른쪽 하단의 OK 를 눌러 선택한 내용을 설치합니다.

---

다음은 안드로이드 **스튜디오 환경변수 설정** 입니다.  
환경변수 추가를 위해 `~./bash_profile` 파일을 열어 아래와 같이 수정합니다.  
수정할 때는 `vi ~./bash_profile` 를 입력하고 `i` 를 통해 수정을 시작합니다. 또한 수정이 완료된 후에는 `esc` 를 누른 뒤에 `:wq` 를 통해 저장합니다.

자 그렇다면 수정할 부분입니다.

```
export ANDROID_HOME=자신의 안드로이드SDK 위치/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

자신의 안드로이드 SDK위치를 모를 경우에는, 안드로이드 스튜디오 SDK 설정화면에서 찾을 수 있습니다.

수정을 마치고, 저장을 한 뒤에는 다음 명령어를 실행합니다.  
`source ~/.bash_profile`

이렇게 환경변수를 마친 뒤에 터미널을 실행시켜 `adb` 를 입력하여 SDK가 잘 설정되었다면 아래와 유사한 결과를 확인할 수 있습니다. (버전의 차이가 있습니다)

```
Android Debug Bridge version 1.0.41
Version 29.0.1-5644136
Installed as /자신의 안드로이드SDK 위치/platform-tools/adb
```

이렇게 완료를 한 뒤에는 정상적으로 에뮬레이터가 실행되는 것을 확인할 수 있습니다.

<img src="https://user-images.githubusercontent.com/46562138/103463100-3ae82b00-4d6d-11eb-9ead-018abb489fb0.png" width="300" height="500"/>

---

## 느낀점

사실 이 과정을 3시간을 넘게 삽질을 하였다. 에러코드에 대해서 직접 구글링과 스택오버플로우를 넘나들며, SDK 설정과 환경변수 설정을 해야한다는 사실을 깨달았다.  
모든 것을 해결하고, 애뮬레이터가 실행 됐을 때는 그 어느때보다 기뻤다.. 하지만 공식문서에서 component 에 대한 내용을 확인하고자 들어갔다가 교훈을 얻게되었다..

[react-native공식문서 setting](https://reactnative.dev/docs/environment-setup)  
아니... 이렇게 친절하게 안드로이드 스튜디오 환경에서의 세팅이 있었던 것이다.. 사실 이렇게 삽질하기 전에도 공식문서를 먼저 보긴했지만 찾지못하였다.  
다시 한번 공식문서를 꼼꼼하게 잘 읽어보자 + 역시 RN 공식문서는 친절하게 되어있다. 라는 것을 몸소 깨닫게 되었다.

워낙 긴 삽질과정을 통해 해결방법을 알아냈기 때문에, devlog 작성을 하였고, 앞으로도 기록을 잘하도록 노력할 것이다.

## etc

typescript 를 사용할 것이기 때문에 init 당시에는  
`npx react-native init [프로젝트명] --template react-native-template-typescript ` 을 사용하였고,  
`npx react-native run-android` 는 너무 길기 때문에 package.json 을 수정하여 `npm run android` 를 사용한다.
