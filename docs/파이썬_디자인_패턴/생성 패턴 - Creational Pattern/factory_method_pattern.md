---
title: 팩토리 메소드 패턴 - Factory Method Pattern
parent: 생성 패턴 - Creational Pattern
nav_order: 3
---
<!--
# 팩토리 패턴 (Factory Pattern)  
## 패턴 개요  
* 다른 클래스의 객체를 생성하는 클래스
* 객체 생성과 클래스 구현을 나눠 상호 의존도를 줄인다.
* 클라이언트는 생성하려는 객체 클래스 구현과 상관없이 사용할 수 있다.
* 코드를 수정하지 않고 간단하게 팩토리에 새로운 클래스를 추가할 수 있다.
* 이미 생성된 객체를 팩토리가 재활용할 수 있다.
-->
# 팩토리 메소드 패턴 (Factory Method Pattern)

## 패턴 개요
팩토리 메소드 패턴은 객체를 생성하는 인터페이스르 정의하고 어떤 클래스를 초기화할지는 서브 클래스의 결정을 맡긴다.  

## 코드 예제1  
**Code**
```python
from abc import ABC, abstractmethod
from typing import Dict, Type

# Product
class Section(ABC):
    @abstractmethod
    def describe(self):
        pass

# ConcreteProduct
class PersonalSection(Section):
    def describe(self) -> None:
        print("Personal Section")

class AlbumSection(Section):
    def describe(self) -> None:
        print("AlbumSection")
        
class PatentSection(Section):
    def describe(self) -> None:
        print("Patent Section")

class PublicationSection(Section):
    def describe(self) -> None:
        print("Publication Section")

# Creator
# 프로필을 구성하는 인터페이스를 정의한다.
# create_profile() 메서드를 선언하여 하위 클래스가 이를 구현하도록 강제한다.
class Profile(ABC):
    def __init__(self):
        self.sections = []
        self.create_profile()
    
    @abstractmethod
    def create_profile(self):
        pass
    
    def get_sections(self):
        return self.sections
        
    def add_section(self, section):
        self.sections.append(section)

# Profile 하위 클래스를 등록할 딕셔너리
profile_registry: Dict[str, Type[Profile]] = {}

# 동적으로 적절한 클래스의 객체를 생성하기 위한 데코레이터
# 클래스의 이름을 key 로, value 로 클래스를 register_profile 에 등록한다.
def register_profile(cls: Type[Profile]) -> Type[Profile]:
    profile_registry[cls.__name__] = cls
    return cls

# ConcreteCreator
# Profile 의 구체적인 구현체
# 각각의 클래스는 create_profile() 메서드를 구현하여 자신에게 맞는 Section 을 추가한다.
@register_profile
class LinkedIn(Profile):
    def create_profile(self):
        self.add_section(PersonalSection())
        self.add_section(PatentSection())
        self.add_section(PublicationSection())
    
@register_profile
class FaceBook(Profile):
    def create_profile(self):
        self.add_section(PersonalSection())
        self.add_section(AlbumSection())

profile_type = input("Which Profile you'd like to create? [LinkedIn or FaceBook]? ").lower()
profile = None

if profile_type == "linkedin":
    profile = profile_registry["LinkedIn"]()  # LinkedIn 객체 생성
elif profile_type == "facebook":
    profile = profile_registry["FaceBook"]()  # FaceBook 객체 생성
else:
    raise ValueError(f"Unknown Profile Type: {profile_type}")

print("Creating Profile..", type(profile).__name__)
print("Profile has sections --", profile.get_sections())
```

**Result**
```
Which Profile you'd like to create? [LinkedIn or FaceBook]? facebook
Creating Profile.. FaceBook
Profile has sections -- [<__main__.PersonalSection object at 0x0000024DBA01E210>, <__main__.AlbumSection object at 0x0000024DBA02C590>]
```