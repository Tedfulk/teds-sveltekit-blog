---
title: Behavioral Design Patterns in Software Development
slug: behavioral-design-patterns
coverImage: /images/posts/behavioral-design-patterns.jpg
date: 2024-04-24T00:00:00.000Z
excerpt: An in-depth look at behavioral design patterns in software development, exploring Visitor, Template Method, Strategy, State, Observer, Memento, Mediator, Iterator, Interpreter, Command, and Chain of Responsibility patterns with examples and real-world applications.
tags:
  - Design Patterns
  - Software Development
  - Programming
  - Python
  - Tutorial
---

<script>
  import Callout from "$lib/components/molecules/Callout.svelte";
  import CodeBlock from "$lib/components/molecules/CodeBlock.svelte";
  import Image from "$lib/components/atoms/Image.svelte";
</script>

## Introduction

Behavioral design patterns are crucial in software development as they help manage complex behaviors between objects, improve code flexibility, and promote separation of concerns. This post will cover the Visitor, Template Method, Strategy, State, Observer, Memento, Mediator, Iterator, Interpreter, Command, and Chain of Responsibility patterns.

## Visitor Pattern

### Introduction
The Visitor pattern allows you to add further operations to objects without having to modify them. This pattern is particularly useful for applying operations across a collection of objects with different interfaces.

### Definition
The Visitor pattern is a behavioral design pattern that lets you separate algorithms from the objects on which they operate by moving the logic of these algorithms into a visitor class.

### Use Cases
- When you need to perform many unrelated operations on a collection of objects with different interfaces
- To avoid polluting classes with unrelated behaviors

### Benefits
- Simplifies the addition of new operations
- Enhances code maintainability
- Allows for separation of concerns

### Examples

<CodeBlock lang="python">

```python
class Visitor:
    def visit(self, element):
        pass

class ConcreteVisitor1(Visitor):
    def visit(self, element):
        print(f"Visitor1: {element.operation()}")

class ConcreteVisitor2(Visitor):
    def visit(self, element):
        print(f"Visitor2: {element.operation()}")

class Element:
    def accept(self, visitor):
        pass

class ConcreteElementA(Element):
    def accept(self, visitor):
        visitor.visit(self)

    def operation(self):
        return "ElementA"

class ConcreteElementB(Element):
    def accept(self, visitor):
        visitor.visit(self)

    def operation(self):
        return "ElementB"

elements = [ConcreteElementA(), ConcreteElementB()]
visitor1 = ConcreteVisitor1()
visitor2 = ConcreteVisitor2()

for element in elements:
    element.accept(visitor1)
    element.accept(visitor2)
```

</CodeBlock>

### Real-world Applications
The Visitor pattern is used in compilers for traversing syntax trees and applying various operations, such as type checking and code generation, on different nodes of the tree.

## Template Method Pattern

### Introduction
The Template Method pattern defines the skeleton of an algorithm in a method, deferring some steps to subclasses. It allows subclasses to redefine certain steps of an algorithm without changing its structure.

### Definition
The Template Method pattern is a behavioral design pattern that defines the skeleton of an algorithm in a method, allowing subclasses to redefine specific steps without altering the algorithm's structure.

### Use Cases
- When you have an algorithm that can be divided into steps and some steps can be implemented differently by subclasses
- To prevent code duplication across similar algorithms

### Benefits
- Promotes code reuse
- Ensures the invariant parts of the algorithm are maintained
- Simplifies code maintenance

### Examples

<CodeBlock lang="python">

```python
from abc import ABC, abstractmethod

class AbstractClass(ABC):
    def template_method(self):
        self.base_operation1()
        self.required_operation1()
        self.base_operation2()
        self.required_operation2()

    # Base operations with a default implementation
    def base_operation1(self):
        print("AbstractClass: Base operation 1")

    def base_operation2(self):
        print("AbstractClass: Base operation 2")

    # Abstract methods to be implemented by subclasses
    @abstractmethod
    def required_operation1(self):
        pass

    @abstractmethod
    def required_operation2(self):
        pass

# Concrete implementations of AbstractClass
class ConcreteClassA(AbstractClass):
    def required_operation1(self):
        print("ConcreteClassA: Implemented Operation1")

    def required_operation2(self):
        print("ConcreteClassA: Implemented Operation2")

class ConcreteClassB(AbstractClass):
    def required_operation1(self):
        print("ConcreteClassB: Implemented Operation1")

    def required_operation2(self):
        print("ConcreteClassB: Implemented Operation2")

# Instantiate ConcreteClassA and execute the template method
class_a = ConcreteClassA()
class_a.template_method()

# Instantiate ConcreteClassB and execute the template method
class_b = ConcreteClassB()
class_b.template_method()
```

</CodeBlock>

### Real-world Applications
The Template Method pattern is commonly used in frameworks where the framework defines the skeleton of an algorithm, and the clients can extend or customize certain steps of the algorithm.

## Strategy Pattern

### Introduction
The Strategy pattern enables selecting an algorithm's behavior at runtime. It defines a family of algorithms, encapsulates each one, and makes them interchangeable.

### Definition
The Strategy pattern is a behavioral design pattern that allows defining a family of algorithms, encapsulating each one, and making them interchangeable without altering the client code.

### Use Cases
- When you need to use different variants of an algorithm within an object
- To isolate the algorithm's code from the context it operates in

### Benefits
- Promotes the Open/Closed Principle
- Reduces conditional statements
- Allows the behavior of an object to be changed at runtime

### Examples

<CodeBlock lang="python">

```python
class Strategy:
    def do_algorithm(self, data):
        pass

class ConcreteStrategyA(Strategy):
    def do_algorithm(self, data):
        return sorted(data)

class ConcreteStrategyB(Strategy):
    def do_algorithm(self, data):
        return sorted(data, reverse=True)

class Context:
    def __init__(self, strategy):
        self._strategy = strategy

    def set_strategy(self, strategy):
        self._strategy = strategy

    def execute_strategy(self, data):
        return self._strategy.do_algorithm(data)

data = [1, 3, 2, 5, 4]
context = Context(ConcreteStrategyA())
print(context.execute_strategy(data))

context.set_strategy(ConcreteStrategyB())
print(context.execute_strategy(data))
```

</CodeBlock>

### Real-world Applications
The Strategy pattern is often used in payment processing systems to select different payment methods, such as credit card, PayPal, or bank transfer, at runtime.

## State Pattern

### Introduction
The State pattern allows an object to alter its behavior when its internal state changes. This pattern is particularly useful for objects that need to change their behavior based on their state.

### Definition
The State pattern is a behavioral design pattern that allows an object to change its behavior when its internal state changes, making the object appear to change its class.

### Use Cases
- When an object must change its behavior at runtime depending on its state
- To simplify state transition logic

### Benefits
- Localizes state-specific behavior
- Simplifies state transition logic
- Promotes the Open/Closed Principle

### Examples

<CodeBlock lang="python">

```python
class State:
    def handle(self, context):
        pass

class ConcreteStateA(State):
    def handle(self, context):
        print("State A handling request.")
        context.state = ConcreteStateB()

class ConcreteStateB(State):
    def handle(self, context):
        print("State B handling request.")
        context.state = ConcreteStateA()

class Context:
    def __init__(self, state):
        self.state = state

    def request(self):
        self.state.handle(self)

context = Context(ConcreteStateA())
context.request()
context.request()
```

</CodeBlock>

### Real-world Applications
The State pattern is used in UI frameworks to manage the state of UI elements, such as buttons that change behavior based on their state (e.g., enabled, disabled, pressed).

## Observer Pattern

### Introduction
The Observer pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

### Definition
The Observer pattern is a behavioral design pattern that establishes a one-to-many dependency between objects, allowing one object to notify its dependents about state changes.

### Use Cases
- When changes in one object need to be propagated to multiple dependent objects
- To implement event handling systems

### Benefits
- Promotes loose coupling between objects
- Supports the principle of separating concerns
- Simplifies the maintenance of systems with multiple observers

### Examples

<CodeBlock lang="python">

```python
class Subject:
    def __init__(self):
        self._observers = []

    def attach(self, observer):
        self._observers.append(observer)

    def detach(self, observer):
        self._observers.remove(observer)

    def notify(self):
        for observer in self._observers:
            observer.update(self)

class ConcreteSubject(Subject):
    def __init__(self):
        super().__init__()
        self._state = None

    @property
    def state(self):
        return self._state

    @state.setter
    def state(self, value):
        self._state = value
        self.notify()

class Observer:
    def update(self, subject):
        pass

class ConcreteObserver(Observer):
    def update(self, subject):
        print(f"Observer: Subject's state is now {subject.state}")

subject = ConcreteSubject()
observer = ConcreteObserver()
subject.attach(observer)
subject.state = 10
```

</CodeBlock>

### Real-world Applications
The Observer pattern is widely used in event-driven systems, such as user interfaces where UI elements need to be updated when the underlying data changes.

## Memento Pattern


### Introduction
The Memento pattern provides the ability to restore an object to its previous state. This pattern is useful for implementing undo functionality.

### Definition
The Memento pattern is a behavioral design pattern that allows you to capture and externalize an object's internal state without violating encapsulation, so the object can be restored to this state later.

### Use Cases
- When you need to save and restore the state of an object
- To implement undo/redo functionality

### Benefits
- Preserves encapsulation boundaries
- Simplifies the process of state recovery

### Examples

<CodeBlock lang="python">

```python
class Memento:
    def __init__(self, state):
        self._state = state

    def get_state(self):
        return self._state

class Originator:
    def __init__(self):
        self._state = None

    def set_state(self, state):
        self._state = state

    def save_state(self):
        return Memento(self._state)

    def restore_state(self, memento):
        self._state = memento.get_state()

class Caretaker:
    def __init__(self):
        self._mementos = []

    def add_memento(self, memento):
        self._mementos.append(memento)

    def get_memento(self, index):
        return self._mementos[index]

originator = Originator()
caretaker = Caretaker()

originator.set_state("State1")
caretaker.add_memento(originator.save_state())

originator.set_state("State2")
caretaker.add_memento(originator.save_state())

originator.restore_state(caretaker.get_memento(0))
print(originator._state)  # Output: State1
```

</CodeBlock>

### Real-world Applications
The Memento pattern is commonly used in text editors and other applications that require undo and redo functionality.

## Mediator Pattern

### Introduction
The Mediator pattern reduces the complexity of communication between multiple objects or classes. This pattern provides a mediator object that handles communication between different objects, promoting loose coupling.

### Definition
The Mediator pattern is a behavioral design pattern that defines an object that encapsulates how a set of objects interact, promoting loose coupling by preventing objects from referring to each other explicitly.

### Use Cases
- When you have multiple objects interacting with each other in complex ways
- To centralize complex communication logic

### Benefits
- Promotes loose coupling between objects
- Simplifies object communication
- Centralizes control logic

### Examples

<CodeBlock lang="python">

```python
class Mediator:
    def notify(self, sender, event):
        pass

class ConcreteMediator(Mediator):
    def __init__(self, component1, component2):
        self._component1 = component1
        self._component2 = component2
        self._component1.mediator = self
        self._component2.mediator = self

    def notify(self, sender, event):
        if event == "A":
            print("Mediator reacts on A and triggers following operations:")
            self._component2.do_c()
        elif event == "D":
            print("Mediator reacts on D and triggers following operations:")
            self._component1.do_b()

class BaseComponent:
    def __init__(self, mediator=None):
        self._mediator = mediator

    @property
    def mediator(self):
        return self._mediator

    @mediator.setter
    def mediator(self, mediator):
        self._mediator = mediator

class Component1(BaseComponent):
    def do_a(self):
        print("Component 1 does A.")
        self.mediator.notify(self, "A")

    def do_b(self):
        print("Component 1 does B.")

class Component2(BaseComponent):
    def do_c(self):
        print("Component 2 does C.")

    def do_d(self):
        print("Component 2 does D.")
        self.mediator.notify(self, "D")

component1 = Component1()
component2 = Component2()
mediator = ConcreteMediator(component1, component2)

component1.do_a()
component2.do_d()
```

</CodeBlock>

### Real-world Applications
The Mediator pattern is used in GUI frameworks to handle interactions between UI components, such as dialogs that coordinate actions between buttons, text fields, and other widgets.

## Iterator Pattern

### Introduction
The Iterator pattern provides a way to access elements of a collection sequentially without exposing its underlying representation. This pattern is useful for traversing collections of objects.

### Definition
The Iterator pattern is a behavioral design pattern that allows you to traverse elements of a collection without exposing the underlying representation.

### Use Cases
- To traverse different collections using a uniform interface
- To abstract the traversal logic from the collection's implementation

### Benefits
- Simplifies traversal of complex data structures
- Promotes code reusability and separation of concerns

### Examples

<CodeBlock lang="python">

```python
class Iterator(ABC):
    @abstractmethod
    def __next__(self):
        pass

class ConcreteIterator(Iterator):
    def __init__(self, collection):
        self._collection = collection
        self._index = 0

    def __next__(self):
        try:
            value = self._collection[self._index]
        except IndexError:
            raise StopIteration()
        self._index += 1
        return value

class Aggregate(ABC):
    @abstractmethod
    def __iter__(self):
        pass

class ConcreteAggregate(Aggregate):
    def __init__(self, collection):
        self._collection = collection

    def __iter__(self):
        return ConcreteIterator(self._collection)

aggregate = ConcreteAggregate([1, 2, 3, 4])
for item in aggregate:
    print(item)
```

</CodeBlock>

### Real-world Applications
The Iterator pattern is widely used in various programming languages and frameworks to provide a standardized way to traverse collections, such as lists, arrays, and trees.

## Interpreter Pattern

### Introduction
The Interpreter pattern provides a way to evaluate language grammar or expressions. This pattern is useful for interpreting or translating languages, particularly when building compilers or interpreters.

### Definition
The Interpreter pattern is a behavioral design pattern that defines a representation of a language's grammar and uses that representation to interpret sentences in the language.

### Use Cases
- When you need to interpret or evaluate expressions
- To build a language interpreter or compiler

### Benefits
- Simplifies the implementation of a language interpreter
- Promotes reuse of expression parsing logic

### Examples

<CodeBlock lang="python">

```python
class Context:
    def __init__(self, input):
        self.input = input
        self.output = 0

class AbstractExpression(ABC):
    @abstractmethod
    def interpret(self, context):
        pass

class TerminalExpression(AbstractExpression):
    def interpret(self, context):
        context.output += int(context.input)

class NonTerminalExpression(AbstractExpression):
    def interpret(self, context):
        context.output *= 2

context = Context("3")
tree = [TerminalExpression(), NonTerminalExpression()]
for expr in tree:
    expr.interpret(context)
print(context.output)  # Output: 6
```

</CodeBlock>

### Real-world Applications
The Interpreter pattern is used in language interpreters, regular expression engines, and other systems that need to evaluate or translate textual expressions.

## Command Pattern

### Introduction
The Command pattern encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations. This pattern is useful for implementing undo/redo functionality.

### Definition
The Command pattern is a behavioral design pattern that turns a request into a stand-alone object containing all information about the request, allowing for parameterization and queuing of requests.

### Use Cases
- When you need to parameterize objects with operations
- To implement undo/redo functionality

### Benefits
- Decouples sender and receiver
- Simplifies adding new commands
- Supports undo/redo operations

### Examples

<CodeBlock lang="python">

```python
class Command(ABC):
    @abstractmethod
    def execute(self):
        pass

class SimpleCommand(Command):
    def __init__(self, payload):
        self._payload = payload

    def execute(self):
        print(f"SimpleCommand: See, I can do simple things like printing ({self._payload})")

class Receiver:
    def do_something(self, a):
        print(f"Receiver: Working on ({a}.)")

    def do_something_else(self, b):
        print(f"Receiver: Also working on ({b}.)")

class ComplexCommand(Command):
    def __init__(self, receiver, a, b):
        self._receiver = receiver
        self._a = a
        self._b = b

    def execute(self):
        print("ComplexCommand: Complex stuff should be done by a receiver object.")
        self._receiver.do_something(self._a)
        self._receiver.do_something_else(self._b)

class Invoker:
    def set_on_start(self, command):
        self._on_start = command

    def set_on_finish(self, command):
        self._on_finish = command

    def do_something_important(self):
        print("Invoker: Does anybody want something done before I begin?")
        if isinstance(self._on_start, Command):
            self._on_start.execute()

        print("Invoker: ...doing something really important...")

        print("Invoker: Does anybody want something done after I finish?")
        if isinstance(self._on_finish, Command):
            self._on_finish.execute()

invoker = Invoker()
invoker.set_on_start(SimpleCommand("Say Hi!"))
receiver = Receiver()
invoker.set_on_finish(ComplexCommand(receiver, "Send email", "Save report"))

invoker.do_something_important()
```

</CodeBlock>

### Real-world Applications
The Command

 pattern is widely used in GUI applications to handle actions triggered by user interface elements, such as buttons and menu items.

## Chain of Responsibility Pattern

### Introduction
The Chain of Responsibility pattern allows passing requests along a chain of handlers. This pattern is useful for decoupling sender and receiver by giving multiple objects a chance to handle the request.

### Definition
The Chain of Responsibility pattern is a behavioral design pattern that lets you pass requests along a chain of handlers, where each handler decides either to process the request or to pass it to the next handler in the chain.

### Use Cases
- When multiple objects can handle a request, but the specific handler is not known beforehand
- To decouple sender and receiver

### Benefits
- Promotes loose coupling between sender and receiver
- Enhances flexibility in assigning responsibilities
- Simplifies adding or removing handlers

### Examples

<CodeBlock lang="python">

```python
class Handler(ABC):
    def __init__(self, successor=None):
        self._successor = successor

    def handle(self, request):
        if self._successor:
            self._successor.handle(request)

class ConcreteHandler1(Handler):
    def handle(self, request):
        if request == "R1":
            print("ConcreteHandler1 handled request R1")
        else:
            super().handle(request)

class ConcreteHandler2(Handler):
    def handle(self, request):
        if request == "R2":
            print("ConcreteHandler2 handled request R2")
        else:
            super().handle(request)

class ConcreteHandler3(Handler):
    def handle(self, request):
        if request == "R3":
            print("ConcreteHandler3 handled request R3")
        else:
            super().handle(request)

handler_chain = ConcreteHandler1(ConcreteHandler2(ConcreteHandler3()))
handler_chain.handle("R2")
handler_chain.handle("R3")
handler_chain.handle("R1")
```

</CodeBlock>

### Real-world Applications
The Chain of Responsibility pattern is used in logging frameworks where different log handlers process messages based on their severity level, such as debug, info, warning, and error.

## Conclusion
Behavioral design patterns are crucial in software development as they help manage complex behaviors between objects, improve code flexibility, and promote separation of concerns. By understanding and applying these patterns, developers can create robust and maintainable software systems.
