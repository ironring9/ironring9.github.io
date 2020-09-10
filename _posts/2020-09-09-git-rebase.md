---
title: "Git Rebase"
categories: 
  - Git
toc: true
---

# Rebase하기

Git에서 브랜치를 다른 브랜치로 합치는 방법은 두 가지입니다.
- Merge
- Rebase

## Rebase와 Merge의 차이

![git-rebase-img001]({{site.url}}/assets/images/git-rebase-img001.png)

위와 같은 브랜치가 있다고 할 때 Merge로 합쳤을 때와 Rebase로 합쳤을 때를 알아보겠습니다.

### Git Merge 결과

![git-rebase-img002]({{site.url}}/assets/images/git-rebase-img002.png)

merge 명령을 사용하면 3-way Merge로 새로운 커밋을 만들어냅니다.

### Git Rebase 결과

**rebase** 명령은 아래와 같은 과정으로 진행됩니다.

![git-rebase-img003]({{site.url}}/assets/images/git-rebase-img003.png)

1. 두 브랜치가 나뉘기 전인 공통 커밋으로 이동한다.
2. 이동한 커밋부터 지금 Checkout한 브랜치가 가리키는 커밋까지 diff를 차례로 만들어 어딘가에 임시로 저장해 놓는다.
3. Rebase할 브랜치(**experiment**)가 합칠 브랜치의 커밋(**C4'**)을 가리키게 하고 아까 저장해 놓았던 변경사항을 차례대로 적용한다.

![git-rebase-img004]({{site.url}}/assets/images/git-rebase-img004.png)

마지막으로 master 브랜치를 Fast-forward 합니다.

병합된 브랜치 커밋을 기준으로 보면 사실상 merge 명령과 결과적으로 다른게 없습니다.
하지만 rebase가 좀 더 깨끗한 히스토리를 만듭니다.
**Rebase한 브랜치의 Log를 살펴보면 히스토리가 선형입니다.**

# Rebase 활용

![git-rebase-img005]({{site.url}}/assets/images/git-rebase-img005.png)

한 가지 예를 들어서 Rebase 활용을 설명해보겠습니다.

위 사진에서 라이브 버전인 master 브랜치, 서버 기능을 추가하는 server 브랜치, 클라이언트에 기능을 추가하는 client 브랜치가 있다고 가정하고
server, client 브랜치를 라이브 버전에 합쳐보겠습니다.

우선 개발이 완료된 client 브랜치는 우선 master 브랜치와 합쳐보겠습니다.
server와는 아무 관련이 없는 client 커밋은 C8, C9입니다. 

이 두 커밋을 master 브랜치에 적용하기 위해서는 -onto 옵션을 사용하여 아래와 같은 명령을 실행합니다.

```
$ git rebase --onto master server client
```

![git-rebase-img006]({{site.url}}/assets/images/git-rebase-img006.png)

이 명령은 master 브랜치로부터 server 브랜치와 client 브랜치의 공통 조상까지의 커밋을 client 브랜치에서 없애고 싶을 때 사용합니다.
client 브랜치에서만 변경된 내용을 임시저장소에 저장하여 master 브랜치에 반영합니다.

이제 master 브랜치로 돌아가서 Fast-forward 시키겠습니다.

```
$ git checkout master
$ git merge client
```

![git-rebase-img007]({{site.url}}/assets/images/git-rebase-img007.png)

server 브랜치의 일이 다 끝나면 git rebase [basebranch] [topicbranh] 라는 명령으로 Checkout 하지 않고 바로 server 브랜치를 master 브랜치로 Rebase 할 수 있습니다.

```
$ git rebase master server
```

![git-rebase-img008]({{site.url}}/assets/images/git-rebase-img008.png)

그리고 나서 master 브랜치를 fast-forward 시킵니다.

```
$ git checkout master
$ git merge server
$ git branch -d client
$ git branch -d server
```

![git-rebase-img009]({{site.url}}/assets/images/git-rebase-img009.png)


# Rebase의 위험성

Rebase가 장점이 많은 기능이지만 단점이 없는 것은 아니니 조심해야합니다. 그 주의사항은 아래 한 문장으로 표현할 수 있습니다.


## 이미 공개 저장소에 Push 한 커밋을 Rebase 하지 마라

Rebase는 기존 커밋을 그대로 사용하는 것이 아니라 내용은 같지만 다른 커밋을 새로 만듭니다.  
새 커밋을 서버에 Push하고 동료 중 누군가가 그 커밋을 Pull 해서 작업을 한다고 해보겠습니다.  
그런데 그 커밋을 git rebase 로 바꿔서 Push 해버리면 동료가 다시 Push 했을 때 동료는 다시 Merge 
해야합니다.  
그리고 동료가 다시 Merge 한 내용을 Pull 하면 내 코드는 정말 엉망이 됩니다.

![git-rebase-img010]({{site.url}}/assets/images/git-rebase-img010.png)

이제 팀원 중 누군가 커밋, Merge 하고 나서 서버에 Push 합니다. 이 리모트 브랜치를 Fetch, Merge 하면 히스토리는 아래와 같이 됩니다.

![git-rebase-img011]({{site.url}}/assets/images/git-rebase-img011.png)

그런데 Push 했던 팀원은 Merge 한 일을 되돌리고 다시 Rebase 합니다. 서버의 히스토리를 새로 덮어씌우려면 git push —force 명령을 사용해야합니다. 이후에 저장소에서 Fetch 하고 나면 아래 그림과 같은 상태가 됩니다.

![git-rebase-img012]({{site.url}}/assets/images/git-rebase-img012.png)

한 팀원이 다른 팀원이 의존하는 커밋을 없애고 Rebase 한 커밋을 다시 Push함
git pull 로 서버의 내용을 가져와서 Merge 하면 같은 내용의 수정사항을 포함한 Merge 커밋이 아래와 같이 만들어집니다.

![git-rebase-img013]({{site.url}}/assets/images/git-rebase-img013.png)

git log 로 히스토리를 확인해보면 저자, 날짜 메시지가 같은 커밋이 두 개 있습니다. (C4,C4') 이렇게 되면 혼란스럽습니다.  
게다가 이 히스토리를 서버에 Push 하면 같은 커밋이 두 개 있기 때문에 다른 사람들도 혼란스러워 합니다.  
'C4', 'C6' 는 포함되지 말았어야 할 커밋입니다.  
애초에 서버로 데이터를 보내기 전에 Rebase로 커밋을 정리했어야 했습니다.

## Rebase 한 것을 다시 Rebase 하기

만약 이런 상황에 빠질 때 유용한 Git 기능이 하나 있습니다. 어떤 팀원이 강제로 내가 한일을 덮어썼다고 해봅시다.  
그러면 내가 했던 일이 무엇이고 덮어쓴 내용이 무엇인지 알아내야합니다.  
커밋 SHA 체크섬 외에도 Git은 커밋에 Patch 할 내용으로 SHA-1 체크섬을 한번 더 구합니다.  
이 값은 patch-id 라고 합니다.

예를 들어 앞서 살펴본 예제를 보면 **한 팀원이 다른 팀원이 의존하는 커밋을 없애고 Rebase 한 커밋을 다시 Push 함** 상황에서 Merge 하는 대신 git rebase teamone/master 명령을 실행하면 Git은 아래와 같은 작업을 합니다.

  1. 현재 브랜치에만 포함된 커밋을 찾는다. (C2, C3, C4, C6, C7)
  2. Merge 커밋을 가려낸다. (C2, C3, C4)
  3. 이 중 덮어쓰지 않은 커밋들만 골라낸다. (C2, C3, C4는 C4'와 동일한 Patch다.)
  4. 남은 커밋들만 다시 teamone/master 바탕으로 커밋을 쌓는다.
결과를 확인해 보면 같은 Merge를 다시 한다 같은 결과 대신 제대로 정리된 강제로 덮어쓴 브랜치에 Rebase하기 같은 결과를 얻을 수 있습니다.

![git-rebase-img014]({{site.url}}/assets/images/git-rebase-img014.png)

# Rebase vs. Merge

Merge가 뭔지, Rebase가 뭔지 여러 예제를 통해 간단히 살펴보았습니다.  
**그럼 도대체 둘 중 어떤걸 쓰는게 좋을까요?**  
이 지문에 대한 답을 찾기 전에 히스토리의 의미에 대해서 짚어보겠습니다.

히스토리를 보는 관점 중에 하나는 **작업한 내용의 기록**으로 보는 것입니다.  
이 관점으로 보았을 때 Merge는 지저분하게 수많은 커밋을 남기게되고 Rebase는 깔끔한 커밋을 남게 합니다.

현재 제가 다니는 회사에서도 Merge는 사용하지 않고 Rebase를 사용하여 커밋을 최대한 깔끔히 남도록 유지 중입니다. 항상 Rebase가 옳은 것은 아니겠지만 미약한 제 경험상으로는 Rebase를 통해 커밋을 깔끔하게 유지하는게 버전 관리나 협업에 도움이 되는것 같습니다.