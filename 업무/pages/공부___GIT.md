- 이전 자료
  collapsed:: true
	- 설정 방법 인터넷 링크
	  collapsed:: true
		- [Git 명령어 총정리집 (by 코딩알려주는 누나❤) - HackMD](https://hackmd.io/@oW_dDxdsRoSpl0M64Tfg2g/ByfwpNJ-K)
		- [Git을 이용한 버전 관리 【Git의 기본】 | 누구나 쉽게 이해할 수 있는 Git 입문~버전 관리를 완벽하게 이용해보자~ | Backlog](https://backlog.com/git-tutorial/kr/intro/intro1_1.html)
		- ![image.png](../assets/image_1689061695418_0.png)
	- 주요 링크
		- [git reset, revert로 이전 커밋으로 돌리기 | 기억보다 기록을 (kyounghwan01.github.io)](https://kyounghwan01.github.io/blog/etc/git/git-reset-revert/#reset)
	- 주요 용어
		- remote : 원격저장소 연결
		- add : 인데스에 등록
		- commit : 인덱스 상태기록 , 로컬 저장소에 히스토리 추가
		- push : 원격으로 쓰기
		- pull : 원격에서 가져오기
		- fetch : 리모트 서버에 업데이트된 내역을 받아온다
		- pull vs fetch
			- pull : 무조건 최신 소스로
			- fetch : 변경사항이 있는지 확인만 하고 변경된 데이터를 local로 가져오지 않습니다.
		- remote : 서버
		- origin : 리모트 서버 이름
	- git 설정
		- 유저이름 설정
			- git config --global user.name "your_name"
		- 유저 이메일 설정
			- git config --global user.email "your_email"
		- 정보확인하기
			- git config --list
		- 자동 로그인
			- git config --global credential.helper store
	- git 초기화 및 연결
		- 초기화
			- git init
		- 추가할 파일 더하기
			- git add .
		- 상태확인
			- git status
		- 히스토리 만들기 (commint)
			- git commit -m "히스토리 이름"
		- github repository와  로컬 연결
			- github 웹에서 repository를 미리 생성해야 연결 가능
			- git remote add origin https://github.com/bitnaGithub/firstproject.git
			- git remote -v  : 확인
		- github로 올리기 (push)
			- git push origin master (master : branch name)
	- github 업데이트
	  collapsed:: true
		- git add .
		- git commit -m "히스토리명"
		- git push origin master
	- git 협업
	  collapsed:: true
		- 다운로드
			- git clone 주소
		- github 내의  branch 만들기
			- git checkout -b 브렌치이름  (b:브랜치를 만들고 + 체크아웃)
	- branch
	  collapsed:: true
		- 종류 확인
			- git branch
		- 생성
			- git branch 이름
		- 삭제
			- git branch -d 이름
		- 변경 (체크아웃)
			- git checkout 이름
			- git branch -b 이름 (생성 & 체크아웃)
	- 로그 보기
	  collapsed:: true
		- checkout 된 branch가 빨간색이다
		- branch를 생성하면 녹색으로 이름이 추가된다
	- 내 branch에 소스 업데이트
		- git add .
		- git commit -m "history"
		- git push origin 브렌치 이름
	- 머지
		- git merge 이름 (업데이트 당하는 branch로 체크아웃후 업데이트 할 branch 이름을 적어준다
		- main branch의 내용이 변경아 안된상태에서는 merge에 문제가 없지만, main이 변경되었다면 머지에러 발생한다
	- 커밋
		- 메시지 수정
			- git commit --amend -m "메시지"
		- 커밋 아이디 확인
			- git log --online -n 4
	- 되돌리기
		- 리버트
			- git revert 커밋아이디
			- 직전 것이 아니면 머지 에러가 나는데... 왜 그럴까?
			- revert 명령어는 `reset --soft`, `mixed`와 동일한 결과를 가져오지만 이력은 `Revert "..."`라는 메세지가 추가됩니다.
			- 실행
				- git revert HEAD (최신 커밋 아이디): 이전 commit 으로 돌아간다
				- git revert 커밋아이디
					- 충돌에러가 발생하네
					  1)소스 수정하고
					  2)git add . 하고
					  3)git revert --continue 로 하면 마무리된다.
					- 해당 커밋아이디가 아닌 해당 커밋아이디의 직전으로 돌아가려고 하는 듯
					  (해당 커밋아이드를 날리고 이전 내용을 마지막으로 삭제된 위치에 신규 commit 이 생성된다)
		- 리셋
			- git reset 옵션의 차이는 명확히 인식했다
			- git reset --hard 아이디
				- 해당 아이디가 마지막 commit 이 된다
				- 지워진 아이디를 입력하면 복구된다
			- soft
				- HEAD 위치
			- mixed
				- HEAD 위치 , staging area
			- hard
				- HEAD 위치 , staging area, working directory
			- 문법
				- git reset HEAD^ : 가장 최근의 commit 취소 (기본옵션 :mixed)
				- git reset HEAD~10 : 현재로부터 10개 commit 취소
			- 실습
				- git reset --hard 커밋아이디
					- 해당커밋아이디가 마지막이 되고 위에는 모두 사라졌다
				- git reset --soft 커밋아이디
					- 해당커밋아이디가 마지막이 된다
					- 소스코드는 유지 / commited 상태
					- add 없이 commit 하니 돌아왔던 commit id 상태에서 소스는 변함이 없었다
					- --soft = --mixed + git add
				- git reset --mixed 커밋아이디
					- 해당커밋아이디가 마지막이 된다
					- 소스코드는 유지 / not staged 상태
					- add 없이 commit 하니 돌아왔던 commit id 상태에서 소스는 변함이 없었다
		- 리스토어
		  collapsed:: true
			- git restore --source 커밋아이디 파일이름
				- 특정시점 특정파일로 되돌리기
		- rebase
	- status
		- 파일 변경 후 git status : not staged , 빨간색으로 modified
		- git add 후 git status : commited , 초록색으로 modifed
	- 코딩알려주는 누나 유튜브
		- [(53) 회사에서 개발자들은 어떻게 일할까? 회사에서 쓰는 실전 깃 깃허브 한방에 끝내기! 15분만 투자해라 님들의 회사생활이 편해짐 - YouTube](https://www.youtube.com/watch?v=cwC8t9dno2s&t=568s)]
		- github에서 company_project repository 생성
		  logseq.order-list-type:: number
		- [leader] 파일 생성
		  logseq.order-list-type:: number
			- git init
			  logseq.order-list-type:: number
			- git add .
			  logseq.order-list-type:: number
			- git commit -m "first commit"
			  logseq.order-list-type:: number
			- git push origin main
			  logseq.order-list-type:: number
		- [fresh man] clone
		  logseq.order-list-type:: number
			- git clone https://github.com/geungge/company_project.git freshman
			  logseq.order-list-type:: number
			- git add .
			  logseq.order-list-type:: number
			- git commit -m "freshman first commit"
			  logseq.order-list-type:: number
			- git checkout -b freshman
			  logseq.order-list-type:: number
			- git push origin freshman
			  logseq.order-list-type:: number
		- github에 "freshman had recent pushes less than a minute ago"
		  logseq.order-list-type:: number
		  "Compare & pull request" 버튼 클릭
		  수정내용을 작성하고 "Create pull request" 클릭
		- 상단에 Pull request 탭에 알람이 생긴다
		  logseq.order-list-type:: number
		- 리더는 "Merge pull request"를 눌러서 머지한다 (freshman -> main)
		  logseq.order-list-type:: number
			- confirm merge를 누르면 보라색으로 바뀌고 main으로 머지되는 것 같다
			  logseq.order-list-type:: number
			- 근데 local main에서 git pull해도 뭐라고 메시지는 나오는데.. 파일은 업데이트가 안되네
			  logseq.order-list-type:: number
			- git pull origin main 을 하니까 파일까지 업데이트 되더라
			  logseq.order-list-type:: number
			- 여전히 leader의 local에서는 freshman의 branch가 보이지 않는다
			  logseq.order-list-type:: number
			  같은 git folder에서 생성한 것이 아니라서 그런거 아닐까 싶다
- VS CODE 연결
  collapsed:: true
	- 간단 순서
		- SOURCE CONTROL
		- Initialize Repository : 저장소 생성 (.git)
		- File Exploror  (설정/exclude/.git 삭제 -> .git 보여진다)
		- 신규파일 생성 하면 Changes  에 추가 됨
		- +를 누르면 Staged Changes 로 이동 된다 (comit 대기 상태)
		- comit 을 눌러서 버전 만들기를 완료한다
	- 버전이동
		- git checkout ID
		- git log
		- git log --all
		- git log --all --oneline
		- 현재 버전으로 이동
		  collapsed:: true
			- git checkout 현재(마지막)ID
			- git checkout master
		- 정리
			- 과거버전 이동 : git checkout ID  (메뉴/checkout)
			- 현재버전 복귀 : git checkout master (메뉴/checkout branch)
	- 기타
		- working directory - stage area -> repository
		- user name / user emai
		  collapsed:: true
			- git config --global user.email "geungge@gmail.com
			- git config --global user.name "geungge"
		- git grpah extention
			- 동그라미 = HEAD
- VS CODE에서 GIt 협업
  collapsed:: true
	- 저장소
		- Remote Repository
		- Local Repository
	- push / pull
		- push : local -> remote
		- pull : local <- remote  = fetch + merge
- 링크
  collapsed:: true
	- [Simple Git tutorial for beginners | Nulab](https://nulab.com/ko/learn/software-development/git-tutorial/)
- 로컬 무시하고 최신 업데이트
  collapsed:: true
	- git fetch --all
	  git reset --hard origin/master
	  git pull origin master
- ### merge
  collapsed:: true
	- [Git - 브랜치와 Merge 의 기초 (git-scm.com)](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge-%EC%9D%98-%EA%B8%B0%EC%B4%88)
	- [[GIT] 병합(merge) 종류 별 완벽 설명 (tistory.com)](https://minemanemo.tistory.com/46)
	- main 과 syc 병합
		- git merge --no-ff syc   (syc->main)
		- git merge --squash syc
			- branch는 그대로 남아있고 branch를 하나의 id로 커밋
		- git merge --ff syc
		- git merge --ff-only syc
	- main 을 remote 상태로 업데이트
		- git fetch origin
		  git merge origin/main
- ### 마지막 리비전
  collapsed:: true
	- git reset
	- git checkout .
- ### 롤백
	- revert
	- rebase
	- reset
		- 돌아갈 ID에 놓고 reset 수행
		- hard : 이전 완전 삭제
		- mix : 로컬에 코드 uncommitted 상태로 남아있다
- ### 브랜치
	- #### 삭제
		- 로컬 : git branch -D 브랜치명
		- 원격 : git branch origin :브랜치명
	- #### 생성
		- git branch 브랜치명
	- #### 이동
		- git switch 브랜치명
- ### 로그 수정
	- 최신 커밋 로그 변경 : git commit --amend
-