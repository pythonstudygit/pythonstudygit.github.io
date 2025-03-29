---
title: curses 모듈
nav_order: 2
parent: 내장_라이브러리
---

# curses 모듈

`curses` 모듈은 텍스트 기반 터미널을 위한 독립적인 화면 그리기 및 키보드 처리 기능을 제공한다.

> {: .Note }  
> Windows 버전의 Python 에는 `curses` 모듈이 포함되어 있지 않다.

## `curses` 애플리케이션 시작 및 종료

`curses` 로 작업을 하기 전에 `curses` 를 초기화해야 한다. initscr() 함수를 호출하여 초기화를 할 수 있고 기능은 다음과 같다.

* 터미널 유형 결정
* 터미널에 필요한 설정 코드를 보내고 다양한 내부 데이터 구조를 생성

초기화에 성공하면 `initscr()` 은 전체 화면을 나타내는 창 객체를 반환한다.
일반적으로 `initscr()` 반환값을 받는 변수 이름을 `stdscr` 로 짓는다.

```python
import curses
stdscr = curses.initscr()
```

보통의 경우 `curses` 애플리케이션은 특정 상황에서만 키를 표시할 수 있도록 화면에 키 자동 에코를 끈다.

```python
curses.noecho()
```

애플리케이션은 일반적으로 Enter 키를 누르지 않고도 키에 즉시 반응해야한다.
이것을 cbreak 모드라고 하며, 일반적인 버퍼링된 입력 모드와 대조된다.

```python
curses.cbreak()
```

터미널은 보통 커서 키나 PageUp 및 Home 등의 탐색 키와 같은 특수 키를 다중 바이트 이스케이프 시퀀스로 반환한다.
이러한 시퀀스를 예상하고 그에 따라 처리하는 코드를 작성할 수도 있지만, `curses` 가 대신 처리하여 `curses.KEY_LEFT` 와 같은 특수 값을 반환할 수 있다.
`curses` 에서 이 작업을 수행하게 하려면 키패드 모드를 활성화해야 한다.

```python
stdscr.keypad(True)
```

`curses` 애플리케이션을 종료하는 것은 시작하는 것보다 쉽다. 다음을 호출해야 한다.

```python
curses.nocbreak()
stdscr.keypad(False)
curses.echo()
```

그 다음 `endwin()` 함수를 호출하여 터미널을 원래 작동 모드로 복원한다.

```python
curses.endwin()
```

`curses` 가 얘기치 않게 종료되면 터미널이 엉망이 될 수 있다.
python 에서는 `curses.wrapper()` 함수를 가져와서 이러한 복잡성을 피하고 디버깅을 훨씬 쉽게 만들 수 있다.

```python
import curses

def main(stdscr):
    stdscr.clear()

    for i in range(0, 11):
        v = i - 10
        stdscr.addstr(i, 0, f"10 divded by {v} is {10/v}")
        stdscr.refresh()
        stdscr.getkey()
```

wrapper() 함수는 호출 가능한 객체를 받아 위에서 설명한 초기화를 수행하며, 색상 지원이 있는 경우 색상도 초기화한다.
그런 다음 wrapper() 는 제공된 호출 가능한 객체를 실행한다.
호출 가능한 객체가 반환되면, wrapper() 는 터미널의 원래 상태를 복원한다.
호출 가능한 객체는 예외를 포착하고, 터미널의 상태를 복원한 다음, 예외를 다시 발생시키는 try...except 내에서 호출 된다.
따라서 예외 발생 시 터미널이 엉망이 되지 않으며, 예외 메시지와 트레이스백을 읽을 수 있다.

## 윈도우와 패드<sub>Windows and Pads</sub>

윈도우는 `curses` 의 기본 추상화 개념이다.
윈도우 객체는 화면의 직사각형 영역을 나타내며, 텍스트를 표시하고, 지우고, 사용자가 문자열을 입력하도록 허용하는 등의 메서드를 지원한다.

`initscr()` 함수가 반환하는 `stdscr` 객체는 전체 화면을 덮는 윈도우 객체이다.
많은 프로그램은 이 단일 창만 필요할 수 있지만, 화면을 작은 창으로 나누어 별도로 다시 그리거나 지우고 싶을 수도 있다.
`newwin()` 함수는 지정된 크기의 새 창을 만들고, 새 창 객체를 반환한다.

```python
begin_y = 7
begin_x = 20
height = 5
width = 40
win = curses.newwin(height, width, begin_y, begin_x)
```

> {: .Warning }  
> `curses` 의 좌표 시스템은 대부분의 다른 프로그램과 다르다.
> 일반적으로 x 좌표가 먼저 오는게 관행인데, curses 는 특이하게도 y, x 순서로 전달된다는 점에 유의해야 한다.

애플리케이션은 curses.LINES 와 curses.COLS 변수를 사용해 각각 y 와 x 의 크기를 확인할 수 있으며, 유효한 좌표는 (0, 0) 에서 (curses.LINES -1, curses.COLS -1) 까지이다.

텍스트를 표시하거나 지우는 메서드를 호출해도 즉시 화면에 반영되지 않고, `refresh()` 메서드를 호출해야 실제로 업데이트된다.
이는 원래 느린 300-baud 터미널 연결을 고려하여 변경 사항을 누적한 후 한 번에 반영하기 위한 방식이다.

패드는 일반 윈도우와 비슷하지만, 실제 디스플레이 화면보다 큰 영역을 가질 수 있고, 한 번에 그 일부만 화면에 표시할 수 있다.
패드를 생성할 때는 패드의 높이와 너비를 지정하고, `refresh()` 호출 시 pad 의 일부를 어느 화면 영역에 표시할지 좌표를 지정해야한다.

```python
pad = curses.newpad(100, 100)

for y in range(0, 99):
    for x in range(0, 99):
        pad.addch(y, x, ord('a') + (x * x + y * y) % 26)

pad.refresh(0, 0, 5, 5, 20, 75)
```

`refresh()` 는 패드의 (0, 0) 좌표부터 시작하는 부분을 화면의 (5, 5) 에서 (20, 75) 까지의 사각형 영역에 표시한다.
이 외의 면에서는 패드가 일반 윈도우와 동일하게 동작하며 같은 메서드를 지원한다.

여러 윈도우와 패드가 동시에 화면에 있을 경우, 개별적으로 `refresh()` 를 호출하면 화면 깜박임이 발생할 수 있다.
이를 방지하기 위해 각 윈도우의 `noutrefresh()` 메서드를 호출해 내부 데이터 구조를 업데이트한 뒤, `doupdate()` 함수를 호출하여 물리적 화면을 한 번에 갱신할 수 있다.



## 추가 자료