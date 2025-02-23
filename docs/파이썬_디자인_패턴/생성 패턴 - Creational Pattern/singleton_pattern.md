---
title: 싱글톤 패턴 - Singleton Pattern
parent: 생성 패턴 - Creational Pattern
nav_order: 1
---
# 싱글톤 패턴 (Singleton Pattern)
## 패턴 개요  
* 클래스에 대한 단일 객체 생성
* 전역 객체 제공
* 공유된 리소스에 대한 동시 접근 제어 (충돌 방지)

## 파이썬 모듈과 비교  
파이썬 모듈은 기본적으로 싱글톤이다.  
1. 파이썬 모듈이 임포트 되었는지 확인
2. 이미 임포트 된 경우, 해당 모듈의 객체를 반환한다.
3. 임포트되지 않은 경우 임포트 후 초기화 한다.
4. 모듈은 인포트와 동시에 초기화된다.
5. 모듈을 다시 임포트하면 초기화되지 않는다.

## 코드 예제1 - \_\_new\_\_ 메소드 오버라이딩  
**Code**
```python
# 일반 빈 클래스
class MyClass:
    pass

c0 = MyClass()
c1 = MyClass()

print(c0 is c1)

class Singleton:
    def __new__(cls):
        if not hasattr(cls, 'instance'):  # 클래스에 instance 속성이 있는지 확인
            # 인스턴스가 없으면 __new__ 메소드를 오버라이드 해 객체를 생성한다.
            cls.instance = super(Singleton, cls).__new__(cls)   
        return cls.instance # 이미 객체가 생성된 경우 해당 인스턴스를 반환한다.
    
s0 = Singleton()
s1 = Singleton()

print(s0 is s1)
```
**Result**
```
False
True
```
## 코드 예제2 - 게으른 초기화 (Lazy instantiation)  
**Code**
```python
# Lazy instantiation
class Singleton:
    __instance = None
    def __init__(self):
        if Singleton.__instance is None:
            print("__init__ method called..")
        else:
            print("Instance already created:", self.get_instance())
    
    # get_instance 메소드 호출 시에만 인스턴스 생성
    @classmethod
    def get_instance(cls):
        if not cls.__instance:
            cls.__instance = Singleton()
        return cls.__instance
  
s0 = Singleton()  # 클래스를 초기화했지만 객체는 생성하지 않는다.
s1 = Singleton.get_instance()  # 객체 생성
s2 = Singleton.get_instance()
s3 = Singleton()

print(s0 is s1)
print(s1 is s2)
print(s2 is s3)
```
**Result**  
```
__init__ method called..
__init__ method called..
Instance already created: <__main__.Singleton object at 0x0000017B1AE17B90>
False
True
False
```

**Code**
```python
import test_module
import test_module as tm

print(test_module is tm)
```

**Result**
```
True
```

## 코드 예제3 - 메타클래스를 활용하는 방법  
**Code**
```python
class MetaSingleton(type):
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(
                MetaSingleton,
                cls
            ).__call__(*args, **kwargs)
        return cls._instances[cls]

class Logger(metaclass=MetaSingleton):
    pass

logger0 = Logger()
logger1 = Logger()

print(logger0 is logger1)
```
**Result**
```
True
```

## 코드 예제4   
**Code**
```python
import sqlite3

class MetaSingleton(type):
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(
                MetaSingleton,
                cls
            ).__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=MetaSingleton):
    connection = None
    
    def connect(self):
        if self.connection is None:
            self.connection = sqlite3.connect("db.sqlite3")
            self.cursorobj = self.connection.cursor()
            
        return self.cursorobj

db0 = Database().connect()
db1 = Database().connect()

print(db0 is db1)
```

**Result**
```
True
```

## 코드 예제5  
**Code**
```python
class MetaSingleton(type):
    _instances = {}
    
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(
                MetaSingleton,
                cls
            ).__call__(*args, **kwargs)
        return cls._instances[cls]

class HealthCheck(metaclass=MetaSingleton):
    def __init__(self):
        self._servers = []
    
    def add_server(self):
        self._servers.append("Server 1")
        self._servers.append("Server 2")
        self._servers.append("Server 3")
        self._servers.append("Server 4")
    
    def change_server(self):
        self._servers.pop()
        self._servers.append("Server 5")

health0 = HealthCheck()
health1 = HealthCheck()

health0.add_server()
print("Schedule health check for servers (1)..")
for i in range(4):
    print("Checking ", health0._servers[i])

health1.change_server()
print("Schedule health check for servers (2)..")
for i in range(4):
    print("Checking ", health1._servers[i])
```

**Result**
```
Schedule health check for servers (1)..
Checking  Server 1
Checking  Server 2
Checking  Server 3
Checking  Server 4
Schedule health check for servers (2)..
Checking  Server 1
Checking  Server 2
Checking  Server 3
Checking  Server 5
```