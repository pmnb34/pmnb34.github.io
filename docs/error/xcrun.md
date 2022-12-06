---
layout: default
title: Xcrun
parent: Error
nav_order: 1
---

# Mac 개발자도구 오류 (git clone 안될때)
{: .no_toc .fw-500 }


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## xcrun: error 해결방법

Mac 에서 최신버전으로 업데이트후 git clone을 할 경우 높은 확률로 xcrun 에러가 출력된다.  
해당 오류는 CommandLineTools 를 인식하지 못해 발생되므로 간단하게 해결해주면된다.  

```bash
$ xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools),  
missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

> 해결방법은 간단하다.  
> 터미널에 하단 명렁어를 입력하면 된다.  
> Xcode 설치 없이 Command Line Tools 를 사용할수 있게 된다.  

```bash
$ xcode-select --install
```