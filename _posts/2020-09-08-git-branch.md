---
title: "Git Branch"
categories: 
  - Git
toc: true
---

코드를 통째로 복사하고 나서 원래 코드와 상과없이 독립적으로 개발을 진행할 수 있는데, 이렇게 독립적으로 개발하는 것을 브랜치라고 합니다.

# 브랜치란 무엇인가

커밋을 하게되면 Git은 현 Staging Area에 있는 데이터의 스냅샷에 대한 포인터, 저자나 커밋 메시지 같은 메타데이터, 이전 커밋에 대한 포인터 등을 포함하는 커밋 객체(커밋 Object)를 저장합니다.

이전 커밋에 대한 포인터가 있어서 현재 커밋이 무엇을 기준으로 바뀌었는지 알수 있습니다.

**Git의 브랜치는 커밋 사이를 가볍게 이동할 수 있는 포인터 같은 것을 말합니다.**



![git-branch-img000]({{site.url}}/assets/images/git-branch-img000.png)

master 브랜치는 git init 명령으로 초기화 할 때 자동으로 생성됩니다.

# 새 브랜치 생성하기

```
$ git branch testing
```

새로만든 브랜치는 현재 커밋을 가리킵니다.

![git-branch-img001]({{site.url}}/assets/images/git-branch-img001.png)

Git은 'HEAD'라는 특수한 포인터를 통해 지금 작업중인 브랜치가 무엇인지 파악합니다.
위 사진을 보면 testing이라는 브랜치는 생성됐지만 여전히 작업브랜치는 master입니다.

# 브랜치 이동하기

```
$ git checkout testing
```

위 명령을 실행하면 아래 그림과 같이 HEAD가 가리키는 브랜치. 즉, 작업브랜치가 testing으로 변경됩니다.

![git-branch-img002]({{site.url}}/assets/images/git-branch-img002.png)

```
$ vim test.rb
$ git commit -a -m 'made a change'
```
커밋으 할 경우 testing 브랜치는 새로운 커밋을 가리키고
master는 여전히 f30ab 커밋을 가리킵니다.


![git-branch-img003]({{site.url}}/assets/images/git-branch-img003.png)

```
$ git checkout master
```

HEAD는 master를 가리키게 됩니다.
위 명령이 한 일은 두 가지 입니다.
1. HEAD는 master를 가리키게 함
2. working directory의 파일도 해당 브랜치 버전으로 변경

![git-branch-img004]({{site.url}}/assets/images/git-branch-img004.png)

```
$ git vim test.rb
$ git commit -a -m 'made other changes'
```

새로운 커밋을 생성할 경우 testing 브랜치와 갈라지게 됩니다.

![git-branch-img005]({{site.url}}/assets/images/git-branch-img005.png)

# 브랜치 관리

```
$ git brach
  iss53
* master
  testing
```

* 기호가 붙어 있는 master 브랜치는 현재 Checkout해서 작업하는 브랜치를 나타냅니다.

```
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```

-v 옵션은 마지막 커밋의 메시지도 함께 보여줍니다.

```
$ git branch --merged
  iss53
* master
```

—merged 옵션은 현재 브랜치 기준으로 merge 된 브랜치들을 보여줍니다.
* 기호가 붙어있지 않은 브랜치는 삭제해도 되는 브랜치입니다.

```
$ git branch --no-merged
  testing
```

반대로 현재 checkout한 브랜치에 merge하지 않은 브랜치를 살펴보려면 위 옵션을 사용하면 됩니다.

**브랜치 이름 변경**
```
$ git branch -m [old_branch_name] [new_branch_name]
```

**브랜치 삭제**
```
git branch -d [branch_name]
```