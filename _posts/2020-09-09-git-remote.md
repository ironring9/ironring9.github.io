### 3. 브랜치 관리

- 브랜치를 관리하는 데 필요한 다른 명령도 살펴보자.

```jsx
$ git brach
  iss53
* master
  testing
```

- * 기호가 붙어 있는 master 브랜치는 현재 Checkout해서 작업하는 브랜치를 나타낸다.

```jsx
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```

- -v 옵션은 마지막 커밋의 메시지도 함께 보여준다.

```jsx
$ git branch --merged
  iss53
* master
```

- —merged 옵션은 현재 브랜치 기준으로 merge 된 브랜치들을 보여준다.
- * 기호가 붙어있지 않은 브랜치는 삭제해도 되는 브랜치이다.

```jsx
$ git branch --no-merged
  testing
```

- 반대로 현재 checkout한 브랜치에 merge하지 않은 브랜치를 살펴보려면 위 옵션을 사용하면 된다.

### 4. 리모트 브랜치

- 리모트 Refs는 리모트 저장소에 있는 포인터인 레퍼런스다. 리모트 저장소에 있는 브랜치, 태그 등등을 의미한다.
- 리모트 Refs가 있지만 보통은 리모트 트래킹 브랜치를 사용한다.

**리모트 트래킹 브랜치**

- 리모트 트래킹 브랜치는 리모트 브랜치를 추적하는 브랜치다.
- 리모트 서버에 연결할 대마다 리모트 브랜치에 따라서 자동으로 움직일 뿐이다.
- 리모트 저장소에 마지막으로 연결했던 순간에 브랜치가 무슨 커밋을 가리키고 있었는지를 나타낸다.
- 리모트 브랜치의 이름은 (remote)/(branch) 형식으로 되어 있다.
- 예를 들어, 리모트 저장소 origin의 master 브랜치를 보고 싶다면 origin/master라는 이름으로 브랜치를 확인하면 된다.

- 예를 들어 git.ourcompany.com이라는 Git서버가 있고 이 서버의 저장소를 하나 Clone하면 Git은 자동으로 origin이라는 이름을 붙인다.
- origin으로부터 저장소 데이터를 모두 내려받고 master 브랜치를 가리키는 포인터를 만든다. 이 포인터는 origin/master라고 부르고 멋대로 조종할 수 없다.
- 그리고 Git은 로컬의 master 브랜치가 origin/master를 가리키게 한다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2010.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2010.png)

- 로컬 저장소에서 어떤 작업을 하고 있는데 동시에 다른 팀원이 [git.ourcompany.com](http://git.ourcompany.com) 서버에 Push하고 master 브랜치를 업데이트한다. 그러면 이제 팀원간의 히스토리는 서로 달라진다.
- 서버 저장소로부터 어떤 데이터도 주고받지 않아서 origin/master 포인터는 그대로다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2011.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2011.png)

- 리모트 서버로부터 저장소 정보를 동기화하려면 git fetch origin 명령을 사용한다.
- 명령을 실행하면 우선 'origin' 서버의 주소 정보(이 예에서는 git.ourcompany.com)를 찾아서, 현재 로컬의 저장소가 갖고있지 않은 새로운 정보가 있으면 모두 내려받고, 받은 데이터를 로컬 저장소에 업데이트하고 나서 origin/master 포인터의 최신 커밋으로 이동시킨다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2012.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2012.png)

**Push하기**

- 로컬의 브랜치를 서버로 전송하기 위해 push 명령어를 사용한다.

```jsx
$ git push origin serverfix
```

- serverfix 브랜치를 origin 리모트에 push한다.

Pull하기

- pull 명령은 사실상 fetch한 후에 merge를 자동으로 해준것이라 할수 있다.

```jsx
$ git pull 
```

