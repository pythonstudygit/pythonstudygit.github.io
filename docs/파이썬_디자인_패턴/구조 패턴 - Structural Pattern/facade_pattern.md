---
title: 퍼사드 패턴 - Facade Pattern
parent: 구조 패턴 - Structural Pattern 
nav_order: 1
---
# 퍼사드 패턴 (The Facade Pattern)  
## 구조 디자인 패턴 개요  
* 객체와 클래스를 병합해 더 큰 구조를 만든다.
* 개체의 관계를 더 쉽게 식별할 수 있는 디자인 패턴이다.
* 클래스 패턴은 상속을 통해 추상화해 인터페이스를 제공하는 반면에 객체 패턴은 한 개의 객체를 더 큰 객체로 확장시킨다.  
  구조 패턴은 클래스 패턴과 객체 패턴을 합친 패턴이다.

## 구조 패턴의 몇 가지 예  

* **어댑터 패턴(The Adapter Pattern)**: 클라이언트의 요구에 따라 특정 인터페이스를 다른 인터페이스에 맞춘다. 서로 다른 클래스의 인터페이스를 목적에 맞춰 변환한다.
* **브릿지 패턴(The Bridge Pattern)**: 객체의 인터페이스와 구현을 분리해 독립적으로 동작할 수 있게 한다.
* **데코레이터 패턴(The Decorator Pattern)**: 런타임에 객체의 책임을 덧붙인다. 인터페이스를 통해 객체에 속성을 추가한다.

## 퍼사드 디자인 패턴 개요  
* 서브시스템의 인터페이스를 통합시킨 단일 인터페이스를 제공해 클리언트가 쉽게 서브시스템에 접근할 수 있게 한다.
* 단일 인터페이스 객체로 복잡한 서브시스템을 대체한다. 서브시스템을 캡슐화하는 것이 아니라 모든 서브시스템들을 결합한다.
* 클라이언트와 내부 구현을 분리한다.

퍼사드 패턴은 복잡한 서브시스템을 간단한 인터페이스를 통해 제어할 수 있도록 만들어준다.  
**Code**
```python
class Hotelier:
    def __init__(self):
        print("Arranging the Hotel for Marriage?")
    
    def __isAvailable(self):
        print("Is the Hotel free for the event on given day?")
        return True
    
    def book_hotel(self):
        if self.__isAvailable():
            print("Registered the Booking\n\n")

class Florist:
    def __init__(self):
        print("Flower Decorations for the Event? --")
    
    def set_flower_requirement(self):
        print("Carnations, Roses and Lilies would be used for Decorations\n\n")
        
class Caterer:
    def __init__(self):
        print("Food Arrangements for the Event --")
    
    def set_cuisine(self):
        print("Chinese & Continental Cuisine to be served\n\n")
    
class Musician:
    def __init__(self):
        print("Musical Arrangements for the Marriage --")
    
    def set_music_type(self):
        print("Jazz and Classical will be played\n\n")

class EventManager:
    
    def __init__(self):
        print("Event Manager:: Letme talk to the folks\n")
    
    def arrange(self):
        self.hotelier = Hotelier()
        self.hotelier.book_hotel()
        
        self.florist = Florist()
        self.florist.set_flower_requirement()
        
        self.caterer = Caterer()
        self.caterer.set_cuisine()
        
        self.musician = Musician()
        self.musician.set_music_type()
        
class You:
    def __init__(self):
        print("You:: Whoa! Marriage Arrangements?!!!")
    
    def ask_event_manager(self):
        print("You:: Let's Contact the Event Manager\n\n")
        
        em = EventManager()
        em.arrange()
    
    def __del__(self):
        print("You:: Thanks to Event Manager, all preparations done! Phew!")
        
you = You()
you.ask_event_manager()
```

**Result**
```
You:: Whoa! Marriage Arrangements?!!!
You:: Let's Contact the Event Manager


Event Manager:: Letme talk to the folks

Arranging the Hotel for Marriage?
Is the Hotel free for the event on given day?
Registered the Booking


Flower Decorations for the Event? --
Carnations, Roses and Lilies would be used for Decorations


Food Arrangements for the Event --
Chinese & Continental Cuisine to be served


Musical Arrangements for the Marriage --
Jazz and Classical will be played


You:: Thanks to Event Manager, all preparations done! Phew!
```
You 클래스 (클라이언트) 는 EventManager 를 통해 결혼 준비를 요청한다.  
EventManager 클래스는 내부적으로 서브시스템 클래스(Hotelier, Florist, Caterer, Musician)를 생성 및 호출하여, 클라이언트가 복잡한 내부 로직을 알 필요 없이 간단한게 이벤트를 진행할 수 있게 한다.  