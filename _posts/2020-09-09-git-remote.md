---
title: "Git Remote"
categories: 
  - Git
toc: true
---

## Remote Branch

리모트 브랜치는 단순히 리모트 서버에 있는 브랜치를 의미합니다.

리모트 브랜치의 이름은 (remote)/(branch) 형식으로 되어 있습니다.
예를 들어 리모트 저장소 origin의 master 브랜치를 보고 싶다면 origin/master라는 이름으로 브랜치를 확인하면 됩니다.

**Check**
*브랜치 이름으로 많이 사용하는 "master", "origin"이라는 이름은 단순히 git 자동으로 생성해주는 브랜치 이름일 뿐 별 다른 의미가 없습니다.*

![git-remote-img001]({{site.url}}/assets/images/git-remote-img001.png)

### Remote Branch 활용하기

로컬 저장소에서 어떤 작업을 하고 있는데 동시에 다른 팀원이 [git.ourcompany.com](http://git.ourcompany.com) 서버에 Push하고 master 브랜치를 업데이트합니다. 그러면 이제 팀원간의 히스토리는 서로 달라집니다.

이 때 서버 저장소로부터 어떤 데이터도 주고받지 않기 때문에 로컬에서 확인할 때 origin/master 포인터는 그대로입니다.

![git-remote-img002]({{site.url}}/assets/images/git-remote-img002.png)


리모트 서버로부터 저장소 정보를 동기화 화려면 **git fetch origin** 명령을 사용합니다.
명령을 실행하면 우선 'origin' 서버의 주소 정보를 찾아서, 현재 로컬의 저장소가 갖고있지 않은 새로운 정보가 있으면 모두 내려받고, 받은 데이터를 로컬 저장소에 업데이트하고 나서 origin/master 포인터의 최신 커밋으로 이동시킵니다.

![git-remote-img003]({{site.url}}/assets/images/git-remote-img003.png)

### Push하기

로컬의 브랜치를 서버로 전송하기 위해 push 명령어를 사용합니다.

```
$ git push origin serverfix
```

serverfix 브랜치를 origin 리모트에 push합니다.

### Pull하기

pull 명령은 사실상 fetch한 후에 merge를 자동으로 해준것이라 할수 있습니다.

```
$ git pull 
```

### 브랜치 추적하기(Upstream Branch)

리모트 트래킹 브랜치를 로컬 브랜치로 checkout 하면 자동으로 **트래킹 브랜치** 가 만들어집니다.
(트래킹 하는 대상 브랜치를 **Upstream 브랜치** 라고 부릅니다.)

트래킹 브랜치는 리모트 브랜치와 직접적인 연결고리가 있는 로컬 브랜치입니다. 트래킹 브랜치에서 git pull 명령을 내리면 리모트 저장소로부터 데이터를 내려받아 연결된 리모트 브랜치와 자동으로 Merge 합니다.

서버로부터 저장소를 clone 하면 Git은 자동으로 master 브랜치를 origin/master 브랜치의 트래킹 브랜치로 만듭니다. 트래킹 브랜치를 직접 만들 수 있는데 리모트를 origin 이 아닌 다른 리모트로 할 수도 있고, 브랜치도 master 가 아닌 다른 브랜치로 추적하게 할 수 있습니다.

git checkout -b <branch> <remote>/<branch> 명령으로 간단히 트래킹 브랜치를 만들 수 있고,
--track 옵션을 사용하여 로컬 브랜치 이름을 자동으로 생성할 수 있습니다.

```
$ git checkout -b sf origin/serverfix
Branch sf set up to track remote branch serverfix from origin.
Switched to a new branch 'sf'
```

이제 sf 브랜치에서 Push 나 Pull 하면 자동으로 origin/serverfix 로 데이터를 보내거나 가져옵니다.

이미 로컬에 존재하는 브랜치가 리모트의 특정 브랜치를 추적하게 하려면 git branch 명령에 -u 옵션을 붙여 설정해줍니다.

```
$ git branch -u origin/serverfix
Branch sf set up to track remote branch serverfix from origin.
```