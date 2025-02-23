---
title: 추상 팩토리 패턴 - Abstract Factory Pattern
parent: 생성 패턴 - Creational Pattern
nav_order: 4
---
# 추상 팩토리 패턴 (Abstract Factory Patter)  

## 패턴 개요  

## 코드 예제1  
**Code**
```python
from abc import ABC, abstractmethod
from typing import Type

class VegPizza(ABC):
    @abstractmethod
    def prepare(self):
        pass
    
class NonVegPizza(ABC):
    @abstractmethod
    def serve(self):
        pass

class DeluxVeggiePizza(VegPizza):
    def prepare(self) -> None:
        print(f"Prepare: {type(self).__name__}")

class ChikenPizza(NonVegPizza):
    def serve(self, veg_pizza) -> None:
        print(f"{type(self).__name__}, is served with Chicken on {type(veg_pizza).__name__}")

class MexicanVegPizza(VegPizza):
    def prepare(self) -> None:
        print(f"Prepare {type(self).__name__}")

class HamPizza(NonVegPizza):
    def serve(self, veg_pizza) -> None:
        print(f"{type(self).__name__}, is served with Ham on {type(veg_pizza).__name__}")

class PizzaFactory(ABC):
    
    @abstractmethod
    def create_veg_pizza(self) -> Type[VegPizza]:
        pass
    
    @abstractmethod
    def create_nonveg_pizza(self) -> Type[NonVegPizza]:
        pass

class IndianPizzaFactory(PizzaFactory):
    def create_veg_pizza(self):
        return DeluxVeggiePizza()
    
    def create_nonveg_pizza(self):
        return ChikenPizza()

class USPizzaFactory(PizzaFactory):
    def create_veg_pizza(self):
        return MexicanVegPizza()

    def create_nonveg_pizza(self):
        return HamPizza()

class PizzaStore:    
    def make_pizzas(self):
        for factory in [IndianPizzaFactory(), USPizzaFactory()]:
            factory: PizzaFactory
            
            self.non_veg_pizza = factory.create_nonveg_pizza()
            self.veg_pizza = factory.create_veg_pizza()
            self.veg_pizza.prepare()
            self.non_veg_pizza.serve(self.veg_pizza)

pizza = PizzaStore()
pizza.make_pizzas()
```
**Result**
```
Prepare: DeluxVeggiePizza
ChikenPizza, is served with Chicken on DeluxVeggiePizza
Prepare MexicanVegPizza
HamPizza, is served with Ham on MexicanVegPizza
```