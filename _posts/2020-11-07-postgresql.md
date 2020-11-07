---
layout: post
title:  "MacOS x Homebrew - PostgreSQL 설치 및 로컬DB 생성"
author: julius
categories: [ Jekyll, tutorial ]
tags: [red, yellow]
image: assets/images/11.jpg
description: "My review of Inception movie. Acting, plot and something else in this short description."
featured: true
hidden: true
rating: 4.5
beforetoc: "해당 문서는 PostgreSQL 13 버전을 기준으로 작성되었습니다. 따라서 다른 버전에서는 다른 커맨드가 필요하거나, 커맨드의 실행에 대한 결과값이 다를 수 있습니다."
---

`** 해당 포스팅은 제가 찾은 정보들을 바탕으로 작성된 것이며, 정보가 빈약하거나 오류가 있을 수 있습니다.
댓글로 지적 & 수정요청 해주시면 너무나 감사하겠습니다!`


## PostgreSQL 설치 및 구동
1\. PostgreSQL 설치
```
brew install postgres
```

2\. 설치 확인
```

postgres --version
결과 콘솔) postgres (PostgresSQL) [버전]
```
3\. MacOS의 LaunchAgents에 PostgreSQL 서비스 실행 설정 파일 등록
* LaunchAgents는 MacOS의 Daemon 입니다.
* Daemon이란, OS의 백그라운드에서 자동으로 구동되는 서비스 프로그램이며, 해당 프로그램은 OS 구동시에 등록된 다양한 설정 파일들을 읽어들여 실행, 또는 처리합니다.
```
ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
결과 콘솔) 없음
```

4\. 구동
```
brew services start postgresql
결과 콘솔) Successfully started `postgresql` (label: homebrew.mxcl.postgresql)
```
* "start" 대신 "stop" 키워드를 집어넣어 커맨드를 날림으로써 서비스를 종료시킬 수 있습니다.

---

## 로컬 DB 스키마 생성
1\. "postgres" 기본 사용자 Role 생성
* Homebrew로 PostgreSQL 설치 시, DB 최초 접속 시에 체크하게 될 "postgres" 사용자 Role이 생성되어있지 않아 접속이 되지 않습니다.
따라서 스크립트로 기본 사용자 Role을 생성해줘야만 접속이 가능합니다.
```
/usr/local/opt/postgres/bin/createuser -s postgres
결과 콘솔) 없음
```

2\. pgAdmin 4.app 실행
* /Library/PostgreSQL/[버] 경로 디렉터리에서 pgAdmin 4.app을 실행시킵니다.
* 최초 서버 접속 시에 현재 OS 사용자의 비밀번호를 입력합니다.
* 좌측 Servers 인스턴스를 열어, 하단의 PostgreSQL [버전] 인스턴스를 클릭하면, "postgres" 사용자에 대한 비밀번호를 묻는데, 빈칸으로 ok를 누르시면 접속이 됩니다.
* 사용자 Role과 동일한 이름으로 DB가 기본 생성되어 있습니다.
* 따라서 접속한 "postgres" DB 인스턴스 내부의 "Schemas" 인스턴스를 우측클릭하여 "create"를 선택 후, 스키마 명을 입력하여 생성합니다.
