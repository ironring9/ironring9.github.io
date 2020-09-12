---
title: "Git Cherry-pick"
categories: 
  - Git
toc: true
---

# Rebase와 Cherry-Pick 워크플로

히스토리를 한 줄로 관리하려고 Merge보다 Rebase나 Cherry-Pick을 더 선호하는 관리자들도 있습니다. ~~제가 다니고 있는 회사가 그렇죠~~

한 브랜치에서 다른 브랜치로 작업한 내용을 옮기는 방식 중 Cherry-pick이라는 것이 있습니다.  
Cherry-pick은 커밋 하나만 Rebase하는 것을 말합니다. Cherry-pick은 브랜치에 있는 커밋 중에서 하나만 고르거나 브랜치에 커밋이 하나밖에 없을 때 Rebase 보다 유용합니다.

![git-cherry-pick-img001]({{site.url}}/assets/images/git-cherry-pick-img001.png)

위와 같은 저장소가 있을 때, **e43a6** 커밋 하나만 **master** 브랜치에 적용하려면 아래와 같은 명령을 실행합니다.

```
$ git cherry-pick e43a6
```
위 명령을 실행하면 **e43a6** 커밋에서 변경된 내용을 현재 브랜치에 똑같이 적용합니다. 하지만 변경을 적용한 시점이 다르므로 새 커밋의 SHA-1 해시값은 달라집니다. 명령 실행 결과는 아래와 같습니다.

![git-cherry-pick-img002]({{site.url}}/assets/images/git-cherry-pick-img002.png)