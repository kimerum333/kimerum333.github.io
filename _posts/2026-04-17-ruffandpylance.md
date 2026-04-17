---
title: "Python 개발환경 처음부터 세팅하기"
date: 2026-04-16 12:00:00 +0900
categories: [Python, Environment]
tags: [Pylance, Ruff]
---

인간은 숨쉬는 법은 너무 당연해서 굳이 기억하지 않는다.<br>
개발 초기 환경세팅법도 그렇다.<br>
일단 해두면 너무 자연스레 숨쉬듯 사용하다보니 <br>
신나게 새 컴퓨터로 바꾼다던가 하면 전부 잊어서 다시 세팅하다 멘탈이 부숴진다.<br>
하여, 다시는 잊어버리지 않기 위해 포스팅을 작성한다.

# Pylance
## 인텔리센스란 무엇인가?
MS 계열 IDE에 내장된 자동완성 기능의 이름이다.<br>
객체 뒤에 . 을 찍으면 맴버 목록을 표시해주거나,<br>
함수 호출시 필요한 파라미터의 정보를 은근슬쩍 보여주거나 한다.<br>

그럼, 인텔리센스는 어떻게 작동하는가.<br>
세상에 존재하는 모든 라이브러리의 함수와 객체 정보를 전부 학습한 녀석인걸까?<br>
당연히 아니다. 라이브러리의 소스파일 읽고 거기서 정보 가져오는 거다.<br>
소스파일을 읽으려면, 파이썬의 경우 인터프리터가 필요하다.<br>

## 그래서 Pylance가 뭔데.
VS코드의 확장 프로그램, Pylance 가 이 일을 맡아준다.<br>
파이랜스에게 프로젝트 환경에 맞는 인터프리터를 물려주면, <br>
해당 프로젝트가 사용하는 라이브러리들의 소스, <br>
또는 소스의 타입힌트파일들(.pyi)을 읽어 색인한 뒤,<br>
우리가 코드를 쓸 때, 그 정보를 바탕으로 조언을 준다.<br>

파이랜스가 없으면 VS코드는 자동완성 기능을 제공할 수 없다.<br>
고로, 쾌적한 파이썬 개발환경을 만들기 위해선 우선 Pylance 부터 설치하자.



# Ruff
## 포매터Formatter 와 린터Linter 는 무엇인가?
코드 예쁘게 쓰라고 타박하는 시어머니들이다.<br>
린터는 뭔가 잘못된 게 보이면 노란 줄을 긋는다. (안 쓴 변수나, 안 쓴 라이브러리)<br>
포매터는 따옴표 양식이나 줄바꿈, 들여쓰기를 맞춰준다.

## 왜 Ruff를 쓰는가?
예전에는 Flake8(린터), Black(포매터), isort(임포트 정렬)을 따로 썼다.<br>
하지만, 지금은 Ruff가 전부 통합해서 해준다.<br>
거기에, Rust기반이라 소스 읽고 교정해주는 속도가 정말 빠르다.

```sh
uv add --dev ruff

uv run ruff check .      # 전체 검사
uv run ruff check --fix  # 자동으로 고칠 수 있는 건 다 고치기
uv run ruff format .     # 코드 예쁘게 정렬하기
```

기본 설정이 잘 되어있어서 거의 손 볼 필요는 없지만, <br>
좀 빡빡하고 유도리가 없는 편이다. ruff.toml파일 손대서 좀 풀기 가능.

# 둘을 전부 적용한 VsCode 설정
## VS Code 설정도 바꾸자.
```
Ctrl + Shift + P
-> user settings json 검색
```
```JSON
//하단에 추가
{
  // 기본 포맷터를 Ruff로 설정
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.formatOnSave": true, // 저장 시 자동 포맷팅
    "editor.codeActionsOnSave": {
      "source.fixAll.ruff": "always", // 저장 시 자잘한 에러 자동 수정
      "source.organizeImports.ruff": "always" // 저장 시 import문 자동 정렬
    }
  },
  // 인텔리센스 강화
  "python.languageServer": "Pylance",
  "python.analysis.typeCheckingMode": "basic" // 타입 체크 활성화
  //기본값은 off인 경우가 많은데, 이를 basic으로만 바꿔도 내가 쓴 코드의 타입 오류를 Pylance가 미리 잡아줘서 런타임 에러를 줄여준다
}
```