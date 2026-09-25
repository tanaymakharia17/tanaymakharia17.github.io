---
title: 'SOLID Principles'
date: 2026-09-25
permalink: /posts/2026/09/solid-principles/
tags:
  - solid
  - design-principles
  - oop
  - clean-code
  - software-engineering
---

**SOLID** is a set of five design principles for writing object-oriented code that is easy to change. The acronym was popularized by Robert C. Martin (Uncle Bob), and the ideas build on the [Clean Code mindset](/posts/2025/07/why-clean-code-matters/): code is read far more often than it is written, so structure matters.

Each principle answers a different question:

| Letter | Principle | The question it answers |
|--------|-----------|--------------------------|
| **S** | Single Responsibility | Does this class do one job? |
| **O** | Open/Closed | Can I add behaviour without editing existing code? |
| **L** | Liskov Substitution | Can a subclass safely replace its parent? |
| **I** | Interface Segregation | Am I forcing classes to implement things they don't need? |
| **D** | Dependency Inversion | Do I depend on abstractions, not concrete details? |

Let's take them one at a time, with bad and good examples.

---

## 1. Single Responsibility Principle (SRP)

### Definition

A class should have **only one reason to change** — it should have one job or responsibility.

### Real-life analogy

A chef cooks; a waiter serves. You don't hire one person to do both, because a change to the menu and a change to the seating plan would collide.

```python
class Chef:
    def cook(self):
        print("Cooking food.")

class Waiter:
    def serve(self):
        print("Serving food.")
```

### Bad design

One class handling salary logic, database access, and reporting — three reasons to change.

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def calculate_salary(self):
        return self.salary

    def save_to_database(self):
        print("Saving employee to DB")

    def generate_report(self):
        print("Generating report")
# Salary logic + database logic + reporting logic — all in one class.
```

### Good design

Split each responsibility into its own class:

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

class SalaryCalculator:
    def calculate(self, employee):
        return employee.salary

class EmployeeRepository:
    def save(self, employee):
        print("Saving employee to DB")

class ReportGenerator:
    def generate(self, employee):
        print("Generating report")
```

**Takeaway:** if a class description needs the word "and", it probably has too many responsibilities.

---

## 2. Open/Closed Principle (OCP)

### Definition

Software entities (classes, modules, functions) should be **open for extension but closed for modification**. You should be able to add new behaviour without editing existing, tested code.

### Real-life analogy

You can install new apps on your phone without modifying the phone's operating system.

### Bad design

Every new payment type means editing `PaymentProcessor` — and re-testing all the old cases.

```python
class PaymentProcessor:
    def process(self, payment_type, amount):
        if payment_type == "credit":
            print("Processing credit card")
        elif payment_type == "paypal":
            print("Processing PayPal")
        # every new payment type edits this method
```

### Good design

Define an abstraction, then add new types by extension:

```python
from abc import ABC, abstractmethod

class PaymentMethod(ABC):
    @abstractmethod
    def process(self, amount):
        pass

class CreditCard(PaymentMethod):
    def process(self, amount):
        print("Processing credit card")

class PayPal(PaymentMethod):
    def process(self, amount):
        print("Processing PayPal")

class PaymentProcessor:
    def process(self, payment_method, amount):
        payment_method.process(amount)

# Adding UPI requires no change to existing code
class UPI(PaymentMethod):
    def process(self, amount):
        print("Processing UPI")
```

**Takeaway:** extend by adding new classes, not by editing old ones.

---

## 3. Liskov Substitution Principle (LSP)

### Definition

A subclass should be **replaceable for its base class without breaking the program**. If code works with a `Bird`, it must still work when given any kind of `Bird`.

### Real-life analogy

A new model of a toy car should work with all the same tracks as the old one — same shape, same connections.

### Bad design

`Penguin` *is-a* `Bird`, but it breaks the contract that birds can fly. Code that expects to call `fly()` crashes.

```python
class Bird:
    def fly(self):
        print("Flying")

class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins can't fly")

def make_fly(bird: Bird):
    bird.fly()

make_fly(Penguin())   # breaks — the substitution failed
```

### Good design

Model the hierarchy so that only birds that can fly have `fly()`:

```python
class Bird:
    pass

class FlyingBird(Bird):
    def fly(self):
        print("Flying")

class Sparrow(FlyingBird):
    pass

class Penguin(Bird):
    def swim(self):
        print("Swimming")

def make_fly(bird: FlyingBird):   # now the type is honest
    bird.fly()
```

**Takeaway:** a subclass must honour the promises of its parent — no surprising exceptions.

---

## 4. Interface Segregation Principle (ISP)

### Definition

Clients should **not be forced to depend on methods they do not use**. Prefer several small, focused interfaces over one large one.

### Real-life analogy

A basic printer only needs to print. It shouldn't be forced to offer scan and fax it can't do.

### Bad design

One fat `Machine` interface forces `BasicPrinter` to implement methods that make no sense.

```python
from abc import ABC, abstractmethod

class Machine(ABC):
    @abstractmethod
    def print(self): ...

    @abstractmethod
    def scan(self): ...

    @abstractmethod
    def fax(self): ...

class BasicPrinter(Machine):
    def print(self):
        print("Printing")

    def scan(self):
        raise NotImplementedError   # forced, unwanted

    def fax(self):
        raise NotImplementedError   # forced, unwanted
```

### Good design

Split the big interface into small ones, and combine them only where needed:

```python
from abc import ABC, abstractmethod

class Printable(ABC):
    @abstractmethod
    def print(self): ...

class Scannable(ABC):
    @abstractmethod
    def scan(self): ...

class BasicPrinter(Printable):
    def print(self):
        print("Printing document.")

class MultiFunctionPrinter(Printable, Scannable):
    def print(self):
        print("Printing document.")

    def scan(self):
        print("Scanning document.")
```

**Takeaway:** if an implementation has methods that only `raise NotImplementedError`, the interface is too big.

> Notice that LSP and ISP often show up together: both are about not forcing classes to handle behaviour they can't support.

---

## 5. Dependency Inversion Principle (DIP)

### Definition

**High-level modules should not depend on low-level modules; both should depend on abstractions.** In practice: depend on interfaces, not concrete classes.

### Real-life analogy

A wall switch depends on "something that can turn on", not on a specific bulb. Swap the LED for a fan and the switch still works.

### Bad design

`Switch` directly creates a `LightBulb`, so it's welded to that one device.

```python
class LightBulb:
    def turn_on(self):
        print("Bulb on")

class Switch:
    def __init__(self):
        self.bulb = LightBulb()   # depends on a concrete class

    def operate(self):
        self.bulb.turn_on()
```

### Good design

Both the switch and the devices depend on a `Switchable` abstraction:

```python
from abc import ABC, abstractmethod

class Switchable(ABC):
    @abstractmethod
    def turn_on(self): ...

class LightBulb(Switchable):
    def turn_on(self):
        print("Bulb on")

class Fan(Switchable):
    def turn_on(self):
        print("Fan on")

class Switch:
    def __init__(self, device: Switchable):   # depends on the abstraction
        self.device = device

    def operate(self):
        self.device.turn_on()

switch = Switch(LightBulb())
switch.operate()

switch2 = Switch(Fan())
switch2.operate()
```

**Takeaway:** pass dependencies in (constructor injection) and type them as abstractions, not concrete classes.

---

## How the Principles Fit Together

- **SRP** keeps each class focused on one job.
- **OCP** lets you add features by writing new classes.
- **LSP** guarantees those new subclasses don't break existing code.
- **ISP** keeps interfaces small so classes only implement what they can.
- **DIP** glues it together by depending on abstractions, so implementations can be swapped freely.

They overlap on purpose. Following one often nudges you toward the others.

## Key Takeaways

1. **SRP** — one class, one reason to change.
2. **OCP** — add new behaviour by extending, not by editing.
3. **LSP** — a subclass must be usable anywhere its parent is.
4. **ISP** — many small interfaces beat one big one.
5. **DIP** — depend on abstractions, inject the concrete details.
6. SOLID is a **guide, not a law** — apply it where change is likely, and don't over-engineer simple code.

## Mini Glossary

| Term | Meaning |
|------|---------|
| Abstraction | An interface or abstract class that hides concrete details |
| Interface | A set of method signatures a class promises to implement |
| Concrete class | An actual, instantiable implementation |
| Substitution | Using a subclass wherever the parent type is expected |
| Constructor injection | Passing dependencies into an object's constructor |
| Extend | Add behaviour without modifying existing code |