---
layout: post
title:  "[Git] 깃허브에 대용량 파일 올리기"
subtitle:   "git LFS란 무엇인가?"
date: 2021-07-30 17:20:10 +0900
categories: study
tags: git
comments: true
---
# Git Large File Storage
### 프로젝트를 정리 하다가

<br/>

블로그를 만들고 포스트 할 내용들을 정리하기 위해 로컬 PC에 있는 프로젝트들을 깃허브에 정리하면서, 이전에 했던 프로젝트들.. 그 중에서도 결과물에 대해 설명하는 영상을 업로드 하는 과정에서 문제가 생겼다.

<br/>

처음엔 뭔가 뜬게 많아 로그는 읽지도 않고 습관처럼 exit을 친 후 깃헙에 들어가 봤는데 웬 걸, 수정된 사항이 전혀 반영되지 않았다. 뭐지? git status를 해도 문제가 없었다. 혹시나 해서 git push origin master를 해보니 다시 commit 한 파일들이 올라간다. 근데.. 되게 느리다.

<br/>

![image-20210730172318407](/assets/img/study/git/image-20210730172318407.png)

<br/>

에러였다. Large files detected. 용량이 큰 파일이 탐지되었단다. 찾아보니 파일의 용량이 50메가가 넘어가면 경고가 뜨고, 100메가 이상의 파일은 에러가 뜨며 업로드가 실패한다고 한다.

<br/>

**"You may want to try Git Large File Storage - https://git-lfs.github.com."**

<br/>

오호, 링크를 타고 들어가보니 git large file storage라는 프로그램이 있다.

<br/>

<br/>

![image-20210730173047388](/assets/img/study/git/image-20210730173047388.png)

<br/>

<br/>

### git lfs 사용 방법

<br/>

사용 방법은 간단하다. 프로그램을 다운받아 cmd 창을 켠 뒤, 아래 명령어를 순차적으로 입력하면 된다.

<br/>

![image-20210730173241936](/assets/img/study/git/image-20210730173241936.png)

<br/>

```bash
git lfs install

git lfs track "*.확장자"

git add .gitattributes
```

<br/>

그 후 올릴 파일을 add, commit 한 뒤 push 하면 끝. 알아서 lfs를 이용해 큰 용량의 파일을 변환(?)하며 업로드 된다. 그냥 다운받아 install로 설치와 gitattributes 파일만 올려주면 알아서 사용이 되는 것!