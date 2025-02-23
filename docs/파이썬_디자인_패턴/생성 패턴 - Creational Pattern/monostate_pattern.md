---
title: 보그 패턴 - Borg Pattern
parent: 생성 패턴 - Creational Pattern
nav_order: 1
---
# 보그 패턴 (Borg Pattern)  
## 패턴 개요 
보그 패턴은 모든 객체가 같은 상태를 공유하는 패턴이다.  
이렇게 하면 여러 객체를 생성하면서도 공통의 설정이나 데이터를 유지할 수 있다.  
보그 패턴은 모노스테이트 싱글톤 패턴(Monostate Singlton Pattern)이라고 부르기도 한다.

### \_\_init\_\_ 으로 구현하기  
**Code**
```python
class Borg:
    _shared_state = {}
    
    def __init__(self):
        self.x = 1
        self.__dict__ = self._shared_state

b0 = Borg()
b1 = Borg()
b0.x = 4

print(b0 is b1)  # b0 과 b1 는 서로 다른 객체이다.
print("Object State 'b0':", b0.__dict__)
print("Object State 'b1':", b1.__dict__)
```
**Result**
```
False
Object State 'b0': {'1': '2', 'x': 4}
Object State 'b1': {'1': '2', 'x': 4}
```
### \_\_new\_\_ 로 구현하기  
**Code**
```python
class Borg:
    _shared_state = {}
    def __new__(cls, *args, **kwargs):
        obj = super(Borg, cls).__new__(cls)
        obj.__dict__ = cls._shared_state
        return obj
```

### 상속을 통한 상태 분리
**Code**
```python
class Borg:
    _shared_state = {}
    def __new__(cls, *args, **kwargs):
        obj = super(Borg, cls).__new__(cls)
        obj.__dict__ = cls._shared_state
        return obj
    
class GlobalConfig(Borg):
    
    _shared_state = {}  # Borg 클래스의 _shared_state 와 분리하기 위함.
    
    def __init__(self, config_value = None):
        # super 함수로 Borg 클래스의 __init__ 메소드를 오바리이딩
        # 이렇게 함으로 써 __dict__ 가 올바른 공유 상태로 재할당되게 된다.
        super().__init__()  
        if config_value is not None:
            self.config_value = config_value
            
class TestGlobalConfig(GlobalConfig):
    _shared_state = {}  # GlobalConfig 의 _shared_state 와 분리하기 위함

# TestGlobalConfig 는 GlobalConfig 의 속성에 영향을 주지 않음.
# Monostate 패턴은 Singleton 보다 유연해 테스트 환경에 사용하기 적합하다.
production_config = GlobalConfig(42)
print("production_config value:", production_config.config_value)
    
test_config0 = TestGlobalConfig(100)
test_config1 = TestGlobalConfig()

print(f"test_config0 value: {test_config0.config_value}")
print(f"test_config1 value: {test_config1.config_value}")

print(f"production_config value: {production_config.config_value}")
```

**Result**
```
production_config value: 42
test_config0 value: 100
test_config1 value: 100
production_config value: 42
```
### 모킹(Mocking)으로 메소드 오버라이딩  
**Code**
```python
# Borg 패턴
class Borg:
    _shared_state = {}
    
    def __init__(self):
        self.__dict__ = self._shared_state

# Production 환경에서의 Service
class Service(Borg):
    _shared_state = {}  # Production 환경의 공유 상태
    
    def __init__(self):
        
        super().__init__()
        
        if not hasattr(self, "data"):
            self.data = "Real Service Data"
                
    def get_data(self):
        return self.data
    
service = Service()
print("Production Service Data:", service.get_data())

# Test 환경에서는 Service 를 상속받아 mocking 한 버전을 생성
class MockService(Service):
    _shared_state = {}
    
    def get_data(self):
        return "Mocked Data"

mock_service = MockService()
print(f"Mock Service data: {mock_service.get_data()}")
```

**Result**
```
Production Service Data: Real Service Data
Mock Service data: Mocked Data
```