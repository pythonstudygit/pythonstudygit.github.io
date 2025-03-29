---
title: BNF 표기법 해석하기
parent: 파이썬 고급 팁
nav_order: 2
---

# BNF 표기법 해석하기

## BNF 표기법 개념

[The Python Language Reference](https://docs.python.org/3/reference/compound_stmts.html#grammar-token-python-grammar-suite)
에 작성되어 있는 어휘 분석<sub>lexical analysis</sub>와 구문<sub>syntax</sub>를 이해하기 위해서 BNF<sup>Backust-Naur Form</sup> 표기법을 이해할 필요가 있다.
이 표기법은 다음과 같은 정의 스타일을 따른다.

1. 각 규칙은 규칙에 의해 정의된 이름과 `::=` 로 시작한다.
2. 수직 바는 `|` 선택지를 구분하는 데 사용되며, 이 표기법에서 가장 낮은 결합력을 갖는다.
3. `*`는 바로 앞의 항목이 0 회 이상 반복됨을 의미한다.
4. `+`는 바로 앞의 항목이 1 회 이상 반복됨을 의미한다.
5. `[]`안에 있는 구분은 0 회 또는 1회 발생(즉, 선택사항)을 나타낸다.
6. 리터럴 문자열은 따옴표`""`로 둘러 싸인다.
7. 공백은 토큰을 구분하기 위해서만 의미를 가지며, 규칙은 보통 한 줄에 작성되지만 선택지가 많은 경우 첫 번째 줄 이후 각 줄이 수직 막대(`|`)로 시작할 수 있다.

어휘 정의에서는 두 가지 추가 규칙이 사용된다.

* **범위 지정:** 두 개의 리터럴 문자가 세 개의 점(`"a"..."z"`)으로 구분된 경우, 주어진 ASCII 문자 범위 내의 임의의 단일 문자를 의미한다.
* **비공식적 설명:** 꺾쇠 괄호(`<...>`)안의 구문은 정의된 기호<sub>symbol</sub>에 대한 비공식적 설명을 제공한다. 예를 들어, 필요에 따라 제어 문자<sub>control character</sub>라는 개념을 설명할 때 사용할 수 있다. 

```
name      ::= lc_letter (lc_letter | "_")*
lc_letter ::= "a"..."z"
```

위의 예시는 첫 번째 줄 name 이 하나의 소문자(lc_letter)로 시작하고, 그 뒤에 소문자나 밑줄`_`이 0 회 이상 반복되는 패턴임을 나타낸다.
여기서 lc_letter 는 'a' 부터 'z' 까지의 단일 문자 중 하나를 의미한다.

## 복합문<sub>Compound statements</sub> BNF 표기 해석

```
compound_stmt ::= if_stmt
                  | while_stmt
                  | for_stmt
                  | try_stmt
                  | with_stmt
                  | match_stmt
                  | funcdef
                  | classdef
                  | async_with_stmt
                  | async_for_stmt
                  | async_funcdef
suite         ::= stmt_list NEWLINE | NEWLINE INDENT statement+ DEDENT
statement     ::= stmt_list NEWLINE | compound_stmt
stmt_list     ::= simple_stmt (";" simple_stmt)* [";"]
```

* `compound_stmt`: 복합문으로 사용할 수 있는 여러 종류의 문을 나타낸다.
* `stmt_list`: 한 줄에 작성된 하나 이상의 단순문<sub>simple statement</sub>들을 나타낸다.
* `statement`: 하나의 실행 단위가 되는 문장을 의미한다.
* `suite`: 복합문에서 실행될 여러 문장의 블록을 의미한다.