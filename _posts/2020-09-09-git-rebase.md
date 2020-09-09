### 5. Reabase하기

- Git에서 브랜치에서 다른 브랜치로 합치는 방법은 두 가지다.
- 하나는 Merge이고 다른 하나는 Rebase이다.

**Rebase의 기초**

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2013.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2013.png)

- 위 브랜치를 합치는 방법은 merge 명령을 사용하는 것이다. merge 명령을 사용하면 3-way Merge로 새로운 커밋을 만들어낸다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2014.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2014.png)

- rebase 명령은 일단 두 브랜치가 나뉘기 전인 공통 커밋으로 이동하고 나서 그 커밋부터 지금 Checkout한 브랜치가 가리키는 커밋까지 diff를 차례로 만들어 어딘가에 임시로 저장해 놓는다. Rebase할 브랜치가 합칠 브랜치가 가리키는 커밋을 가리키게 하고 아가 저장해 놓았던 변경사항을 차례대로 적용한다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2015.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2015.png)

- 그러고 나서 master 브랜치를 Fast-forward한다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2016.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2016.png)

- C4' 기준으로 보면 사실상 merge 명령과 결과적으로 다른게 없다. 하지만 rebase가 좀 더 깨끗한 히스토리를 만든다.
- Rebase한 브랜치의 Log를 살펴보면 히스토리가 선형이다.

**Rebase 활용**

- Rebase는 단순히 브랜치를 합치는 것만 아니라 다른 용도로도 사용할 수 있다. server 브랜치를 만들어서 서버 기능을 추가하고 그 브랜치에서 다시 client 브랜치를 만들어 클라이언트 기능을 추가한다. 마지막으로 server 브랜치로 돌아가서 몇 가지 기능을 더 추가한다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2017.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2017.png)

- 이 때 테스트가 덜 된 server 브랜치는 그대로 두고 client 브랜치만 master로 합치려는 상황을 생각해보자. server와는 아무 관련이 없는 client 커밋은 C8, C9 이다. 이 두 커밋을 master 브랜치에 적용하기 위해서는 —onto 옵션을 사용하여 아래와 같은 명령을 실행한다.

```jsx
$ git rebase --onto master server client
```

- 이 명령은 master 브랜치로부터 server 브랜치와 client 브랜치의 공통 조상까지의 커밋을 client 브랜치에서 없애고 싶을 때 사용한다. client 브랜치에서만 변경된 패치를 만들어 master 브랜치에서 client 브랜치를 기반으로 새로 만들어 적용한다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2018.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2018.png)

- 이제 master 브랜치로 돌아가서 Fast-forward 시킬 수 있다.

```jsx
$ git checkout master
$ git merge client
```

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2019.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2019.png)

- server 브랜치의 일이 다 끝나면 git rebase <basebranch> <topicbranh> 라는 명령으로 Checkout 하지 않고 바로 server 브랜치를 master 브랜치로 Rebase 할 수 있다.

```jsx
$ git rebase master server
```

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2020.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2020.png)

- 그리고 나서 master 브랜치를 fast-forward 시킨다.

```jsx
$ git checkout master
$ git merge server
$ git branch -d client
$ git branch -d server
```

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2021.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2021.png)

**Rebase의 위험성**

- Rebase가 장점이 많은 기능이지만 단점이 없는 것은 아니니 조심해야 한다. 그 주의사항은 아래 한 문장으로 표현할 수 있다.
- **이미 공개 저장소에 Push 한 커밋을 Rebase 하지 마라**
- Rebase는 기존 커밋을 그대로 사용하는 것이 아니라 내용은 같지만 다른 커밋을 새로 만든다.
- 새 커밋을 서버에 Push하고 동료 중 누군가가 그 커밋을 Pull 해서 작업을 한다고 하자.
- 그런데 그 커밋을 git rebase 로 바꿔서 Push 해버리면 동료가 다시 Push 했을 때 동료는 다시 Merge 해야한다. 그리고 동료가 다시 Merge 한 내용을 Pull 하면 내 코드는 정말 엉망이 된다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2022.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2022.png)

- 이제 팀원 중 누군가 커밋, Merge 하고 나서 서버에 Push 한다. 이 리모트 브랜치를 Fetch, Merge 하면 히스토리는 아래와 같이 된다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2023.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2023.png)

- 그런데 Push 했던 팀원은 Merge 한 일을 되돌리고 다시 Rebase 한다. 서버의 히스토리를 새로 덮어씌우려면 git push —force 명령을 사용해야한다. 이후에 저장소에서 Fetch 하고 나면 아래 그림과 같은 상태가 된다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2024.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2024.png)

- 한 팀원이 다른 팀원이 의존하는 커밋을 없애고 Rebase 한 커밋을 다시 Push함
- git pull 로 서버의 내용을 가져와서 Merge 하면 같은 내용의 수정사항을 포함한 Merge 커밋이 아래와 같이 만들어진다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2025.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2025.png)

- git log 로 히스토리를 확인해보면 저자, 날짜 메시지가 같은 커밋이 두 개 있다. (C4,C4') 이렇게 되면 혼란스럽다. 게다가 이 히스토리를 서버에 Push 하면 같은 커밋이 두 개 있기 때문에 다른 사람들도 혼란스러워 한다. 'C4', 'C6' 는 포함되지 말았어야 할 커밋이다. 애초에 서버로 데이터를 보내기 전에 Rebase로 커밋을 정리했어야 했다.

**Rebase 한 것을 다시 Rebase 하기**

- 만약 이런 상황에 빠질 때 유용한 Git 기능이 하나 있다. 어떤 팀원이 강제로 내가 한일을 덮어썼다고 하자. 그러면 내가 했던 일이 무엇이고 덮어쓴 내용이 무엇인지 알아내야한다.
- 커밋 SHA 체크섬 외에도 Git은 커밋에 Patch 할 내용으로 SHA-1 체크섬을 한번 더 구한다. 이 값은 patch-id 라고 한다.
- 예를 들어 앞서 살펴본 예제를 보면 **한 팀원이 다른 팀원이 의존하는 커밋을 없애고 Rebase 한 커밋을 다시 Push 함** 상황에서 Merge 하는 대신 git rebase teamone/master 명령을 실행하면 Git은 아래와 같은 작업을 한다.
  1. 현재 브랜치에만 포함된 커밋을 찾는다. (C2, C3, C4, C6, C7)
  2. Merge 커밋을 가려낸다. (C2, C3, C4)
  3. 이 중 덮어쓰지 않은 커밋들만 골라낸다. (C2, C3, C4는 C4'와 동일한 Patch다.)
  4. 남은 커밋들만 다시 teamone/master 바탕으로 커밋을 쌓는다.
- 결과를 확인해 보면 같은 Merge를 다시 한다 같은 결과 대신 제대로 정리된 강제로 덮어쓴 브랜치에 Rebase하기 같은 결과를 얻을 수 있다.

![Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2026.png](Git%20%E1%84%87%E1%85%B3%E1%84%85%E1%85%A2%E1%86%AB%E1%84%8E%E1%85%B5%201f85e910c6154b5689be747a4c50d7ea/Untitled%2026.png)