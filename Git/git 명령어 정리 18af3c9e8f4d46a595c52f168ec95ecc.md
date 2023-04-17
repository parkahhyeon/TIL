# git 명령어 모음

## git init

> `$ git init`  #새로운 저장소 생성
> 
- 보통 프로젝트 처음 시작할 때 git 저장소를 만들기 위해 사용.
- ‘초기화’한다는 뜻이지만 내용이 존재하지 않아도 됨.

🌟 `$ git init` 을 하기 전에 **먼저 git 저장소를 만들 디렉터리로 이동**해야 함!! (없으면 생성)

- `$ git status` 로 현재 git 저장소가 없는 상태인지 확인 후, 그 상태에서 init 실행하기

```bash
$ cd <이동할_dir명> # git 저장소를 생성할 디렉터리로 이동 후
$ git status (fatal: not a git repository) # git저장소 유무 상태 확인
$ git init # git이라는 디렉터리를 빈 저장소로 만들어 준다.
```

## git config

> Git 설정 확인
> 

```bash
$ git config -l
$ git config --list
```

## git config —global user.name(또는 email) 설정
|

```bash
# 현재 user.name과 user.email 확인
$ git config --global user.name
$ git config --global user.email

#  --global를 사용해 전역으로 계정 설정.
$ git config --global user.name "USER_NAME"
$ git config --global user.email "USER_EMAIL"

# 특정 레파지토리에만 계정을 다르게 설정할 때, 
##방법1 ' --global' 플래그를 빼고 수행하면 됨.
$ git config user.name "example"
$ git config user.email example@gmail.com

##방법2. 레파지토리마다 다른 계정 정보 사용.
$ git config --local user.name "USER_NAME"
$ git config --local user.email "USER_EMAIL"

# 이미 설정된 user.name, user.email이 있을 때, 설정된 계정 삭제하기
## global
$ git config --unset --global user.name
$ git config --unset --global user.email

## local
$ git config --unset user.name
$ git config --unset user.email
```

## git clone

> `$ git clone`  #저장소 복제/다운로드
> 
- local에 github의 저장소를 복제해와 작업하는 것.
- 주로 오픈소스를 개발하거나 이미 진행중인 프로젝트에 참여할 때 사용.

```bash
$ git clone <https:... 복제해올_URL> # 기존의 소스코드 다운로드/복제

$ git clone /로컬/저장소/경로  # 로컬 저장소 복제

$ git clone 사용자명@호스트:/원격/저장소/경로  # 원격 서버 저장소 복제
```

- 저장소를 clone하면 origin remote에 가져온 url이 저장됨.
- 이후에는 주소 지정없이 origin 명령어로 저장소의 내용을 `fetch`  또는 `push` 가능.
(private 저장소는 접근권한이 없기 때문에 username과 password 필요)

- **shallow clone**
    
    clone시 너무 무거운 프로젝트의 저장소는 여러가지 편집과정이 다 섞여 있기 때문에, 최신 수정이 반영된 코드만 복제해 올 경우 `shallow clone` 을 사용함.
    
    ```bash
    $ git clone --depth=1 <복제해올_URL>
    ```
    
- **트러블 슈팅**
    
    clone에 실패하는 경우 이런 오류 메시지를 볼 수 있음.
    
    `fatal: destination path '저장소명' already exists and is not an empty directory.`
    
    - 이미 해당 저장소 안에 내용이 존재한다는 것!
    - git이 자동으로 repository 이름과 같은 이름에 복제를 시도하기 때문에 이미 clone 이력이 있는 경우 오류가 발생함.
    - 이를 해결하기 위해서는 아래와 같이 다른 디렉터리를 지정해 clone을 시도하자.
        
        ```bash
        $ git clone <복제할_url> <다른_dir명>
        ```
        

## git add

> `$ git add`  #추가
> 

```bash
# 커밋에 단일 파일의 변경사항을 포함
$ git add <파일명>  // 특정 파일 Index(Staging area)에 추가
$ git add .        // 현재 및 하위 디렉터리 모든 파일 index 추가

# 커밋에 파일의 변경사항을 한번에 모두 포함
$ git add -A

$ git commit
$ git status
```

## git commit -m “커밋 메시지”

> 임시 변경사항 확정
> 

```bash
# 커밋 생성 (실제로 변경된 사항 확정)
$ git commit -m "커밋 메시지 설명" // local repository에 추가
$ git status #파일 상태 확인       
```

## git log

> 커밋 히스토리 조회
> 

```bash
$ git log   // 저장소의 커밋 히스토리를 시간순으로 보여줌 (가장 최근의 커밋이 먼저 나옴)
						// 각 커미스이 SHA-1 체크섬, author, 저자 이메일, 커밋한 날짜, 커밋 메시지를 보여줌
$ git log --oneline   // 한 줄로 조회
$ git log --since=2.weeks  // 지난 2주 동안 만들어진 커밋들만 조회
$ git log --pretty=format:"%h %s" --graph  // 브랜치와 머지 히스토리를 보여주는 아스키 그래프를 출력함.
```

[option]

| -p | 각 커밋의 diff 결과를 보여줌. |
| --- | --- |
| -2 | 최근 두 개의 결과만 보여줌. |
| -stat | 각 커밋의 히스토리 통계 정보를 조회. |
| --pretty=oneline | 각 커밋을 한 라인으로 보여줌. 많은 커밋을 한 번에 조회할 때 유용. |
| --pretty=short
--pretty=full
--pretty=fuller | 히스토리를 가감해서 보여줌. |
| -pretty=format “  “ | 나만의 형식으로 결과를 출력하고 싶을 때 사용. 특히 결과를 다른 프로그램으로 파싱하고자 할 때 유용. |

<git log --pretty=format 에 쓸 몇가지 유용한 옵션>

| 옵션 | 설명 |
| --- | --- |
| %H | 커밋 해시 |
| %h | 짧은 길이 커밋 해시 |
| %T | 트리 해시 |
| %t | 짧은 길이 트리 해시 |
| %P 부모 해시 | %P 부모 해시 |
| %p  | 짧은 길이 부모 해시 |
| %an  | 저자 이름 |
| %ae  | 저자 메일 |
| %ad  | 저자 시각 (형식은 –-date=옵션 참고) |
| %ar  | 저자 상대적 시각 |
| %cn  | 커미터 이름 |
| %ce  | 커미터 메일 |
| %cd  | 커미터 시각 |
| %cr        | 커미터 상대적 시각 |
| %s  | 요약 |
- 저자(Author) 와 커미터(Committer) 를 구분하는 것이 조금 이상해 보일 수 있다.
- 저자는 원래 작업을 수행한 원작자이고 커밋터는 마지막으로 이 작업을 적용한(저장소에 포함시킨) 사람이다.
- 만약 당신이 어떤 프로젝트에 패치를 보냈고 그 프로젝트의 담당자가 패치를 적용했다면 두 명의 정보를 모두 알 필요가 있다. 그래서 이 경우 당신이 저자고 그 담당자가 커미터다.

## git show

> 커밋 정보 확인
> 

```bash
$ git show  // 최신 커밋 정보를 출력. HEAD는 가장 최신 커밋을 가리킴.

// #따라서 다음과 같이 사용 가능 :
$ git show HEAD
$ git show [commit_ID] // 커밋 아이디 정보 및 수정 내용 보여줌. 커밋 아이디는 7자리까지만 입력해도 됨.
$ git show HEAD^  // HEAD^(HEAD~1)는 가장 최신 커밋의 이전 커밋을 가리킴. 
                  // HEAD^^^(HEAD~3)은 최신 커밋의 3개 전의 커밋을 의미.
```

## git branch

## git checkout

> 브랜치 목록 조회, 생성, 이동, 삭제 .etc
> 

> 내가 사용할 브랜치 지정
> 

```bash
#브랜치 목록 조회. 브랜치면 앞에 *(asterisk) 붙어 있는 곳이 현재 위치를 의미함
$ git branch

#새 브랜치 생성 (local로 생성)
$ git branch <브랜치 이름>

#특정 브랜치 워킹 디렉터리 변경. 브랜치 이동하기
$ git checkout [브랜치 이름]

#브랜치 생성 & 이동
$ git checkout -b <브랜치 이름>

# 다른 브랜치에서 현재의 브랜치로 파일/폴더 가져오기
$ git checkout <가져올 브랜치> --<파일경로>

# master branch로 되돌아가기
$ git checkout master

# 브랜치 삭제
$ git branch -d <브랜치 이름>

# 만든 브랜치를 원격 서버에 전송하기
$ git push origin <브랜치 이름>

#새 브랜치를 원격 저장소로 push
$ git push -u < remote > <브랜치이름>

# 원격에 저장된 git 프로젝트의 현재 상태를 다운받고 현재 위치한 브랜치로 병합하기
$ git pull < remote > <브랜치 이름>
```

## git push

> 원격서버에 업로드
> 

```bash
# 변경사항 원격 서버에 업로드
$ git push origin master

# 커밋을 원격 서버에 업로드
$ git push < remote > <브랜치이름>

# 커밋을 원격 서버에 업로드
$ git push -u < remote > <브랜치이름>

# 클라우드 주소 등록 및 발행
$ git remote add origin <등록된 원격 서버 주소>

# 클라우드 주소 삭제
$ git remote remove <등록된 클라우드 주소>
```

## git merge

> 변경사항 병합
> 

```bash
# 원격 저장소의 변경 내용이 현재 디렉토리에 가져와져서 병합됨.
$ git pull

# 현재 브랜치에 다른 브랜치의 수정사항 병합
$ git merge <다른 브랜치이름>

# 각 파일을 병합
$ git add <파일명>

# 변경 내용 merge 전에 바뀐 내용을 비교
$ git diff <브랜치이름><다른 브랜치이름>
```

## 로컬 변경사항 return

```bash
# 로컬의 변경사항을 변경 전으로 되돌림
$ git checkout --<파일명>

# 원격에 저장된 git 프로젝트의 현 상태를 다운로드
$ git fetch origin
```

## 터미널 화면 정리 clear

```bash
# 방법 1
[Ctrl] + L

# 방법 2
$ clear   // 터미널 화면 클리어
```

---

# 상황별 대처법

## github에 잘못 올린 파일 지우기

- repository 및 로컬저장소 파일 삭제
    
    ```bash
    $ git rm -r <file/dir명>
    ```
    
- repository 파일만 삭제
    
    ```bash
    $ git rm --cached -r <file/dir명>  # cached는 원격저장소의 폴더/파일을 삭제하는 명령어
    ```
    

위의 옵션 중 하나를 선택해 작성 후 다음 명령어를 추가로 입력해주면 삭제 완료.

```bash
$ git commit -m "commit_message"
$ git push origin master
```

## Repository 초기화⭐

- 실제로 git이 엉망이 되어 repository를 초기화하고 현재 폴더를 재정비한 후 다시 동기화 시킬 때 유용하게 사용 가능.
1. `.git`  디렉터리 삭제 / 상태 확인
    
    ```bash
    $ rm -rf .git
    $ git status (fatal: not a git repository)
    ```
    
2. git 초기화 후 새로운 git 설정
    
    ```bash
    $ cd <생성할 디렉터리>
    $ git init
    $ git add .
    $ git commit -m "commit message"
    ```
    
3. github 저장소 연결 후 강제 push
    
    ```bash
    $ git remote add origin <연결할 url>
    $ git push --force --set-upstream origin master
    ```
    

## 작업 도중 github repo 이름 변경 시 url(경로) 변경해주기

1. 현재 repo 확인
    
    ```bash
    $ git remote -v
    ```
    
2. push, fetch url 변경
    
    ```bash
    $ git remote set-url --push origin <변경할_저장소_주소>
    $ git remote set-url origin <변경할_저장소_주소>
    ```
    
3. 결과 확인
    
    ```bash
    $ git remote -v
    ```
    

## 이미 push한 마지막 커밋 메시지 수정하기 (amend)

amend; 수정하다

1. 원하는 변경 내용을 입력
    
    ```bash
    $ git commit --amend -m "변경_내용_입력" 
    $ git commit --amend --no-edit  // --no-edit 옵션은 수정하지 않을 때
    ```
    
2. 강제 push 하기
    
    ```bash
    $ git push origin main -f
    ```
    

## Git 버전 확인

```bash
$ git --version
```

## commit한 author 변경하기

```bash
# 커밋만 했을 경우
$ git commit --amend --author="example <example@gmail.com>"
$ git rebase --continue 
$ git push -f origin master

# push까지 했을 경우
#로컬과 원격지에 이미 반영되어 있고 잘못 커밋한 내역도 없애고 싶은 경우에는 rebase를 사용.
$ git log --oneline   //(1) 변경할 커밋 바로 이전의 커밋 해시값 찾기
....
```

## git에서의 대소문자 에러

- git은 기본적으로 파일명 또는 폴더명의 대소문자 구분 못함.
- 그런데, 만약 프로젝트의 모든 첫글자가 대문자인 상황에 한 개의 파일을 소문자로 설정한 채 `commit push` 를 한다면 ERROR 발생!!
    
    **대처 방법**
    
    1. `$ git config --global core.ignorecase`   → 대소문자 무시하지 마라.
    2. `$ git rm -r --cache .`   → 저장된 캐시 모두 삭제 (사용 시 주의)
    *로컬에는 파일을 남기지만 원격 저장소에 있는 파일은 모두 삭제시키는 명령어. 
    3. `$ git add .`   → add하고 (혹시 모르니 소문자를 대문자로 바꿔서 add함.)
    4. `$ git commit -m "remove all cache"`  → 원격저장소에 다 삭제.
    5. 빨리 커밋하기!
