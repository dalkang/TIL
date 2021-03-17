# Github TIL

## 1. Git?

* #### 버전으로 코드를 관리하는 도구

  * ##### `버전` 으로 관리하는 장점?

    * 언제든지 과거의 코드로 돌아갈 수 있음.
    * 작업 이력을 확인할 수 있음.



## 2. 저장소 설정

> 유의사항
>
> 1. `git`은 폴더 단위로 관리
>
> 2. `git` 저장소 상위 폴더나 하위 폴더에는 새로운 `git` 저장소를 또 생성하면 안 됨

### (1) Git으로 저장소 관리 시작 : `git init`

* 앞으로 학습할 내용을 기록할 폴더를 하나 생성한다. 이때 해당 폴더는 최상단에 생성한다.

* `git bash` 에서 폴더로 이동한 후에 아래의 명령어로 `git` 관리를 시작한다.

  ```bash
  $ git init
  ```



### (2) Commit을 위한 Staging : `git add`

* 현재 코드 상태의 스냅샷을 찍기 위한 파일 선택 (== Staging Area에 파일 추가)

  ```bash
  $ git add [파일 이름]
  # .은 모든 변경 사항을 staging area로 올림
  ```



### (3) 버전 관리를 위해 저장 : `git commit`

* 현재 상태를 `commit` 하여, 버전 관리를 진행한다.

* 커밋할 파일을 선택해서 단계별로 staging area에 추가하는 것

  ```bash
  $ git commit -m "커밋 메세지"
  ```



### (4) 원격 저장소 정보 추가 : `git remote`

* Github 원격(remote) 저장소(repository)를 생성하고 폴더와 연결한다.

* **새로운 원격 저장소가 추가될 때만** 입력한다.

  ```bash
  $ git remote origin [저장소 이름] [저장소 주소]
  ```

* 현재 저장된 원격 저장소 정보 출력

  ```bash
  $ git remote
  # origin
  ```



### (5) 원격 저장소로 코드 `git push`

* 최종적으로 Github 원격 저장소에 push한다.

  ```bash
  $ git push origin master
  ```



### (6) 그 외 명령어

* 현재 `git` 의 상태를 조회 : `git status`

  ```bash
  $ git status
  ```

  * **항상 최상단 폴더에서 `git status` 로 확인하는 습관 들이기**

* 버전 관리 이력을 조회

  ```bash
  $ git log
  ```

* `git` 설정(user.name, user.email) : **최초 1회 설정**

  ```bash
  $ git config --global user.name "github ID"
  $ git config --global user.email "github e-mail"
  ```
