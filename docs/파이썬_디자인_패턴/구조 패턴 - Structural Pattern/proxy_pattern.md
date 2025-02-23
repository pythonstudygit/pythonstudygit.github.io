---
title: 프록시 패턴 - Proxy Pattern
parent: 구조 패턴 - Structural Pattern 
nav_order: 1
---
# 프록시 패턴 (Proxy Pattern)  

## 패턴 개요  

## 코드 예제1  
**Code**
```python
class Actor:
    def __init__(self):
        self.is_busy = False
        
    def occupied(self):
        self.is_busy = True
        print(type(self).__name__, "is occupied with current movie")
        
    def available(self):
        self.is_busy = False
        print(type(self).__name__, "is free for the movie")
    
    def get_status(self):
        return self.is_busy
    
class Agent:
    def __init__(self):
        self.principal = None
    
    def work(self):
        self.actor = Actor()
        
        if self.actor.get_status():
            self.actor.occupied()
            
        else:
            self.actor.available()
    
agent = Agent()
agent.work()
```

**Result**
```commandline
Actor is free for the movie
```

## 사용 사례  
