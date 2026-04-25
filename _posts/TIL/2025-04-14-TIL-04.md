---
layout: post
title: Github 기본 명령어
subtitle: Github
categories: TIL
tags: [TIL , Git]
---

## Github와 친해지기

---

### 기본적인 문법

git branch  login : commit이 있어야 가능   

git branch -m main : 현재 브랜치의 이름을 main으로 변경하라   

git branch : 현재 .git 폴더 안의 branch 확인   

git switch login "브랜치 이름" = git checkout login "브랜치 이름" : 브랜치 위치 이동    

git switch -c "브랜치 이름" = git checkout -b "브랜치 이름" : 브랜치 한번에 생성 & 이동  

git merge "브랜치 이름" : "브랜치 이름"에서 작업한걸 main에 합친다   

git branch "브랜치 명" : 브랜치 만들기   

git branch -d "이름" : "이름" 브랜치 삭제   

---

### PullRequest 

git merge <- 잘 안씀

Pull Request 란 ? 
Pull : 당겨서 합치는거 ( merge )  , Request : 요청하다 

git remote origin "주소" : 깃 주소 연동

git remote -v : 연결된거 확인

git remote remove origin : 연동해둔 깃주소 삭제

git pull origin main : pull할 main을 가져옴

git reflog : 모든 브랜치의 reflog를 보기

git reset --hard { commit id } : commit id 까지 복구

git clone -b "branch 명" : " branch 명 " branch 다운

---

