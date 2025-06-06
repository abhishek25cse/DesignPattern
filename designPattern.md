
**Software Design Patterns: An Indian Developer’s Perspective**

As a software engineer in India, I often rely on design patterns to solve recurring architecture and coding challenges. Design patterns provide proven templates for creating flexible and maintainable code. In this guide, I explain major **Creational**, **Structural**, and **Behavioral** design patterns from my perspective, and I tie each pattern to Indian tech examples—from UPI payments to Ola cabs and Aadhaar. I’ll describe *how I use* each pattern, when it’s best applied, and give a real-world Indian analogy or example.

**Creational Patterns**

**Singleton Pattern**

When I use the **Singleton** pattern, I make sure that only one instance of a class exists across my entire application. In practice, this means I control object creation so that every time I ask for that class, I get the same instance. As Refactoring Guru describes, Singleton “ensures that a class has only one instance, while providing a global access point to this instance”. In my own code, I implement a private constructor and a static getInstance() method so that multiple parts of my app share the same object.

I find Singleton most useful when I need a single shared resource or configuration. For example, I maintain one global logger, one database connection pool, or one HTTP client. In a food delivery app, I might use a Singleton for the HTTP client so that all network requests reuse the same connection and save resources. As one developer noted, “Singleton ensures a single instance of a class, like Aadhaar in India—one unique ID per person, referenced everywhere”. Indeed, the Indian Aadhaar system is a great analogy: every citizen has exactly one Aadhaar number, and all government services refer to that one record. In the same way, a Singleton object is a *single* source of truth that the whole app can access.

- **When to use:** I use Singleton for shared resources or services that must have exactly one instance, such as a configuration manager, a cache, or a logging system.
- **Indian Example:** Think of the Aadhaar identity system — one unique ID per citizen. In my code, a Singleton could manage user session data or global config so there aren’t conflicting copies. For instance, Paytm-like payment app might have one SessionManager singleton that tracks the current user across the app.

**Factory Method Pattern**

When I use the **Factory Method** pattern, I delegate object creation to subclasses via a “factory” interface. Instead of calling new directly, I call a factory method that returns objects. As Refactoring Guru explains, the Factory Method “provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created”. In my code, I might have an abstract creator class with a create() method. Each subclass decides what concrete product to instantiate. This lets me add new product types without changing client code.

I reach for Factory Method when I need flexibility in what kind of object is created. For example, suppose I’m building a travel app like MakeMyTrip or IRCTC that handles different ticket types. I might define a BookingFactory interface and have TrainBookingFactory, BusBookingFactory, and FlightBookingFactory subclasses that create TrainTicket, BusTicket, or FlightTicket objects, respectively. The client code just calls bookingFactory.createBooking() and doesn’t care which specific class is returned.

- **When to use:** I use Factory Method when my code needs to instantiate objects but I want to defer the exact class decision to subclasses or to runtime. It’s ideal when adding new types should not modify existing code.
- **Indian Example:** In an Ola-like ride-hailing service, I could use a factory method to create different vehicle objects. For instance, RideFactory.getRide("car") returns a CarRide object, while getRide("bike") returns a BikeRide object. As one example points out, Ola uses such a factory: APIRequestFactory.getRequest("car") to create ride requests for cars, bikes, or autos. This way, the app can support new ride types without touching the main booking logic.

**Abstract Factory Pattern**

When I use the **Abstract Factory** pattern, I create families of related objects without specifying their concrete classes. Refactoring Guru describes Abstract Factory as letting you “produce families of related objects without specifying their concrete classes”. In my code, I define an abstract factory interface with methods to create each product in the family. Then I implement concrete factories for each variant. Clients use the abstract interface and get matching products from a chosen factory.

A typical use-case is UI theming or multi-language support. For example, imagine I’m building a government portal that needs both English and Hindi interfaces. I could define abstract products like Label, Button, and Form. The **HindiFactory** produces HindiLabel, HindiButton, etc., while the **EnglishFactory** produces EnglishLabel, EnglishButton. The rest of my code just uses the abstract Label and Button interfaces, unaware of the language variant. This way, swapping to Hindi or English involves just choosing the appropriate factory.

- **When to use:** I apply Abstract Factory when I want to create multiple related objects together and ensure they belong to the same family (e.g., same theme or style). It helps avoid code changes when adding new families of products.
- **Indian Example:** Consider a banking app that supports both English and Hindi. An abstract factory can generate UI components for each language. For instance, the **HindiUIFactory** might create Hindi-label buttons and Hindi error messages, while an **EnglishUIFactory** creates English components. This matches Refactoring Guru’s description of Abstract Factory: producing related objects (UI components) without hard-coding their classes.

**Builder Pattern**

When I use the **Builder** pattern, I construct a complex object step by step. This is useful when an object has many parts or optional parameters. Refactoring Guru succinctly states that Builder “lets you construct complex objects step by step”. In practice, I separate the construction of the object from its representation. The builder exposes fluent methods (like setName(), setAmount(), etc.) and a final build() call. This avoids giant constructors with many parameters.

Builder shines when dealing with complicated request or object creation. For example, UPI payment requests have many fields: payer ID, payee ID, amount, currency, transaction note, etc. In PhonePe (a UPI app), the Builder pattern is used to assemble the payment request. The code snippet below shows how PhonePe might do it:

PaymentRequest paymentRequest = PaymentRequest.builder()

.setPayer("user123")

.setPayee("merchant456")

.setAmount(1000)

.setCurrency("INR")

.build();

This was highlighted by a Medium example: “in PhonePe, creating API requests for initiating UPI payments involves multiple parameters... The builder pattern allows constructing such objects step-by-step, making the code readable and maintainable”. By calling only the relevant set methods, I avoid having to pass every single parameter if not needed.

- **When to use:** I use Builder when I have an object with many optional or conditional fields. It keeps my client code clean and avoids telescoping constructors.
- **Indian Example:** Building a UPI payment request in PhonePe or Paytm is a perfect scenario. As noted, PhonePe’s payment request (with payer, payee, amount) is built step-by-step using a Builder. In my code, I might implement a UPIPaymentBuilder so that creating a UPIPayment object is clear and easy.

**Prototype Pattern**

When I use the **Prototype** pattern, I create new objects by cloning an existing instance (the prototype) instead of constructing them from scratch. According to Refactoring Guru, Prototype “lets you copy existing objects without making your code dependent on their classes”. This means that any object that implements a clone method can produce a duplicate of itself. The client code works with an interface (e.g. a clone() method) and is decoupled from concrete classes.

Prototype is handy when object creation is expensive and many similar instances are needed. For example, suppose I have a travel-booking template already filled out. If a user wants to repeat a past booking (say, the same train ticket as last week), I could use Prototype to clone the previous ticket object and then adjust any minor details, rather than building a new ticket object from scratch. This is akin to having a *template* order in Flipkart or Zomato that I clone whenever the user reorders a meal or product. In such a case, the original pre-configured object serves as a prototype.

- **When to use:** I use Prototype when creating a new object by copying an existing one is easier than new construction. It’s great for copying complex objects with many fields, keeping code decoupled from concrete classes.
- **Indian Example:** Imagine an e-commerce app (like Amazon India) where a user can repeat an old order. The system could clone the previous Order object and only adjust the order date or shipping info. Here, the original order acts as a prototype for a new order, avoiding manual re-entry of each field.

**Structural Patterns**

**Adapter Pattern**

When I use the **Adapter** pattern, I make two incompatible interfaces work together. Adapter acts like a translator between our code and an external library or service. As one source explains, Adapter “allows objects with incompatible interfaces to collaborate”. In code, I write an Adapter class that implements the target interface my code expects, and inside it I call the methods of the incompatible class or API.

A simple analogy: in real life, a travel adapter lets my device plug into foreign electrical outlets. In software, I might use Adapter if I need to integrate, say, an external payment API into my UPI workflow. Suppose an international payment gateway provides JSON data, but my app expects XML. I could write an UPIAdapter that converts JSON to the internal format so the rest of my app can work seamlessly.

- **When to use:** I use Adapter when I have an existing class or API that doesn’t match the interface my code needs, and I cannot (or don’t want to) change that external interface. Adapter lets me wrap it and present a compatible interface to the rest of my app.
- **Indian Example:** Consider a scenario where my Indian e-commerce app integrates different wallets. I could write a PaytmAdapter or PhonePeAdapter to unify their APIs. Or, if I have a legacy NetBanking API and I want it to look like UPI, an adapter class would bridge the formats, much like an actual charger adapter converts socket types.

**Bridge Pattern**

When I use the **Bridge** pattern, I separate an abstraction from its implementation so that the two can vary independently. According to Refactoring Guru, Bridge “lets you split a large class or a set of closely related classes into two separate hierarchies — abstraction and implementation — which can be developed independently”. In practice, I define an abstraction interface (or abstract class) and an implementer interface. Then concrete classes implement each side.

Bridge is handy when I have multiple dimensions of variation. For instance, suppose I’m working on a ride-hailing app with different services (Ola Share, Ola Auto, etc.) and different providers of those rides. I could have an abstraction Ride that defines high-level operations, and separate implementations like OlaService and UberService. The Bridge pattern allows me to mix-and-match (e.g., SharedAutoRide, SharedCarRide, etc.) without explosive class hierarchies. Each side (types of rides vs providers) can change independently.

- **When to use:** I use Bridge when both the abstractions and implementations can have multiple variants, and I want to avoid a combinatorial explosion of subclasses. It decouples two orthogonal aspects of a class.
- **Indian Example:** Think of a payment system that supports different payment methods (UPI, credit card, wallet) *and* different banks. With Bridge, I could have a Payment abstraction and a Gateway implementation interface. Concrete classes like UPIPayment or CardPayment combine with HDFCGateway or ICICIGateway. This lets my code handle new payment types or banks without mixing them up.

**Composite Pattern**

When I use the **Composite** pattern, I treat groups of objects and individual objects uniformly by organizing them in a tree structure. Refactoring Guru explains that Composite “lets you compose objects into tree structures and then work with these structures as if they were individual objects”. In code, I define a common interface (say, MenuComponent) that both leaf objects and container objects implement. Then a container (composite) holds a list of MenuComponent children. The client can call methods (e.g. display()) on the composite and it will propagate to all children.

Composite fits when I have hierarchical data. For example, in India, many apps have nested categories. In Zomato’s restaurant menu, there are categories (e.g., “Starters”, “Mains”) that contain individual MenuItems. By applying Composite, both a MenuCategory (composite) and a MenuItem (leaf) implement the same interface. My code can then iterate and render the menu tree without caring about the differences. Likewise, DigiLocker organizes documents into folders and sub-folders: each folder (composite) and file (leaf) can be handled through the same interface.

- **When to use:** I use Composite for hierarchical or tree-like data where clients should treat individual and composite objects uniformly.
- **Indian Example:** A clear example is a file system like *DigiLocker*. Folders can contain documents or other folders, and I can call an operation like printContents() on any folder or document. Or think of Flipkart’s product categories: each category can contain subcategories or products, and each category/subcategory is itself a CatalogComponent. Composite makes it easy to display or process this tree.

**Decorator Pattern**

When I use the **Decorator** pattern, I add responsibilities to objects dynamically by wrapping them in another object. Refactoring Guru says Decorator “lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors”. In practice, I define a base component interface and concrete components. Decorator classes also implement the interface and hold an instance of the component, adding behavior before or after delegating calls.

Decorator is useful for adding features without subclassing. For instance, suppose I have a basic HTTP request object in my app. I can decorate it with features like authentication headers or compression. A Medium example illustrates this well: “In Ola Cabs, dynamic behaviours like adding headers for authentication or compressing the response can be achieved using decorators”. In code, I might write:

APIRequest request = new BaseRequest();

request = new AuthenticationDecorator(request);

request = new CompressionDecorator(request);

request.send();

Here AuthenticationDecorator and CompressionDecorator both wrap the original request and add functionality.

- **When to use:** I use Decorator when I want to add or modify behavior of individual objects without affecting others, especially when subclassing would lead to many classes. Decorator provides flexible layering of behavior.
- **Indian Example:** In an Ola-like app, decorating an API request is a real use case. As noted, Ola might wrap a request with an AuthDecorator and then a LoggingDecorator. Similarly, in a Paytm transaction flow, I could use decorators to add features like encryption or logging around the basic payment request object without changing its core code.

**Facade Pattern**

When I use the **Facade** pattern, I provide a simple interface to a complex subsystem. Refactoring Guru defines Facade as a way to “provide a simplified interface to a library, a framework, or any other complex set of classes”. In implementation, I create a Facade class that wraps the subsystem’s functionality, exposing only what the client really needs. The complex interactions remain hidden inside the facade.

Facade is great when integrating a large system. For example, IRCTC (the Indian Railway reservation site) itself acts like a facade: it hides the complexity of seat allocation, quotas, and payment from the end user. In my code, I might write a TrainBookingFacade that internally calls multiple services (seat availability, ticketing, payment) but exposes a single method bookTicket() to the app. This way, other parts of the code don’t have to manage each subsystem step by step.

- **When to use:** I use Facade when I want to simplify and decouple my application from a complex subsystem. It’s handy for wrapping elaborate APIs so clients deal with just one interface.
- **Indian Example:** A common example is a payment gateway aggregator. Suppose we have separate services for UPI, NetBanking, and wallets. I could write a PaymentFacade with a single processPayment() method. Internally, it calls the right gateway APIs. This facade approach is like how calling IRCTC’s simple “book” endpoint hides a lot of backend complexity.

**Proxy Pattern**

When I use the **Proxy** pattern, I create a placeholder or surrogate for another object to control access to it. Refactoring Guru describes Proxy as providing “a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object”. In code, a proxy class implements the same interface as the real subject and holds a reference to it. The proxy might add access checks, lazy initialization, caching, or logging.

Proxy is useful when you need extra processing on method calls. For example, if I integrate with a remote Aadhaar authentication service, I might use a proxy to cache recent responses or rate-limit calls. Each time my app requests verification, it goes through the proxy which first checks its cache. Only if needed does it forward the request to the real Aadhaar API. Similarly, in a government system, an API gateway often acts as a proxy: it sits in front of microservices to handle logging, auth, or caching before passing requests on.

- **When to use:** I use Proxy when I need to add a layer of control over access to an object, such as lazy loading, caching, logging, or access control.
- **Indian Example:** Consider DigiLocker (digital document store). If there’s a microservice that fetches documents, I could use a proxy to store recently accessed documents in a cache. So subsequent requests are faster. In essence, the proxy stands in front of the real document service. Similarly, an Aadhaar verification proxy could enforce security checks before forwarding calls to the UIDAI servers.

**Behavioral Patterns**

**Observer Pattern**

When I use the **Observer** pattern, I let one object (the subject or publisher) notify many others (observers or subscribers) about changes in state. Refactoring Guru defines Observer as letting you “define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing”. In practice, the subject keeps a list of observers. When its state changes, it loops over this list and calls an update or notify method on each observer.

Observer is perfect for event-driven situations. For example, in a stock trading app (often built in India), if a stock price updates, all user interfaces displaying that stock should refresh. Each UI component can subscribe to the stock object. When the stock’s price changes, it notifies all subscribers. A real-world analogy is subscribing to a government alert: if citizens (observers) subscribe to daily news alerts, whenever a new alert (state change) is issued, everyone in the subscription list is notified.

- **When to use:** I use Observer when changes in one object need to automatically propagate to many others, and I want to keep them loosely coupled.
- **Indian Example:** In mobile apps like UPI wallets or IRCTC, think of push notifications. For instance, when an IRCTC train status changes (delayed, arrived, etc.), the app notifies all users who subscribed to that train. Under the hood, this works like Observer: the train status object notifies all subscribed user interfaces of the update.

**Strategy Pattern**

When I use the **Strategy** pattern, I define a family of algorithms, encapsulate each one, and make them interchangeable at runtime. As Refactoring Guru puts it, Strategy “lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable”. I typically create a Strategy interface and several concrete strategy classes that implement it. The client then holds a reference to a strategy and can swap it out as needed.

Strategy is very useful when I have multiple ways to perform a task and want to choose one dynamically. For example, in ride-sharing apps like Ola or Uber, fare can be calculated by different algorithms (normal pricing vs. surge pricing). I can implement each fare calculation as a strategy class (NormalFareStrategy, SurgeFareStrategy). Depending on current conditions, my app sets the appropriate strategy, and the ride booking code just calls calculateFare(). The client code doesn’t need to change to accommodate new pricing methods.

- **When to use:** I use Strategy when I want to select an algorithm’s behavior at runtime. Each strategy is a separate class, and I can switch them without modifying the client.
- **Indian Example:** A concrete example is in Ola’s fare engine. Suppose Ola applies one algorithm for standard rides and another for shared rides. By encapsulating each fare-calculation algorithm in its own strategy class, the app can choose based on ride type. Another example: Zomato might calculate delivery fee differently for premium subscribers versus regular users; each can be a strategy.

**Command Pattern**

When I use the **Command** pattern, I turn requests or actions into objects. Refactoring Guru explains that Command “turns a request into a stand-alone object that contains all information about the request. This lets you pass requests as method arguments, delay or queue a request’s execution, and support undoable operations”. In implementation, I create a Command interface with an execute() method. Each concrete command holds the details needed to perform an action and a reference to the receiver (the actual business object).

Command is ideal for queuing or scheduling actions. For instance, in a chat or social media app built in India, sending a message can be represented as a command object. The client creates a SendMessageCommand with the message text, recipient, etc., and enqueues it. A background worker takes the command and calls execute(), sending the message. This decouples the UI from the sending logic. Another use is implementing “undo”: e.g., editing a bank transfer form, each change could be a command so the user can undo/redo it.

- **When to use:** I use Command when I need to parameterize actions, queue them, or implement undo functionality.
- **Indian Example:** In UPI apps, when a user initiates a fund transfer, I could encapsulate that operation in a TransferMoneyCommand. This command object holds payer, payee, amount, etc., and is passed to a payment processor. This makes it easy to log, retry on failure, or even cancel (undo) if needed. Each user action in a web frontend (like in IRCTC booking) can similarly be a Command object.

**Iterator Pattern**

When I use the **Iterator** pattern, I traverse elements of a collection without exposing its internal structure. The pattern “lets you traverse elements of a collection without exposing its underlying representation”. In code, I usually give my collection class an iterator() method that returns an Iterator object. The Iterator has hasNext() and next() methods to loop through elements. This way, I can loop over lists, trees, or any collection uniformly.

Iterator is used any time I need to scan through a group of objects. In an Indian context, think of IRCTC search results: I might implement an iterator to go through the list of train options. The calling code does something like:

Iterator<Train> it = trainList.iterator();

while (it.hasNext()) {

`    `Train train = it.next();

`    `// display train details

}

This code doesn’t need to know whether trainList is an array, a database result set, or something else. The iterator abstracts that away.

- **When to use:** I use Iterator whenever I need to access elements of a collection sequentially and want to hide the container’s implementation details.
- **Indian Example:** A practical case is traversing a user’s transaction history. Suppose my Paytm-like app fetches a list of recent transactions. Using Iterator, I can loop through each Transaction object for display or analysis. Whether the transactions come from an in-memory list, a file, or a database, the iterator interface stays the same.

**Mediator Pattern**

When I use the **Mediator** pattern, I reduce direct dependencies between components by having them communicate through a mediator object. Refactoring Guru notes that Mediator “lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object”. In code, I create a mediator class that holds references to all components. Each component talks only to the mediator, which decides how to dispatch messages or coordinate actions.

Mediator is useful in complex systems where many components interact. A great Indian example is the UPI architecture. GeeksforGeeks explains that UPI has “a central UPI switch managed by NPCI” which handles all transactions between apps and banks. This switch essentially acts as a mediator: it receives payment requests from one bank or app and coordinates with the receiving bank. In a similar way, in my app I could write a ChatMediator for a group chat: users (colleagues in an Ola driver group, say) send messages to the mediator, which then broadcasts to all participants. Each user only knows the mediator, not each other.

- **When to use:** I use Mediator when I want to decouple components and centralize their communication, especially to avoid tight coupling in a many-to-many relationship.
- **Indian Example:** The NPCI UPI system is essentially a Mediator between banks and apps. In my code, I might implement a PaymentMediator that receives a transfer request from the Paytm module, then calls the appropriate bank module. This ensures Paytm doesn’t call every bank directly, but only the mediator does the coordination.

**Memento Pattern**

When I use the **Memento** pattern, I save and restore an object’s state without breaking encapsulation. Refactoring Guru describes Memento as letting you “save and restore the previous state of an object without revealing the details of its implementation”. Typically, the object needing save/restore (the originator) creates a memento object containing its state. A caretaker (perhaps a history manager) stores these mementos. Later, the originator can ask a memento to revert to an earlier state.

Memento is helpful for undo functionality. For example, consider a user filling out an Aadhaar enrollment form or income-tax filing on an Indian government portal. The user might want to "save draft" and come back later. Internally, my code can use Memento to capture the entire form’s state. Each time the user updates info, I could push a memento onto a stack. If the user clicks undo or reloads the draft, I pop the memento and restore the form. The key is that the caretaker (e.g., the draft manager) holds mementos without understanding the form’s private fields.

- **When to use:** I use Memento when I need to implement undo/redo or save/restore without exposing the object’s inner workings.
- **Indian Example:** A real application is in online forms. For instance, IRCTC’s multi-page booking form could allow “save and resume later.” Using the Memento pattern, the app can snapshot all current entries (passenger details, preferences) into a memento. When the user returns, the app restores the form state from the memento, all while keeping the form’s internals private.

**State Pattern**

When I use the **State** pattern, I let an object change its behavior when its internal state changes, as if its class changed. Refactoring Guru explains that State “lets an object alter its behavior when its internal state changes. It appears as if the object changed its class”. In code, I encapsulate each state in a class that implements a common interface. The context object holds a state instance and delegates calls to it. Changing the context’s state is done by swapping its state object.

State is ideal for workflow or lifecycle logic. For example, in an Indian banking app, a money transfer transaction goes through states: *Initiated*, *OTP Pending*, *Completed*, *Failed*. I can implement each as a State class. When the user enters OTP, the context switches from OtpPendingState to CompletedState, and subsequent calls (like execute()) do something different. This avoids big conditionals checking a status value.

- **When to use:** I use State when an object’s behavior should change with its state, and I want to avoid large switch/case or if/else chains.
- **Indian Example:** Consider an IRCTC ticket booking. The booking request might be in states like *Searching Trains*, *Collecting Passengers*, *Payment Pending*, *Booked*. Each of these can be a state object. As the user progresses, the booking context switches states and its responses (like available actions) change. This makes the code cleaner than peppering if (state==PAYMENT\_PENDING) all over.

**Template Method Pattern**

When I use the **Template Method** pattern, I define the skeleton of an algorithm in a base class, but allow subclasses to override specific steps. As Refactoring Guru states, Template Method “defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure”. The base class has a final method that calls abstract or hook methods for each step. Subclasses then implement those steps.

Template Method is useful for invariant workflows with customizable parts. For example, IRCTC ticket booking has a fixed sequence: select train, fill passenger details, choose payment, confirm. However, the exact payment gateway or user verification might vary (for different user categories). I could write an abstract BookingProcess class with a bookTicket() template method that calls selectTrain(), enterDetails(), makePayment(). Concrete subclasses override makePayment() (say, GuestBookingProcess vs RegisteredUserBookingProcess) to handle differences.

- **When to use:** I use Template Method to implement the common structure of an algorithm once, and let subclasses provide the details of each step.
- **Indian Example:** A good analogy is automating a government form filing. All filings follow the same general steps (user authentication, form fill, validation, submission). I can put those steps in a template method. Then for each specific department’s form, I override only the step that fills form fields or validates data. The high-level workflow stays in one place.

**Chain of Responsibility Pattern**

When I use the **Chain of Responsibility** (CoR) pattern, I let a request pass through a chain of handlers until one of them handles it. Refactoring Guru says CoR “lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain”. In my code, each handler object holds a reference to the next handler. The request travels down the chain until someone handles it or it reaches the end.

I use CoR when multiple processing steps could handle a request, and I want the flexibility to dynamically assign responsibility. For example, consider an online payment system in India with fraud checks. A payment request might first go to an authentication handler, then to a fraud-detection handler, then to a bank authorization handler. Each of these is a handler in the chain. If the first handler (authentication) succeeds, it passes the request to the next. If a handler finds a problem (like fraud), it can halt the chain and reject the payment.

- **When to use:** I use Chain of Responsibility for sequential processing of requests where different conditions might handle or block the request.
- **Indian Example:** A concrete scenario is IRCTC’s booking security. A booking request could go through a chain: **Verify CAPTCHA → Check Login Status → Check Agent Quota → Process Payment**. Each handler checks one thing and either stops the chain (on failure) or passes it on. This mimics how CoR separates each step, as described in examples.

Each of these patterns helps me write code that is clearer, more modular, and more adaptable. By relating them to real Indian use-cases—like Aadhaar, UPI, Ola, Zomato, and government services—I ensure the patterns are not just abstract concepts but practical tools I apply every day in development.

**Sources:** Definitions and intent for patterns are drawn from design pattern literature, and Indian examples are inspired by real systems (Aadhaar, NPCI UPI, Ola, IRCTC, etc.) and community discussions.

