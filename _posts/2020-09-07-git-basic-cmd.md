---
title: "Git 기본명령어"
categories: 
  - git
toc: true
---

## 1. **기존 Directory를 Git 저장소로 만들기**

```bash
$ git init
```

- 이 명령은 .git이라는 하위 디렉터리를 만든다.
- .git 디렉터리에는 저장소에 필요한 뼈대 파일(skeleton)이 들어있다.

```bash
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version' 
```

- Git이 파일을 관리하게 하려면 저장소에 파일을 추가하고 커밋해야한다.
- git add 명령으로 파일을 추가하고 git commit 명령으로 커밋한다.

## 2. 수정하고 (로컬) 저장소에 저장하기

![github-basic-cmd-img001]({{site.url}}/assets/images/github-basic-cmd-img001.png)

- working directory의 모든 파일은 크게 Tracked(관리대상임)와 Untracked(관리대상이 아님)로 나눈다.
  - Tracked(관리대상)
    - Unmodified(수정하지않음) - 마지막 커밋 이후 수정하지 않은상태
    - Modified(수정함) - 파일 수정 상태
    - Staged(커밋으로 저장소에 기록할 상태) - 실제 커밋하기 위해 staged 상태로 만든다.
  - Untracked(관리대상이 아님)
    - 나머지 모든 파일

### **파일의 상태 확인하기**

```bash
$ git status
On branch master
nothing to commit, working directory clean
```

- 위의 내용은 파일을 하나도 수정하지 않았다는 것을 말해준다.

```bash
$ echo 'My Project' > README
$ git status
On branch master
Untracked files:
	(use "git add <file>..." to include in what will be committed)

		README

nothing added to commit but untracked files present (use "git add" to track)
```

- README 파일을 새로 생성할 경우 Untracked 상태로 추가된다.

### **파일을 새로 추적하기**

```bash
$ git add README
$ git status
On branch master
Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)

		new file:   README
```

- git add 명령어를 통해 README 파일이 Staged(Tracked)상태로 변한것을 볼수 있다.
- 커밋을 하게 되면 git add를 실행한 시점의 파일이 커밋되어 저장소 히스토리에 남는다.

(아래부터 코드의 실행 결과를 간단히 표시하겠습니다.)

### **Modifed 상태의 파일을 Stage하기**

```bash
$ git status
Changes to be committed:
		new file:   README

Changes not staged for commit:
		modified:   MODIFIEDFILE.js

```

- MODIFIEDFILE.js 파일은 "Changes not staged for commit"에 있다.
- 이것은 수정한 파일이 Tracked 상태이지만 아직 Staged 상태는 아니라는 것이다.
- Staged 상태로 만들려면 git add 명령어를 사용해야한다.

```bash
$ git add MODIFIEDFILE.js
$ git status
Changes to be committed:
		new file:   README
		modified:   MODIFIEDFILE.js
```

- 두 파일 모두 Staged 상태이므로 다음 커밋에 포함된다.
- 만약 위 상태에서 MODIFIEDFILE.js를 다시 수정하면 어떤 상태가 될까??

```bash
$ vim MODIFIEDFILE.js
$ git status
On branch master
Changes to be committed:
		new file:   README
		modified:   MODIFIEDFILE.js

Changes not staged for commit:
		modified:   MODIFIEDFILE.js
```

- 위와 같이 MODIFIEDFILE,js가 Staged 상태이면서 동시에  Unstaged 상태로 나온다.
- git add 명령을 실행하면 Git은 파일을 바로 Staged 상태로 만든다.
- 지금 이 시점에서 커밋을 하면 마지막으로 수정한 버전으로 커밋되는 것이 아니라 마지막으로 git add 명령을 실행했을 때의 버전이 커밋된다.

### **파일 상태를 짤막하게 확인하기**

```bash
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplelegit.rb
?? LICENSE.txt
```

- -s 옵션을 사용하면 위와 같이 현재 변경한 상태를 짤막하게 보여준다.

  ?? : Untracked 상태

  A : Staged 상태로 추가한 파일 중 새로 생성한 파일

  _ M : README 파일을 변경했지만 아직 Staged 상태로 추가하지 않은 상태

  M_ : lib/simplegit.rb 파일을 변경하고 Staged 상태로 추가한 상태

  MM : Rakefile 파일을 변경하고 Staged 상태로 추가한 후 다시 내용을 변경해서 Staged이면서 Unstaged 상태인 파일이다.

### **파일 무시하기**

```bash
$ cat .gitignore
*.[oa]
*~
```

- 어떤 파일은 Git이 관리할 필요가 없다. (보통 로그파일이나. 로컬 설정파일들..) 그런 파일을 무시하려면                .gitignore 파일을 만들고 그 안에 무시할 파일 패턴을 적는다.
- [**.gitignore 예제 확인 사이트**](https://github.com/github/gitignore)

### **변경사항 커밋하기**

```bash
$ git commit
```

- 수정한 것을 커밋하기 위해 Staging Area에 파일을 정리했다. (git add)
- Git은 생성하거나 수정하고 나서 git add 명령으로 추가하지 않은 파일은 커밋하지 않는다.
- -m 옵션을 사용하면 커밋할 때 코멘트를 달 수 있다.

### **Staging Area 생략하기**

```bash
$ git commit -a -m "add and commit"
```

- -a 옵션을 사용하면 Git은 Tracked 상태의 파일을 자동으로 Staging Area에 넣는다.

### **파일 삭제하기**

```bash
$ rm grit.gemspec
$ git status
On brach master
Changes not staged for commit:
		deleted:   grit.gemspec
```

- Git에서 파일을 제거하려면 git rm 명령으로 Tracked 상태의 파일을 삭제한 후(정확하게는 Staging area에서 삭제하는 것) 커밋해야한다. 이 명령은 working directory에 있는 파일도 삭제하기 때문에 실제 파일도 지워진다.
- Git 명령을 사용하지 않고 단순히 working directory에서 파일을 삭제하고 git status 명령으로 상태를 확인하면 위와 같이 Unstaged 상태라고 표시한다.

```bash
$ git rm grit.gemspec
rm 'grit.gemspec'
$ git status
On branch master
Changes to be committed:
		deleted:   grit.gemspec
```

- git rm 명령을 실행하면 삭제한 파일은 Staged 상태가 된다.
- 커밋하면 파일은 삭제되고 Git은 더이상 추적하지않는다.
- -f 옵션을 사용하면 강제 삭제가 가능하다.(ex. 수정한 파일을 Index에 추가했을 경우)

```bash
$ git rm --cached README
```

- —cahced 옵션은 하드디스크에 있는 파일은 그대로 두고 Git만 추적하지 않게한다.
- 이것은 .gitignore 파일에 추가하는 것을 빼먹었거나 대용량 로그 파일 등을 실수로 추가했을 때 유용하다.
- 

### **파일 이름 변경하기**

```bash
$ git mv file_old file_new
```

## 3. 되돌리기

```bash
$ git commit --amend
```

- 완료한 커밋을 수정해야 할 때 사용한다. (너무 일찍 커밋했거나 어떤 파일을 빼먹었을때, 커밋 메시지를 잘못 적었을 때)
- 이 명령은 Staging Area를 사용하여 커밋한다. 커밋 후 바로 이 명령어를 실행하면 방금 전에 한 커밋과 모든것이 같고, 커밋 메시지만 수정한다.

```bash
$ git commit -m 'file commit'
$ git add forgotten_file
$ git commit --amend
```

- 커밋을 했는데 Stage하는 것을 깜빡하고 빠트린 파일이 있으면 위와 같이 고칠 수 있다.
- 여기서 실행한 명령어 3개는 모두 하나의 커밋으로 기록된다.
- 두 번째 커밋은 첫 번째 커밋을 덮어쓴다.

### **파일 상태를 Unstage로 변경하기**

```bash
$ git reset HEAD MODIFIEDFILE.js
```

- 실수로 git add * 명령어를 사용하여 따로 커밋하려는 파일까지 Staging Area로 넘어갔다면 위 명령어를 사용하면된다.
- git reset 명령을 —hard 옵션과 함께 사용하면 working directory 파일까지 수정되기에 조심해야한다.

### **Modified 파일 되돌리기**

```bash
$ git status
Changes not staged for commit:
		modified:   MODIFIEDFILE.js

$ git checkout -- MODIFIEDFILE.js
$ git status
Changes not staged for commit:
		
```

- MODIFIEDFILE.js 파일을 수정하고 나서 최근 커밋된 버전으로 되돌리는 방법이다.
- 위 명령어는 원래 파일로 덮어썼기 때문에 수정한 내용이 전부 사라진다. 때문에 꼭 필요할 때만 사용해야한다.

## 4. 리모트 저장소

- 리모트 저장소는 인터넷이나 네트워크 어딘가에 있는 저장소를 말한다.
- 간단히 말해서 리모트 저장소를 관리하면서 데이터를 거기에 Push하고 Pull 하는 것이다.

### **리모트 저장소 확인하기**

```bash
$ git clone https://github.com/schacon/ticgit
...
$ cd ticgit
$ git remote
origin
```

- git remote 명령으로 현재 프로젝트에 등록된 리모트 저장소를 확인할 수 있다.
- 저장소를 Clone 하면 origin이라는 리모트 저장소가 자동으로 등록되기 때문에 origin이라는 이름을 볼 수 있다.
- -v 옵션을 사용하면 단축이름과 URL을 함께 볼 수 있다.

### **리모트 저장소 추가하기**

```bash
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote
origin
pb
```

- 로컬 저장소에는 없지만 Paul의 저장소에 있는 것을 가져오려면 아래와 같이 실행한다.

```bash
$ git fetch [remote-name]
```

- 이 명령은 로컬에는 없지만, 리모트 저장소에는 있는 데이터를 모두 가져온다. 그러면 리모트 저장소의 모든 브랜치를 로컬에서 접근할 수 있어서 언제든지 Merge 하거나 내용을 살펴볼 수 있다.
- git fetch 명령은 리모트 저장소의 데이터를 모두 로컬로 가져오지만, 자동으로 Merge하지 않는다.
- 때문에 수동 Merge 필요

### **리모트 저장소에 Push하기**

```jsx
$ git push origin master
```

- 이 명령은 Clone한 리모트 저장소에 쓰기 권한이 있고, Clone하고 난 이후 아무도 Upstream 저장소에 Push하지 않았을 때만 사용할 수 있다. 다시 말해서 Clone한 사람이 여러명 있을 때, 다른 사람이 Push한 후에 Push하려고 하면 Push 할 수 없다.