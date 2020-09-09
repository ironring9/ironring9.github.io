---
title: "Mac backtick 설정하기"
categories: 
  - Mac
toc: true
---

# backtick 설정 ##

윈도우에서는 키보드의 숫자키 1 옆에 \`(backtick)이 있지만 
mac은 ₩(원화)표시가 타이핑 됩니다.
\`(backtick)은 코드나 md파일에도 자주 사용되므로 ₩(원화) -> \`(backtick) 로 바꿔보겠습니다.

# 설정방법 ##

```
cd ~/Library/
```
~/Library 폴더로 이동합니다.

```
mkdir KeyBindings
```
KeyBindings 폴더를 생성합니다.

```
cd KeyBindings
```
KeyBindings 폴더로 이동합니다.

```
vi DefaultkeyBinding.dict
```
vi 편집기로 DefaultkeyBinding.dict 파일 생성 및 편집

```
{
    "₩" = ("insertText:", "`");
}
```
위 내용을 작성하고 mac 재시동하면 성공!!!
