---
layout: default
title: expo
parent: Setting
nav_order: 3
---

# expo 세팅
{: .no_toc .fw-500 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## expo 설치

expo 를 처음부터 세팅하려면 많은 세팅이 필요합니다.  
create-expo-app은 이부분을 해결하고 간편하게 expo를 바로 시작할 수 있습니다. 

```bash
# expo  설치한다.
# expo 앱을 생성한다. 
# Ios, Android 외에 Web을 사용하기 위한 설치

$ npm install -g expo-cli
$ npx create-expo-app expo_app
$ npx expo install react-dom react-native-web @expo/webpack-config
```
```bash
$ npm run ios
$ npm run android
$ npm run web
```
