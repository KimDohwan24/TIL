# DoHwan's Study Room

김도환의 개발 학습 기록 블로그입니다. Java·Spring, React·Next.js, 데이터베이스, 알고리즘과 프로젝트에서 얻은 인사이트를 정리합니다.

- 사이트: <https://kimdohwan24.github.io/TIL/>
- 배포: GitHub Pages (`main` 브랜치 push 시 GitHub Actions 실행)
- 기반 테마: [jekyll-theme-yat](https://github.com/jeffreytse/jekyll-theme-yat)

## 로컬 실행

Ruby와 Bundler가 설치된 환경에서 실행합니다.

```bash
bundle install
bundle exec jekyll serve --livereload
```

브라우저에서 <http://localhost:4000/TIL/>을 엽니다.

## 글 작성

`_posts/<category>/YYYY-MM-DD-title.md` 형식으로 파일을 만들고, 아래 정보를 작성합니다.

```md
---
layout: post
title: 글 제목
subtitle: 한 줄 요약
description: 검색·공유에 사용할 글 설명
categories: TIL
tags: [Spring, JPA]
---

## 문제 상황
```

- `description`은 첫 번째 제목 대신 검색 결과와 링크 미리보기에 표시됩니다.
- 날짜가 미래라면 현재 설정상(`future: true`) 빌드 결과에 포함됩니다. 예약 발행이 필요할 때는 배포 전 날짜를 반드시 확인합니다.
- 본문 이미지를 확대 표시하려면 front matter에 `photo_swipe: true`를 추가합니다.

## 품질 확인

배포 전 아래 명령으로 정적 사이트가 정상 생성되는지 확인합니다.

```bash
bundle exec jekyll build
```

의존성을 변경했다면 `Gemfile.lock`도 함께 갱신해 커밋합니다.

## 구조

```text
_posts/       블로그 글
_layouts/     페이지 레이아웃
_includes/    재사용 UI와 확장 기능
_data/        홈 화면 등 기본 데이터
assets/       스타일, 스크립트, 이미지
.github/      GitHub Actions 배포 설정
```

## License

이 저장소에는 [jekyll-theme-yat](https://github.com/jeffreytse/jekyll-theme-yat)의 MIT 라이선스 기반 코드가 포함되어 있습니다. 자세한 내용은 [LICENSE.txt](LICENSE.txt)를 참고하세요.
