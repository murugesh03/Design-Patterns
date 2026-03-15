# 🏗️ Design Patterns — Complete Reference Guide
### Beginner to Super Advanced | Detailed Explanations with Code
> *Every pattern explained with real-world analogies, when to use, when NOT to use, code examples, and pitfalls*

---

## Table of Contents

**PART 1 — FOUNDATIONS**
1. [What Are Design Patterns?](#1-what-are-design-patterns)
2. [SOLID Principles](#2-solid-principles)
3. [DRY, KISS & YAGNI](#3-dry-kiss--yagni)

**PART 2 — CREATIONAL PATTERNS**
*(How objects are created)*

4. [Singleton](#4-singleton)
5. [Factory Method](#5-factory-method)
6. [Abstract Factory](#6-abstract-factory)
7. [Builder](#7-builder)
8. [Prototype](#8-prototype)
9. [Object Pool](#9-object-pool)

**PART 3 — STRUCTURAL PATTERNS**
*(How objects are composed)*

10. [Adapter](#10-adapter)
11. [Bridge](#11-bridge)
12. [Composite](#12-composite)
13. [Decorator](#13-decorator)
14. [Facade](#14-facade)
15. [Flyweight](#15-flyweight)
16. [Proxy](#16-proxy)

**PART 4 — BEHAVIORAL PATTERNS**
*(How objects communicate)*

17. [Chain of Responsibility](#17-chain-of-responsibility)
18. [Command](#18-command)
19. [Iterator](#19-iterator)
20. [Mediator](#20-mediator)
21. [Memento](#21-memento)
22. [Observer](#22-observer)
23. [State](#23-state)
24. [Strategy](#24-strategy)
25. [Template Method](#25-template-method)
26. [Visitor](#26-visitor)
27. [Interpreter](#27-interpreter)

**PART 5 — ARCHITECTURAL PATTERNS**
*(How systems are structured)*

28. [MVC — Model View Controller](#28-mvc--model-view-controller)
29. [MVP — Model View Presenter](#29-mvp--model-view-presenter)
30. [MVVM — Model View ViewModel](#30-mvvm--model-view-viewmodel)
31. [Repository Pattern](#31-repository-pattern)
32. [Service Layer Pattern](#32-service-layer-pattern)
33. [Event Sourcing](#33-event-sourcing)
34. [CQRS — Command Query Responsibility Segregation](#34-cqrs--command-query-responsibility-segregation)

**PART 6 — ADVANCED & MODERN PATTERNS**

35. [Dependency Injection](#35-dependency-injection)
36. [Decorator vs HOC (React Pattern)](#36-decorator-vs-hoc-react-pattern)
37. [Module Pattern](#37-module-pattern)
38. [Mixin Pattern](#38-mixin-pattern)
39. [Null Object Pattern](#39-null-object-pattern)
40. [Specification Pattern](#40-specification-pattern)
41. [Pipeline Pattern](#41-pipeline-pattern)
42. [Saga Pattern](#42-saga-pattern)
43. [Circuit Breaker Pattern](#43-circuit-breaker-pattern)
44. [Anti-Patterns to Avoid](#44-anti-patterns-to-avoid)
45. [Pattern Selection Cheat Sheet](#45-pattern-selection-cheat-sheet)

---

# PART 1 — FOUNDATIONS

---

## 1. What Are Design Patterns?

Design patterns are **reusable solutions to commonly occurring problems** in software design. They are not code you copy-paste — they are **templates** that describe how to solve a problem in a general way.

### The Origin

In 1994, four authors (the "Gang of Four" — GoF) published **"Design Patterns: Elements of Reusable Object-Oriented Software"** describing 23 classic patterns. This book changed how developers think about code structure.

### Why Learn Them?

```
Without patterns:            With patterns:
─────────────────────        ─────────────────────
"I wrote some code           "I used the Observer
that watches for             pattern to decouple
changes and updates          the UI from the data
multiple components"         layer"

✗ Hard to communicate        ✓ Shared vocabulary
✗ Possibly brittle           ✓ Proven solution
✗ Reinventing the wheel      ✓ Faster development
✗ Hard to maintain           ✓ Easier to maintain
```

### The Three Categories

```
┌─────────────────────────────────────────────────────────────┐
│                    DESIGN PATTERNS                          │
├──────────────────┬──────────────────┬───────────────────────┤
│   CREATIONAL     │   STRUCTURAL     │     BEHAVIORAL        │
│                  │                  │                       │
│ How objects are  │ How objects are  │ How objects           │
│ created          │ composed         │ communicate           │
│                  │                  │                       │
│ • Singleton      │ • Adapter        │ • Observer            │
│ • Factory        │ • Bridge         │ • Strategy            │
│ • Abstract Fctry │ • Composite      │ • Command             │
│ • Builder        │ • Decorator      │ • Iterator            │
│ • Prototype      │ • Facade         │ • State               │
│ • Object Pool    │ • Flyweight      │ • Chain of Resp.      │
│                  │ • Proxy          │ • Template Method     │
│                  │                  │ • Visitor             │
│                  │                  │ • Mediator            │
│                  │                  │ • Memento             │
└──────────────────┴──────────────────┴───────────────────────┘
```

### Pattern Structure

Every pattern in this guide follows this structure:

```
🎯 Intent       — What problem does it solve?
🌍 Real World   — Analogy from the real world
✅ Use When     — Situations where it helps
❌ Avoid When   — When it's overkill or harmful
💡 Key Insight  — The core idea in one sentence
⚠️  Pitfalls    — Common mistakes
```

---

## 2. SOLID Principles

SOLID is the foundation of good OOP design. Design patterns are implementations of SOLID principles.

### S — Single Responsibility Principle (SRP)

> **"A class should have only one reason to change."**

```javascript
// ❌ VIOLATES SRP — This class does TOO many things
class UserManager {
  constructor(user) {
    this.user = user;
  }

  // Responsibility 1: Business logic
  validateEmail(email) {
    return email.includes('@');
  }

  // Responsibility 2: Database
  saveToDatabase(user) {
    // SQL query here...
    console.log('Saving to DB:', user);
  }

  // Responsibility 3: Sending emails
  sendWelcomeEmail(user) {
    // SMTP logic here...
    console.log('Sending email to:', user.email);
  }

  // Responsibility 4: Logging
  log(message) {
    console.log(`[LOG] ${new Date().toISOString()}: ${message}`);
  }
}
// Problem: If email sending changes, or DB changes, or validation changes
// → this class changes. Too many reasons to change!

// ✅ FOLLOWS SRP — Each class has ONE job
class UserValidator {
  validateEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }

  validateAge(age) {
    return age >= 0 && age <= 150;
  }
}

class UserRepository {
  save(user) {
    console.log('Saving to DB:', user);
    // Only DB logic here
  }

  findById(id) {
    // Only DB logic here
  }
}

class EmailService {
  sendWelcome(user) {
    console.log('Sending welcome email to:', user.email);
    // Only email logic here
  }
}

class Logger {
  log(message) {
    console.log(`[LOG] ${new Date().toISOString()}: ${message}`);
  }
}

// Now if email provider changes → only EmailService changes
// If DB changes → only UserRepository changes ✓
```

### O — Open/Closed Principle (OCP)

> **"Software entities should be open for extension, but closed for modification."**

```javascript
// ❌ VIOLATES OCP — Adding a new shape requires modifying existing code
class AreaCalculator {
  calculateArea(shape) {
    if (shape.type === 'circle') {
      return Math.PI * shape.radius ** 2;
    } else if (shape.type === 'rectangle') {
      return shape.width * shape.height;
    } else if (shape.type === 'triangle') {
      return 0.5 * shape.base * shape.height;
    }
    // Every new shape = modify this class! ❌
  }
}

// ✅ FOLLOWS OCP — Add new shapes by EXTENDING, not modifying
class Shape {
  area() {
    throw new Error('area() must be implemented');
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }
  area() {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }
  area() {
    return this.width * this.height;
  }
}

// Adding a new shape? Just create a new class! Never touch existing code.
class Triangle extends Shape {
  constructor(base, height) {
    super();
    this.base = base;
    this.height = height;
  }
  area() {
    return 0.5 * this.base * this.height;
  }
}

class AreaCalculator {
  calculateTotal(shapes) {
    return shapes.reduce((total, shape) => total + shape.area(), 0);
  }
}
// Adding Hexagon? Create Hexagon class. Done. AreaCalculator never changes. ✓
```

### L — Liskov Substitution Principle (LSP)

> **"Subtypes must be substitutable for their base types."**

```javascript
// ❌ VIOLATES LSP — Square breaks Rectangle's contract
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  setWidth(w)  { this.width = w; }
  setHeight(h) { this.height = h; }
  area()       { return this.width * this.height; }
}

class Square extends Rectangle {
  setWidth(w) {
    this.width = w;
    this.height = w;  // ← PROBLEM: breaks Rectangle's contract!
  }
  setHeight(h) {
    this.width = h;   // ← PROBLEM
    this.height = h;
  }
}

function testRectangle(rect) {
  rect.setWidth(5);
  rect.setHeight(4);
  console.log(rect.area()); // Expected: 20
}

testRectangle(new Rectangle(0, 0)); // 20 ✓
testRectangle(new Square(0));       // 16 ✗ — Square breaks the function!

// ✅ FOLLOWS LSP — Separate hierarchy
class Shape {
  area() { throw new Error('Implement area()'); }
}

class Rectangle extends Shape {
  constructor(w, h) { super(); this.w = w; this.h = h; }
  area() { return this.w * this.h; }
}

class Square extends Shape {
  constructor(side) { super(); this.side = side; }
  area() { return this.side ** 2; }
}
// Both work independently — no broken substitution ✓
```

### I — Interface Segregation Principle (ISP)

> **"Clients should not be forced to depend on interfaces they don't use."**

```javascript
// ❌ VIOLATES ISP — Fat interface forces useless implementations
class Animal {
  eat()   { throw new Error('implement'); }
  fly()   { throw new Error('implement'); }  // Not all animals fly!
  swim()  { throw new Error('implement'); }  // Not all animals swim!
  run()   { throw new Error('implement'); }  // Not all animals run!
}

class Dog extends Animal {
  eat()  { console.log('Dog eats'); }
  fly()  { throw new Error('Dogs cannot fly!'); }  // Forced to implement ❌
  swim() { console.log('Dog swims'); }
  run()  { console.log('Dog runs'); }
}

// ✅ FOLLOWS ISP — Small, focused interfaces
class Animal {
  eat() { throw new Error('implement'); }
}

const Flyable = (Base) => class extends Base {
  fly() { throw new Error('implement'); }
};

const Swimmable = (Base) => class extends Base {
  swim() { throw new Error('implement'); }
};

const Runnable = (Base) => class extends Base {
  run() { throw new Error('implement'); }
};

// Dog only gets what it needs
class Dog extends Runnable(Swimmable(Animal)) {
  eat()  { console.log('Dog eats'); }
  swim() { console.log('Dog swims'); }
  run()  { console.log('Dog runs'); }
}

// Eagle gets different abilities
class Eagle extends Flyable(Animal) {
  eat() { console.log('Eagle eats'); }
  fly() { console.log('Eagle soars'); }
}
```

### D — Dependency Inversion Principle (DIP)

> **"Depend on abstractions, not concretions."**

```javascript
// ❌ VIOLATES DIP — High-level module depends on low-level module directly
class MySQLDatabase {
  save(data) {
    console.log('Saving to MySQL:', data);
  }
}

class UserService {
  constructor() {
    this.db = new MySQLDatabase(); // ← Direct dependency on concrete class!
  }
  saveUser(user) {
    this.db.save(user); // Hard to test, hard to swap database
  }
}

// ✅ FOLLOWS DIP — Both depend on an abstraction
// The "abstraction" (interface/contract)
class Database {
  save(data)   { throw new Error('implement'); }
  find(id)     { throw new Error('implement'); }
  delete(id)   { throw new Error('implement'); }
}

// Low-level modules implement the abstraction
class MySQLDatabase extends Database {
  save(data)   { console.log('Saving to MySQL:', data); }
  find(id)     { console.log('Finding in MySQL:', id); }
  delete(id)   { console.log('Deleting from MySQL:', id); }
}

class MongoDatabase extends Database {
  save(data)   { console.log('Saving to MongoDB:', data); }
  find(id)     { console.log('Finding in MongoDB:', id); }
  delete(id)   { console.log('Deleting from MongoDB:', id); }
}

class InMemoryDatabase extends Database {
  constructor() { super(); this.store = new Map(); }
  save(data)   { this.store.set(data.id, data); }
  find(id)     { return this.store.get(id); }
  delete(id)   { this.store.delete(id); }
}

// High-level module depends on abstraction
class UserService {
  constructor(database) {    // ← Inject the dependency
    this.db = database;      // Works with ANY database!
  }
  saveUser(user) {
    this.db.save(user);
  }
}

// Production: use MySQL
const prodService = new UserService(new MySQLDatabase());
// Testing: use in-memory (fast, no real DB needed)
const testService = new UserService(new InMemoryDatabase());
// Easy to swap: just change the injected dependency ✓
```

---

## 3. DRY, KISS & YAGNI

```javascript
// ── DRY: Don't Repeat Yourself ────────────────────
// ❌ Repetition
function getUserFullName(user) {
  return user.firstName + ' ' + user.lastName;
}
function getAdminFullName(admin) {
  return admin.firstName + ' ' + admin.lastName; // Same logic!
}

// ✅ DRY
function getFullName(person) {
  return `${person.firstName} ${person.lastName}`;
}

// ── KISS: Keep It Simple, Stupid ──────────────────
// ❌ Over-engineered
function isEvenUltraOptimized(n) {
  return ((n >> 1) << 1) === n; // Clever bit trick...but WHY?
}

// ✅ KISS — Simple and clear
function isEven(n) {
  return n % 2 === 0; // Everyone understands this immediately
}

// ── YAGNI: You Aren't Gonna Need It ───────────────
// ❌ Building features "just in case"
class UserManager {
  constructor() {
    this.users = [];
    this.cache = new Map();          // "Might need caching later"
    this.eventBus = new EventBus();  // "Might need events later"
    this.exportFormats = [];         // "Might need export later"
    this.pluginSystem = {};          // "Might need plugins later"
  }
}

// ✅ YAGNI — Build what you need NOW
class UserManager {
  constructor() {
    this.users = []; // That's all we need right now
  }
  addUser(user) { this.users.push(user); }
  findUser(id)  { return this.users.find(u => u.id === id); }
}
// Add more when you ACTUALLY need it
```

---

# PART 2 — CREATIONAL PATTERNS

---

## 4. Singleton

### 🎯 Intent
Ensure a class has **only one instance** and provide a global access point to it.

### 🌍 Real World Analogy
A country has **one president** at a time. No matter who asks "who is the president?", they all get the same person.

### 💡 Key Insight
The class itself controls its own instantiation — constructor is private, a static method returns the single instance.

```javascript
// ─── Basic Singleton ──────────────────────────────
class Singleton {
  static #instance = null;  // Private static field

  #data = {};               // Private instance data

  // Private constructor — no one can call new Singleton()
  constructor() {
    if (Singleton.#instance) {
      throw new Error('Use Singleton.getInstance()');
    }
  }

  static getInstance() {
    if (!Singleton.#instance) {
      Singleton.#instance = new Singleton();
    }
    return Singleton.#instance;
  }

  set(key, value) { this.#data[key] = value; }
  get(key)        { return this.#data[key]; }
}

const s1 = Singleton.getInstance();
const s2 = Singleton.getInstance();

console.log(s1 === s2);       // true  — same instance!
s1.set('theme', 'dark');
console.log(s2.get('theme')); // 'dark' — same data!

// ─── Real-World: Configuration Manager ──────────
class ConfigManager {
  static #instance = null;

  #config = {
    apiUrl: 'https://api.example.com',
    timeout: 5000,
    maxRetries: 3,
    environment: 'production',
  };

  static getInstance() {
    if (!ConfigManager.#instance) {
      ConfigManager.#instance = new ConfigManager();
    }
    return ConfigManager.#instance;
  }

  get(key) {
    return this.#config[key];
  }

  set(key, value) {
    // Could add validation here
    this.#config[key] = value;
    console.log(`Config updated: ${key} = ${value}`);
  }

  getAll() {
    return { ...this.#config }; // Return a copy, not the reference
  }
}

// Anywhere in the app:
const config = ConfigManager.getInstance();
console.log(config.get('apiUrl')); // https://api.example.com

// ─── Real-World: Logger ───────────────────────────
class Logger {
  static #instance = null;
  #logs = [];
  #level = 'INFO';

  static getInstance() {
    if (!Logger.#instance) {
      Logger.#instance = new Logger();
    }
    return Logger.#instance;
  }

  setLevel(level) { this.#level = level; }

  info(message) {
    this.#log('INFO', message);
  }

  warn(message) {
    this.#log('WARN', message);
  }

  error(message) {
    this.#log('ERROR', message);
  }

  #log(level, message) {
    const entry = {
      level,
      message,
      timestamp: new Date().toISOString(),
    };
    this.#logs.push(entry);
    console.log(`[${entry.timestamp}] [${level}] ${message}`);
  }

  getLogs() { return [...this.#logs]; }

  clearLogs() { this.#logs = []; }
}

// Same logger instance everywhere:
Logger.getInstance().info('Application started');
Logger.getInstance().error('Something went wrong');
Logger.getInstance().getLogs(); // All logs from all callers

// ─── Module Pattern Singleton (JavaScript specific) ─
// In JS modules, the module itself is a singleton!
// database.js
let connection = null;

function getConnection() {
  if (!connection) {
    connection = createDatabaseConnection();
  }
  return connection;
}

function createDatabaseConnection() {
  return { status: 'connected', query: (sql) => console.log('Running:', sql) };
}

export { getConnection }; // Every import gets the same connection
```

### ✅ Use When
- Configuration objects shared across the app
- Logger instances
- Database connection pools
- Caches
- Thread pools

### ❌ Avoid When
- When you need to test in isolation (Singletons make mocking hard)
- When the "single" might need to become "multiple" later
- When it introduces hidden global state (hard to track dependencies)

### ⚠️ Pitfalls
```javascript
// ⚠️ Pitfall 1: Not thread-safe in multi-threaded environments
// (JavaScript is single-threaded, so this is only relevant for Node.js workers)

// ⚠️ Pitfall 2: Hard to test — use dependency injection instead
class ServiceThatUsesSingleton {
  doWork() {
    Logger.getInstance().info('Working...'); // Hard to mock Logger in tests!
  }
}

// Better: inject the logger
class ServiceWithInjection {
  constructor(logger) {
    this.logger = logger; // Easy to pass mock in tests
  }
  doWork() {
    this.logger.info('Working...');
  }
}
```

---

## 5. Factory Method

### 🎯 Intent
Define an interface for creating objects, but let **subclasses decide which class to instantiate**. The factory method defers instantiation to subclasses.

### 🌍 Real World Analogy
A **pizza store** (creator) defines how to make pizza, but each franchise (subclass) creates their own style. NYC franchise creates NY-style pizza; Chicago franchise creates deep-dish.

### 💡 Key Insight
Instead of `new ConcreteClass()`, call `createSomething()` — the subclass decides what "something" is.

```javascript
// ─── Problem Without Factory Method ──────────────
// If you do this everywhere:
const button = new WindowsButton();
// Then changing to Mac requires changing ALL these lines!

// ─── Solution: Factory Method ────────────────────

// Product interface
class Button {
  render() { throw new Error('render() must be implemented'); }
  onClick(handler) { throw new Error('onClick() must be implemented'); }
}

// Concrete Products
class WindowsButton extends Button {
  render() {
    console.log('Rendering a Windows-style button [■]');
    return '<button class="windows-btn">Click me</button>';
  }
  onClick(handler) {
    console.log('Windows button: registering click handler');
    // Windows-specific event handling
  }
}

class MacButton extends Button {
  render() {
    console.log('Rendering a Mac-style button (●)');
    return '<button class="mac-btn">Click me</button>';
  }
  onClick(handler) {
    console.log('Mac button: registering click handler');
    // Mac-specific event handling
  }
}

class LinuxButton extends Button {
  render() {
    console.log('Rendering a Linux-style button');
    return '<button class="linux-btn">Click me</button>';
  }
  onClick(handler) { /* Linux-specific */ }
}

// Creator (abstract)
class Dialog {
  // The Factory Method — subclasses override this
  createButton() {
    throw new Error('createButton() must be implemented');
  }

  // Template method that uses the factory method
  render() {
    const okButton = this.createButton();       // calls factory method
    const cancelButton = this.createButton();   // same factory

    okButton.onClick(() => this.submit());
    cancelButton.onClick(() => this.cancel());

    return `
      <dialog>
        <h2>Alert</h2>
        ${okButton.render()}
        ${cancelButton.render()}
      </dialog>
    `;
  }

  submit() { console.log('Submitted!'); }
  cancel() { console.log('Cancelled!'); }
}

// Concrete Creators — override the factory method
class WindowsDialog extends Dialog {
  createButton() {
    return new WindowsButton();  // Always creates Windows buttons
  }
}

class MacDialog extends Dialog {
  createButton() {
    return new MacButton();      // Always creates Mac buttons
  }
}

// Client code — doesn't care WHICH dialog it is
function renderUI(dialog) {
  console.log(dialog.render());  // Works with any dialog!
}

// Determine at runtime
const os = process.platform;
let dialog;
if (os === 'win32')  dialog = new WindowsDialog();
else if (os === 'darwin') dialog = new MacDialog();
else dialog = new LinuxDialog();

renderUI(dialog); // Client code never changes!

// ─── Real-World: Notification Factory ────────────
class Notification {
  send(message) { throw new Error('Implement send()'); }
}

class EmailNotification extends Notification {
  constructor(email) {
    super();
    this.email = email;
  }
  send(message) {
    console.log(`📧 Sending email to ${this.email}: ${message}`);
  }
}

class SMSNotification extends Notification {
  constructor(phone) {
    super();
    this.phone = phone;
  }
  send(message) {
    console.log(`📱 Sending SMS to ${this.phone}: ${message}`);
  }
}

class PushNotification extends Notification {
  constructor(deviceToken) {
    super();
    this.deviceToken = deviceToken;
  }
  send(message) {
    console.log(`🔔 Push to device ${this.deviceToken}: ${message}`);
  }
}

class SlackNotification extends Notification {
  constructor(channel) {
    super();
    this.channel = channel;
  }
  send(message) {
    console.log(`💬 Slack to #${this.channel}: ${message}`);
  }
}

// Factory Method
class NotificationService {
  // Factory method
  createNotification(type, target) {
    switch (type) {
      case 'email': return new EmailNotification(target);
      case 'sms':   return new SMSNotification(target);
      case 'push':  return new PushNotification(target);
      case 'slack': return new SlackNotification(target);
      default: throw new Error(`Unknown notification type: ${type}`);
    }
  }

  notify(type, target, message) {
    const notification = this.createNotification(type, target);
    notification.send(message);
  }
}

const service = new NotificationService();
service.notify('email', 'user@example.com', 'Your order shipped!');
service.notify('sms',   '+1234567890',       'Your order shipped!');
service.notify('slack', 'alerts',            'New order received!');
```

### ✅ Use When
- You don't know the exact type of object to create until runtime
- You want subclasses to control what objects get created
- You want to provide users of your library a way to extend its internal components

### ❌ Avoid When
- The type of object is always known and fixed
- The extra class hierarchy adds complexity without benefit

---

## 6. Abstract Factory

### 🎯 Intent
Create **families of related objects** without specifying their concrete classes. It's a "factory of factories."

### 🌍 Real World Analogy
An **IKEA furniture store** (abstract factory) has consistent product families: the "BILLY" family has a matching bookshelf, desk, and chair. You pick a family and get all matching pieces.

### 💡 Key Insight
Instead of one factory method, you have a factory **interface** with multiple methods — each for a related product in the family.

```javascript
// ─── Abstract Factory Example: UI Component Library ─

// Abstract Products — interfaces for each product type
class Button {
  render() { throw new Error('implement'); }
  getStyle() { throw new Error('implement'); }
}

class TextField extends Button {
  validate() { throw new Error('implement'); }
}

class Checkbox {
  isChecked() { throw new Error('implement'); }
  render() { throw new Error('implement'); }
}

// ── Concrete Products: Material Design Family ─────
class MaterialButton extends Button {
  render() {
    return '<button class="md-btn md-ripple">Click</button>';
  }
  getStyle() { return 'material'; }
}

class MaterialTextField {
  constructor(label) { this.label = label; }
  validate() { return this.label.length > 0; }
  render() { return `<div class="md-text-field"><label>${this.label}</label><input/></div>`; }
}

class MaterialCheckbox {
  constructor() { this.checked = false; }
  isChecked() { return this.checked; }
  render() { return '<label class="md-checkbox"><input type="checkbox"/></label>'; }
}

// ── Concrete Products: Fluent UI Family ──────────
class FluentButton extends Button {
  render() { return '<button class="fluent-btn">Click</button>'; }
  getStyle() { return 'fluent'; }
}

class FluentTextField {
  constructor(label) { this.label = label; }
  validate() { return this.label.length > 0; }
  render() { return `<div class="fluent-field"><span>${this.label}</span><input/></div>`; }
}

class FluentCheckbox {
  constructor() { this.checked = false; }
  isChecked() { return this.checked; }
  render() { return '<div class="fluent-checkbox"><input type="checkbox"/></div>'; }
}

// ── Concrete Products: Ant Design Family ─────────
class AntButton extends Button {
  render() { return '<button class="ant-btn ant-btn-primary">Click</button>'; }
  getStyle() { return 'antd'; }
}

class AntTextField {
  constructor(label) { this.label = label; }
  validate() { return this.label.length > 0; }
  render() { return `<div class="ant-form-item"><label>${this.label}</label><input class="ant-input"/></div>`; }
}

class AntCheckbox {
  constructor() { this.checked = false; }
  isChecked() { return this.checked; }
  render() { return '<label class="ant-checkbox-wrapper"><input class="ant-checkbox"/></label>'; }
}

// ── Abstract Factory ──────────────────────────────
class UIFactory {
  createButton()    { throw new Error('implement createButton()'); }
  createTextField() { throw new Error('implement createTextField()'); }
  createCheckbox()  { throw new Error('implement createCheckbox()'); }
}

// ── Concrete Factories ────────────────────────────
class MaterialFactory extends UIFactory {
  createButton()           { return new MaterialButton(); }
  createTextField(label)   { return new MaterialTextField(label); }
  createCheckbox()         { return new MaterialCheckbox(); }
}

class FluentFactory extends UIFactory {
  createButton()           { return new FluentButton(); }
  createTextField(label)   { return new FluentTextField(label); }
  createCheckbox()         { return new FluentCheckbox(); }
}

class AntDesignFactory extends UIFactory {
  createButton()           { return new AntButton(); }
  createTextField(label)   { return new AntTextField(label); }
  createCheckbox()         { return new AntCheckbox(); }
}

// ── Client Code — Works with ANY factory! ─────────
class LoginForm {
  constructor(factory) {
    this.factory = factory;
    this.usernameField = factory.createTextField('Username');
    this.passwordField = factory.createTextField('Password');
    this.rememberMe    = factory.createCheckbox();
    this.submitBtn     = factory.createButton();
  }

  render() {
    return `
      <form>
        ${this.usernameField.render()}
        ${this.passwordField.render()}
        ${this.rememberMe.render()}
        ${this.submitBtn.render()}
      </form>
    `;
  }
}

// Configure factory based on user preference / platform
function getUIFactory(theme) {
  const factories = {
    material: new MaterialFactory(),
    fluent:   new FluentFactory(),
    antd:     new AntDesignFactory(),
  };
  return factories[theme] || new MaterialFactory();
}

const factory  = getUIFactory('material');
const loginForm = new LoginForm(factory);
console.log(loginForm.render()); // All Material Design components!

// Switching themes is just changing the factory:
const darkFactory = getUIFactory('fluent');
const darkForm = new LoginForm(darkFactory); // All Fluent UI components!
```

### ✅ Use When
- Your code needs to work with various families of related products
- You want to enforce that products from one family are used together
- You have platform-specific implementations (Windows/Mac/Linux, Material/Fluent/Bootstrap)

---

## 7. Builder

### 🎯 Intent
Construct complex objects **step by step**. The same construction process can create different representations.

### 🌍 Real World Analogy
When you order a **custom burger**, the cashier takes your order step by step: "Bun type? Patty? Toppings? Sauce?" Each choice is optional or customizable. The result is assembled at the end.

### 💡 Key Insight
Separate the construction of a complex object from its representation. Use method chaining for a fluent API.

```javascript
// ─── Problem Without Builder ──────────────────────
// Constructor with too many parameters (telescoping constructor anti-pattern)
class User {
  constructor(firstName, lastName, email, age, phone, address, city,
              country, zipCode, role, isAdmin, isVerified, createdAt) {
    // 13 parameters! Which order? What's optional?
  }
}
new User('John', 'Doe', 'john@example.com', 30, null, null, null,
         null, null, 'user', false, true, new Date()); // Terrible!

// ─── Solution: Builder Pattern ───────────────────
class User {
  constructor(builder) {
    // Required fields
    this.firstName = builder.firstName;
    this.lastName  = builder.lastName;
    this.email     = builder.email;

    // Optional fields with defaults
    this.age       = builder.age       || null;
    this.phone     = builder.phone     || null;
    this.address   = builder.address   || null;
    this.city      = builder.city      || null;
    this.country   = builder.country   || 'US';
    this.role      = builder.role      || 'user';
    this.isAdmin   = builder.isAdmin   || false;
    this.isVerified = builder.isVerified || false;
    this.createdAt = builder.createdAt || new Date();
  }

  toString() {
    return `User(${this.firstName} ${this.lastName}, ${this.email}, role: ${this.role})`;
  }
}

class UserBuilder {
  constructor(firstName, lastName, email) {
    // Required fields in constructor
    this.firstName = firstName;
    this.lastName  = lastName;
    this.email     = email;
  }

  // Each setter returns 'this' for method chaining
  setAge(age) {
    if (age < 0 || age > 150) throw new Error('Invalid age');
    this.age = age;
    return this;  // ← enables chaining!
  }

  setPhone(phone) {
    this.phone = phone;
    return this;
  }

  setAddress(address, city, country, zipCode) {
    this.address = address;
    this.city    = city;
    this.country = country;
    this.zipCode = zipCode;
    return this;
  }

  setRole(role) {
    const validRoles = ['user', 'admin', 'moderator', 'viewer'];
    if (!validRoles.includes(role)) throw new Error(`Invalid role: ${role}`);
    this.role = role;
    return this;
  }

  makeAdmin() {
    this.isAdmin = true;
    this.role    = 'admin';
    return this;
  }

  verify() {
    this.isVerified = true;
    return this;
  }

  build() {
    // Validate required fields before building
    if (!this.email.includes('@')) throw new Error('Invalid email');
    return new User(this);
  }
}

// Beautiful, readable object creation:
const user = new UserBuilder('John', 'Doe', 'john@example.com')
  .setAge(30)
  .setPhone('+1-555-0123')
  .setAddress('123 Main St', 'Springfield', 'US', '12345')
  .setRole('moderator')
  .verify()
  .build();

console.log(user.toString()); // User(John Doe, john@example.com, role: moderator)

// Minimal user:
const minimalUser = new UserBuilder('Jane', 'Smith', 'jane@example.com')
  .build();

// ─── Real-World: SQL Query Builder ────────────────
class QueryBuilder {
  #table  = '';
  #conditions = [];
  #columns = ['*'];
  #orderBy = null;
  #limit   = null;
  #offset  = null;
  #joins   = [];

  from(table) {
    this.#table = table;
    return this;
  }

  select(...columns) {
    this.#columns = columns;
    return this;
  }

  where(condition) {
    this.#conditions.push(condition);
    return this;
  }

  join(table, condition) {
    this.#joins.push(`JOIN ${table} ON ${condition}`);
    return this;
  }

  leftJoin(table, condition) {
    this.#joins.push(`LEFT JOIN ${table} ON ${condition}`);
    return this;
  }

  orderBy(column, direction = 'ASC') {
    this.#orderBy = `${column} ${direction}`;
    return this;
  }

  limit(n) {
    this.#limit = n;
    return this;
  }

  offset(n) {
    this.#offset = n;
    return this;
  }

  build() {
    if (!this.#table) throw new Error('Table is required');

    let query = `SELECT ${this.#columns.join(', ')} FROM ${this.#table}`;

    if (this.#joins.length)      query += ` ${this.#joins.join(' ')}`;
    if (this.#conditions.length) query += ` WHERE ${this.#conditions.join(' AND ')}`;
    if (this.#orderBy)           query += ` ORDER BY ${this.#orderBy}`;
    if (this.#limit !== null)    query += ` LIMIT ${this.#limit}`;
    if (this.#offset !== null)   query += ` OFFSET ${this.#offset}`;

    return query;
  }
}

const query = new QueryBuilder()
  .from('users')
  .select('id', 'name', 'email', 'created_at')
  .join('orders', 'users.id = orders.user_id')
  .where('users.age > 18')
  .where('users.is_verified = true')
  .orderBy('created_at', 'DESC')
  .limit(20)
  .offset(40)
  .build();

console.log(query);
// SELECT id, name, email, created_at FROM users
// JOIN orders ON users.id = orders.user_id
// WHERE users.age > 18 AND users.is_verified = true
// ORDER BY created_at DESC LIMIT 20 OFFSET 40

// ─── HTTP Request Builder ─────────────────────────
class HttpRequestBuilder {
  constructor() {
    this.config = {
      method:  'GET',
      headers: {},
      timeout: 5000,
    };
  }

  get(url)    { this.config.method = 'GET';    this.config.url = url; return this; }
  post(url)   { this.config.method = 'POST';   this.config.url = url; return this; }
  put(url)    { this.config.method = 'PUT';    this.config.url = url; return this; }
  delete(url) { this.config.method = 'DELETE'; this.config.url = url; return this; }

  header(key, value) {
    this.config.headers[key] = value;
    return this;
  }

  auth(token) {
    return this.header('Authorization', `Bearer ${token}`);
  }

  contentType(type) {
    return this.header('Content-Type', type);
  }

  json(data) {
    this.config.body = JSON.stringify(data);
    return this.contentType('application/json');
  }

  timeout(ms) {
    this.config.timeout = ms;
    return this;
  }

  async send() {
    console.log('Sending request:', this.config);
    // return fetch(this.config.url, this.config);
  }
}

// Elegant API:
await new HttpRequestBuilder()
  .post('https://api.example.com/users')
  .auth('my-secret-token')
  .json({ name: 'Alice', email: 'alice@example.com' })
  .timeout(3000)
  .send();
```

### ✅ Use When
- Creating complex objects with many optional parameters
- You want to enforce a construction process step-by-step
- You need to create different representations of the same thing (e.g., JSON vs XML output)

---

## 8. Prototype

### 🎯 Intent
Create new objects by **cloning an existing object** (the prototype) rather than creating from scratch.

### 🌍 Real World Analogy
A **photocopier** — instead of hand-drawing a document each time, you copy an existing one. The copy is a new object but starts as a clone.

### 💡 Key Insight
Cloning is sometimes cheaper than constructing from scratch, especially if initialization is expensive.

```javascript
// ─── Basic Prototype ──────────────────────────────
class Shape {
  constructor(color, x, y) {
    this.color = color;
    this.x = x;
    this.y = y;
  }

  clone() {
    // Create a new instance of the same class with the same properties
    return Object.assign(Object.create(Object.getPrototypeOf(this)), this);
  }

  moveTo(x, y) {
    const cloned = this.clone();
    cloned.x = x;
    cloned.y = y;
    return cloned;
  }
}

class Circle extends Shape {
  constructor(color, x, y, radius) {
    super(color, x, y);
    this.radius = radius;
  }

  clone() {
    return new Circle(this.color, this.x, this.y, this.radius);
  }

  resize(factor) {
    const cloned = this.clone();
    cloned.radius *= factor;
    return cloned;  // Return new — original unchanged (immutable pattern)
  }

  area() { return Math.PI * this.radius ** 2; }
}

const originalCircle = new Circle('red', 10, 20, 50);
const copiedCircle   = originalCircle.clone();
const biggerCircle   = originalCircle.resize(2);
const movedCircle    = originalCircle.moveTo(100, 200);

console.log(originalCircle !== copiedCircle);   // true — different objects
console.log(originalCircle.radius);              // 50 — unchanged
console.log(biggerCircle.radius);                // 100 — new object

// ─── Prototype Registry ───────────────────────────
// A library of pre-configured prototypes to clone from
class PrototypeRegistry {
  #prototypes = new Map();

  register(name, prototype) {
    this.#prototypes.set(name, prototype);
    return this;
  }

  clone(name, overrides = {}) {
    const prototype = this.#prototypes.get(name);
    if (!prototype) throw new Error(`Prototype '${name}' not found`);
    return Object.assign(prototype.clone(), overrides);
  }
}

// Setup pre-configured shapes
const registry = new PrototypeRegistry();
registry
  .register('small-red-circle',  new Circle('red', 0, 0, 10))
  .register('large-blue-circle', new Circle('blue', 0, 0, 100))
  .register('medium-circle',     new Circle('green', 0, 0, 50));

// Clone and customize
const shape1 = registry.clone('small-red-circle', { x: 50, y: 50 });
const shape2 = registry.clone('small-red-circle', { color: 'yellow', x: 100 });

// ─── Deep Clone for Complex Objects ───────────────
class Config {
  constructor(data) {
    this.data = data;
  }

  clone() {
    // Deep clone — JSON trick (works for JSON-safe data)
    return new Config(JSON.parse(JSON.stringify(this.data)));
  }

  // Using structuredClone (modern JS)
  deepClone() {
    return new Config(structuredClone(this.data));
  }
}

const baseConfig = new Config({
  server: { host: 'localhost', port: 3000 },
  db: { host: 'localhost', port: 5432, name: 'mydb' },
  features: { darkMode: false, beta: false },
});

const testConfig = baseConfig.clone();
testConfig.data.db.name = 'test_db';
testConfig.data.features.beta = true;

console.log(baseConfig.data.db.name);  // 'mydb' — original unchanged
console.log(testConfig.data.db.name);  // 'test_db' — independent copy
```

---

## 9. Object Pool

### 🎯 Intent
Reuse a set of initialized objects from a "pool" rather than creating and destroying them on demand.

### 🌍 Real World Analogy
A **library** — instead of buying books each time you read, you borrow from a pool of books. When done, you return it for others to use.

### 💡 Key Insight
Creating/destroying objects is expensive. Reusing them from a pool is much faster.

```javascript
class PooledObject {
  constructor(id) {
    this.id = id;
    this.inUse = false;
    this.data = null;
  }

  acquire(data) {
    this.inUse = true;
    this.data  = data;
    return this;
  }

  release() {
    this.inUse = false;
    this.data  = null;  // Clean up before returning to pool
  }
}

class ObjectPool {
  #pool    = [];
  #active  = new Set();
  #factory;
  #maxSize;

  constructor(factory, initialSize = 5, maxSize = 20) {
    this.#factory = factory;
    this.#maxSize = maxSize;

    // Pre-create initial objects
    for (let i = 0; i < initialSize; i++) {
      this.#pool.push(factory(i));
    }
  }

  acquire(...args) {
    let obj = this.#pool.find(o => !o.inUse);

    if (!obj) {
      if (this.#pool.length < this.#maxSize) {
        // Create new if pool not at max
        obj = this.#factory(this.#pool.length);
        this.#pool.push(obj);
      } else {
        throw new Error('Object pool exhausted! Max size reached.');
      }
    }

    obj.acquire(...args);
    this.#active.add(obj);
    return obj;
  }

  release(obj) {
    obj.release();
    this.#active.delete(obj);
  }

  get available() { return this.#pool.filter(o => !o.inUse).length; }
  get activeCount() { return this.#active.size; }
  get totalSize()   { return this.#pool.length; }
}

// Database Connection Pool Example
class DatabaseConnection {
  constructor(id) {
    this.id = id;
    this.inUse = false;
    console.log(`Connection #${id} created`); // Expensive!
  }

  acquire(data) {
    this.inUse = true;
    return this;
  }

  release() {
    this.inUse = false;
    // Reset connection state
  }

  query(sql) {
    if (!this.inUse) throw new Error('Connection not acquired!');
    console.log(`[Conn #${this.id}] Executing: ${sql}`);
    return { rows: [], count: 0 };
  }
}

const dbPool = new ObjectPool(
  id => new DatabaseConnection(id),
  3,    // Start with 3 connections
  10    // Max 10 connections
);

console.log(`Available: ${dbPool.available}`); // 3

// Acquire connections
const conn1 = dbPool.acquire();
const conn2 = dbPool.acquire();
console.log(`Available: ${dbPool.available}`); // 1

conn1.query('SELECT * FROM users');
conn2.query('INSERT INTO orders VALUES (...)');

// Return to pool when done
dbPool.release(conn1);
dbPool.release(conn2);

console.log(`Available: ${dbPool.available}`); // 3 again — reused!
```

---

# PART 3 — STRUCTURAL PATTERNS

---

## 10. Adapter

### 🎯 Intent
Allow incompatible interfaces to work together by **wrapping** one class with an adapter that translates the interface.

### 🌍 Real World Analogy
A **power adapter** — your US laptop charger (3-pin, 110V) doesn't fit a European outlet (2-pin, 220V). The adapter converts between the two standards without changing either device.

### 💡 Key Insight
You can't change the old class (it's third-party or legacy). So you wrap it in an adapter that speaks the language both sides understand.

```javascript
// ─── The Problem: Incompatible interfaces ─────────
// Your app uses this interface:
class NewPaymentGateway {
  processPayment(amount, currency, cardDetails) { /* ... */ }
  refund(transactionId, amount) { /* ... */ }
  getStatus(transactionId) { /* ... */ }
}

// But you also have this LEGACY payment system:
class LegacyPaymentSystem {
  // Completely different interface!
  makeTransaction(value, curr, card_no, card_exp, card_cvv) {
    console.log(`Legacy: processing $${value} ${curr}`);
    return `LEGACY_TXN_${Date.now()}`;
  }

  reverseTransaction(txn_id, value) {
    console.log(`Legacy: reversing ${txn_id} for $${value}`);
    return true;
  }

  checkTransaction(txn_id) {
    return { status: 'completed', id: txn_id };
  }
}

// ─── Adapter: makes LegacyPaymentSystem look like NewPaymentGateway ─
class LegacyPaymentAdapter extends NewPaymentGateway {
  constructor(legacySystem) {
    super();
    this.legacy = legacySystem;
  }

  // Adapts the new interface to call the old methods
  processPayment(amount, currency, cardDetails) {
    const { cardNumber, expiry, cvv } = cardDetails;
    // Translate the call to the legacy format
    const txnId = this.legacy.makeTransaction(
      amount,
      currency,
      cardNumber,
      expiry,
      cvv
    );
    return { success: true, transactionId: txnId };
  }

  refund(transactionId, amount) {
    const success = this.legacy.reverseTransaction(transactionId, amount);
    return { success, transactionId };
  }

  getStatus(transactionId) {
    const result = this.legacy.checkTransaction(transactionId);
    // Translate legacy response format to new format
    return {
      status:        result.status,
      transactionId: result.id,
      timestamp:     new Date().toISOString(),
    };
  }
}

// ─── Client code works with ONE interface ─────────
class PaymentService {
  constructor(gateway) {
    this.gateway = gateway; // Works with ANY gateway that has the right interface
  }

  pay(order) {
    return this.gateway.processPayment(
      order.total,
      order.currency,
      order.card
    );
  }
}

const legacySystem = new LegacyPaymentSystem();
const adapter      = new LegacyPaymentAdapter(legacySystem);
const service      = new PaymentService(adapter);

service.pay({
  total: 99.99,
  currency: 'USD',
  card: { cardNumber: '4111111111111111', expiry: '12/26', cvv: '123' }
});

// ─── Real-World: API Response Adapter ─────────────
// Different APIs return data in different formats
class TwitterApiAdapter {
  constructor(twitterClient) {
    this.client = twitterClient;
  }

  // Normalizes Twitter's format to your standard format
  async getUser(username) {
    const twitterUser = await this.client.users.showByUsername({ username });
    // Adapt Twitter format → your standard format
    return {
      id:          twitterUser.data.id,
      name:        twitterUser.data.name,
      username:    twitterUser.data.username,
      bio:         twitterUser.data.description,
      followers:   twitterUser.data.public_metrics.followers_count,
      following:   twitterUser.data.public_metrics.following_count,
      avatarUrl:   twitterUser.data.profile_image_url,
      platform:    'twitter',
    };
  }
}

class GithubApiAdapter {
  constructor(githubClient) {
    this.client = githubClient;
  }

  async getUser(username) {
    const githubUser = await this.client.users.getByUsername({ username });
    // Adapt GitHub format → same standard format
    return {
      id:        String(githubUser.data.id),
      name:      githubUser.data.name,
      username:  githubUser.data.login,
      bio:       githubUser.data.bio,
      followers: githubUser.data.followers,
      following: githubUser.data.following,
      avatarUrl: githubUser.data.avatar_url,
      platform:  'github',
    };
  }
}

// Client works with any social platform uniformly
class SocialProfileService {
  constructor(adapters) {
    this.adapters = adapters;
  }

  async getProfile(platform, username) {
    const adapter = this.adapters[platform];
    if (!adapter) throw new Error(`Platform ${platform} not supported`);
    return adapter.getUser(username);
  }
}
```

---

## 11. Bridge

### 🎯 Intent
**Decouple an abstraction from its implementation** so both can vary independently. It's like having a bridge between two class hierarchies.

### 🌍 Real World Analogy
A **TV remote** (abstraction) and a **TV** (implementation). You can have different remotes (universal, smart, basic) with different TVs (Samsung, LG, Sony). Each combination works independently.

### 💡 Key Insight
Instead of having `CircleRed`, `CircleBlue`, `SquareRed`, `SquareBlue` (m×n classes), you have `Circle` + `Square` (m) + `Red` + `Blue` (n). Use composition instead of deep inheritance.

```javascript
// ─── Without Bridge: Class Explosion! ─────────────
// Shape + Color = m × n classes
// CircleRed, CircleBlue, CircleGreen
// SquareRed, SquareBlue, SquareGreen
// TriangleRed, TriangleBlue, TriangleGreen = 9 classes for 3+3!

// ─── With Bridge: m + n classes ───────────────────

// Implementation hierarchy (the "bridge" side)
class Renderer {
  renderCircle(x, y, radius) { throw new Error('implement'); }
  renderSquare(x, y, side)   { throw new Error('implement'); }
}

class SVGRenderer extends Renderer {
  renderCircle(x, y, radius) {
    return `<circle cx="${x}" cy="${y}" r="${radius}" fill="red"/>`;
  }
  renderSquare(x, y, side) {
    return `<rect x="${x}" y="${y}" width="${side}" height="${side}"/>`;
  }
}

class CanvasRenderer extends Renderer {
  constructor(ctx) { super(); this.ctx = ctx; }
  renderCircle(x, y, radius) {
    // this.ctx.beginPath(); this.ctx.arc(x, y, radius, 0, 2*Math.PI);
    return `[Canvas] Drawing circle at (${x},${y}) r=${radius}`;
  }
  renderSquare(x, y, side) {
    return `[Canvas] Drawing square at (${x},${y}) side=${side}`;
  }
}

class WebGLRenderer extends Renderer {
  renderCircle(x, y, radius) {
    return `[WebGL] Circle vertex shader at (${x},${y})`;
  }
  renderSquare(x, y, side) {
    return `[WebGL] Square vertex shader at (${x},${y})`;
  }
}

// Abstraction hierarchy (the "shape" side)
class Shape {
  constructor(renderer) {
    this.renderer = renderer;  // ← The bridge!
  }
  draw() { throw new Error('implement'); }
  resize(factor) { throw new Error('implement'); }
}

class Circle extends Shape {
  constructor(x, y, radius, renderer) {
    super(renderer);
    this.x = x; this.y = y; this.radius = radius;
  }

  draw() {
    return this.renderer.renderCircle(this.x, this.y, this.radius);
  }

  resize(factor) {
    return new Circle(this.x, this.y, this.radius * factor, this.renderer);
  }
}

class Square extends Shape {
  constructor(x, y, side, renderer) {
    super(renderer);
    this.x = x; this.y = y; this.side = side;
  }

  draw() {
    return this.renderer.renderSquare(this.x, this.y, this.side);
  }

  resize(factor) {
    return new Square(this.x, this.y, this.side * factor, this.renderer);
  }
}

// Mix and match!
const svgCircle    = new Circle(100, 100, 50, new SVGRenderer());
const canvasCircle = new Circle(100, 100, 50, new CanvasRenderer());
const svgSquare    = new Square(50, 50, 80,   new SVGRenderer());

console.log(svgCircle.draw());    // <circle cx="100" cy="100" r="50"/>
console.log(canvasCircle.draw()); // [Canvas] Drawing circle at (100,100) r=50
console.log(svgSquare.draw());    // <rect x="50" y="50" width="80" height="80"/>

// Switch renderer at runtime!
class DynamicShape extends Shape {
  setRenderer(renderer) {
    this.renderer = renderer;  // Hot-swap implementation!
    return this;
  }
}
```

---

## 12. Composite

### 🎯 Intent
Compose objects into **tree structures** to represent part-whole hierarchies. Lets clients treat individual objects and compositions uniformly.

### 🌍 Real World Analogy
A **file system** — a folder can contain files AND other folders. Whether you call `getSize()` on a file or a folder, it works. The folder sums up all children recursively.

### 💡 Key Insight
Both leaf nodes and composite nodes share the same interface. Client code never needs to know if it's dealing with a single item or a group.

```javascript
// ─── File System Example ──────────────────────────
class FileSystemItem {
  constructor(name) {
    this.name = name;
  }
  getSize()   { throw new Error('implement'); }
  print(indent = '') { throw new Error('implement'); }
}

// Leaf node — no children
class File extends FileSystemItem {
  constructor(name, size) {
    super(name);
    this.size = size;
  }

  getSize() { return this.size; }

  print(indent = '') {
    console.log(`${indent}📄 ${this.name} (${this.size}KB)`);
  }
}

// Composite node — has children
class Folder extends FileSystemItem {
  constructor(name) {
    super(name);
    this.children = [];
  }

  add(item) {
    this.children.push(item);
    return this;
  }

  remove(name) {
    this.children = this.children.filter(c => c.name !== name);
    return this;
  }

  getSize() {
    // Recursively sum all children's sizes
    return this.children.reduce((total, child) => total + child.getSize(), 0);
  }

  print(indent = '') {
    console.log(`${indent}📁 ${this.name}/ (${this.getSize()}KB)`);
    this.children.forEach(child => child.print(indent + '  '));
  }

  find(name) {
    for (const child of this.children) {
      if (child.name === name) return child;
      if (child instanceof Folder) {
        const found = child.find(name);
        if (found) return found;
      }
    }
    return null;
  }
}

// Build a tree
const root = new Folder('root');

const src = new Folder('src');
src.add(new File('app.js', 15))
   .add(new File('index.js', 8));

const components = new Folder('components');
components
  .add(new File('Button.jsx', 5))
  .add(new File('Modal.jsx', 12))
  .add(new File('Form.jsx', 20));

src.add(components);

const publicDir = new Folder('public');
publicDir
  .add(new File('index.html', 3))
  .add(new File('favicon.ico', 1));

root.add(src).add(publicDir);
root.add(new File('package.json', 2));
root.add(new File('README.md', 4));

// Client code treats files and folders IDENTICALLY:
root.print();
// 📁 root/ (70KB)
//   📁 src/ (60KB)
//     📄 app.js (15KB)
//     📄 index.js (8KB)
//     📁 components/ (37KB)
//       📄 Button.jsx (5KB)
//       📄 Modal.jsx (12KB)
//       📄 Form.jsx (20KB)
//   📁 public/ (4KB)
//     📄 index.html (3KB)
//     📄 favicon.ico (1KB)
//   📄 package.json (2KB)
//   📄 README.md (4KB)

console.log('Total size:', root.getSize(), 'KB'); // Works on any node!
console.log('Components size:', components.getSize(), 'KB'); // 37KB

// ─── UI Component Tree (React-like) ───────────────
class UIComponent {
  constructor(type, props = {}) {
    this.type     = type;
    this.props    = props;
    this.children = [];
  }

  add(child) {
    this.children.push(child);
    return this;
  }

  render(indent = '') {
    const propsStr = Object.entries(this.props)
      .map(([k, v]) => `${k}="${v}"`).join(' ');

    if (this.children.length === 0) {
      return `${indent}<${this.type} ${propsStr}/>`;
    }

    const childrenStr = this.children
      .map(c => c.render(indent + '  '))
      .join('\n');

    return `${indent}<${this.type} ${propsStr}>\n${childrenStr}\n${indent}</${this.type}>`;
  }
}

const ui = new UIComponent('div', { class: 'app' });
const header = new UIComponent('header', { class: 'header' });
header.add(new UIComponent('h1', { class: 'title' }));
header.add(new UIComponent('nav', { class: 'nav' }));

const main = new UIComponent('main', { class: 'content' });
main.add(new UIComponent('article', { class: 'post' }));
main.add(new UIComponent('aside', { class: 'sidebar' }));

ui.add(header).add(main);
console.log(ui.render());
```

---

## 13. Decorator

### 🎯 Intent
**Dynamically add responsibilities** to an object without modifying its class. Decorators wrap objects and add behavior.

### 🌍 Real World Analogy
**Coffee customization** — You start with a plain coffee. Then you "decorate" it: add milk (+$0.25), then add sugar (+$0.10), then add whipped cream (+$0.50). Each addition wraps the previous and adds its cost/behavior.

### 💡 Key Insight
Decorators implement the SAME interface as the object they wrap. They call the wrapped object's methods AND add their own behavior before/after.

```javascript
// ─── Coffee Example ───────────────────────────────
class Coffee {
  cost()        { return 2.00; }
  description() { return 'Plain coffee'; }
}

// Base Decorator
class CoffeeDecorator {
  constructor(coffee) {
    this.coffee = coffee; // Wrap the coffee
  }
  cost()        { return this.coffee.cost(); }       // delegate
  description() { return this.coffee.description(); } // delegate
}

// Concrete Decorators
class MilkDecorator extends CoffeeDecorator {
  cost()        { return this.coffee.cost() + 0.25; }
  description() { return this.coffee.description() + ', Milk'; }
}

class SugarDecorator extends CoffeeDecorator {
  cost()        { return this.coffee.cost() + 0.10; }
  description() { return this.coffee.description() + ', Sugar'; }
}

class WhipDecorator extends CoffeeDecorator {
  cost()        { return this.coffee.cost() + 0.50; }
  description() { return this.coffee.description() + ', Whipped Cream'; }
}

class VanillaDecorator extends CoffeeDecorator {
  cost()        { return this.coffee.cost() + 0.75; }
  description() { return this.coffee.description() + ', Vanilla Syrup'; }
}

// Wrap coffee in any order!
let myCoffee = new Coffee();
myCoffee = new MilkDecorator(myCoffee);
myCoffee = new SugarDecorator(myCoffee);
myCoffee = new WhipDecorator(myCoffee);

console.log(myCoffee.description()); // Plain coffee, Milk, Sugar, Whipped Cream
console.log(`$${myCoffee.cost()}`);  // $2.85

// ─── Real-World: Logger Decorator ────────────────
class UserService {
  async getUser(id) {
    console.log(`Fetching user ${id}`);
    return { id, name: 'Alice', email: 'alice@example.com' };
  }

  async saveUser(user) {
    console.log(`Saving user ${user.id}`);
    return true;
  }
}

// Logging decorator
class LoggingDecorator {
  constructor(service, logger = console) {
    this.service = service;
    this.logger  = logger;
  }

  async getUser(id) {
    this.logger.log(`[${new Date().toISOString()}] getUser(${id}) called`);
    try {
      const result = await this.service.getUser(id);
      this.logger.log(`[LOG] getUser(${id}) succeeded`);
      return result;
    } catch (error) {
      this.logger.error(`[LOG] getUser(${id}) failed: ${error.message}`);
      throw error;
    }
  }

  async saveUser(user) {
    this.logger.log(`[LOG] saveUser(${user.id}) called`);
    const result = await this.service.saveUser(user);
    this.logger.log(`[LOG] saveUser(${user.id}) completed`);
    return result;
  }
}

// Cache decorator
class CacheDecorator {
  constructor(service, ttl = 60000) {
    this.service = service;
    this.cache   = new Map();
    this.ttl     = ttl;
  }

  async getUser(id) {
    const cacheKey  = `user_${id}`;
    const cached    = this.cache.get(cacheKey);

    if (cached && Date.now() - cached.timestamp < this.ttl) {
      console.log(`[CACHE] Hit for user ${id}`);
      return cached.data;
    }

    const user = await this.service.getUser(id);
    this.cache.set(cacheKey, { data: user, timestamp: Date.now() });
    console.log(`[CACHE] Stored user ${id}`);
    return user;
  }

  async saveUser(user) {
    this.cache.delete(`user_${user.id}`); // Invalidate cache
    return this.service.saveUser(user);
  }
}

// Retry decorator
class RetryDecorator {
  constructor(service, maxRetries = 3, delay = 1000) {
    this.service    = service;
    this.maxRetries = maxRetries;
    this.delay      = delay;
  }

  async getUser(id) {
    for (let attempt = 1; attempt <= this.maxRetries; attempt++) {
      try {
        return await this.service.getUser(id);
      } catch (error) {
        if (attempt === this.maxRetries) throw error;
        console.log(`[RETRY] Attempt ${attempt} failed, retrying...`);
        await new Promise(r => setTimeout(r, this.delay * attempt));
      }
    }
  }

  async saveUser(user) {
    return this.service.saveUser(user); // No retry for mutations
  }
}

// Stack decorators:
let service = new UserService();
service = new CacheDecorator(service, 300000);   // 5-min cache
service = new RetryDecorator(service, 3, 500);   // retry 3 times
service = new LoggingDecorator(service);          // log everything

// Client uses the same interface regardless of decorators:
const user = await service.getUser(42);
// Logged, retried on failure, cached — transparently!
```

---

## 14. Facade

### 🎯 Intent
Provide a **simplified interface** to a complex subsystem. Hide complexity behind a simple "front door."

### 🌍 Real World Analogy
A **home theater** — to watch a movie you need to: turn on TV, switch to HDMI, turn on receiver, set volume, turn on Blu-ray, insert disc, dim lights... OR press ONE button on a "Movie Mode" remote. That button is the facade.

### 💡 Key Insight
The facade doesn't prevent advanced users from accessing the subsystem directly. It just makes the common use case easy.

```javascript
// ─── Complex subsystems ───────────────────────────
class DVDPlayer {
  on()   { console.log('DVD Player on'); }
  off()  { console.log('DVD Player off'); }
  play(movie) { console.log(`Playing "${movie}"`); }
  stop() { console.log('DVD stopped'); }
  eject(){ console.log('DVD ejected'); }
}

class Amplifier {
  on()         { console.log('Amplifier on'); }
  off()        { console.log('Amplifier off'); }
  setVolume(v) { console.log(`Volume set to ${v}`); }
  setSurroundSound() { console.log('Surround sound on'); }
}

class Projector {
  on()    { console.log('Projector on'); }
  off()   { console.log('Projector off'); }
  wideScreenMode() { console.log('Widescreen mode'); }
}

class Lights {
  dim(level)  { console.log(`Lights dimmed to ${level}%`); }
  brighten()  { console.log('Lights at full brightness'); }
}

class Screen {
  down() { console.log('Screen down'); }
  up()   { console.log('Screen up'); }
}

class PopcornMaker {
  on()  { console.log('Popcorn maker on'); }
  off() { console.log('Popcorn maker off'); }
  pop() { console.log('Popping popcorn!'); }
}

// ─── FACADE: Simple interface to all the complexity ─
class HomeTheaterFacade {
  constructor({ dvd, amp, projector, lights, screen, popcorn }) {
    this.dvd       = dvd;
    this.amp       = amp;
    this.projector = projector;
    this.lights    = lights;
    this.screen    = screen;
    this.popcorn   = popcorn;
  }

  // One method to "watch a movie" instead of 11 steps
  watchMovie(movie) {
    console.log('\n🎬 Get ready to watch a movie...\n');
    this.popcorn.on();
    this.popcorn.pop();
    this.lights.dim(10);
    this.screen.down();
    this.projector.on();
    this.projector.wideScreenMode();
    this.amp.on();
    this.amp.setSurroundSound();
    this.amp.setVolume(5);
    this.dvd.on();
    this.dvd.play(movie);
    console.log('\n🍿 Enjoy the show!\n');
  }

  endMovie() {
    console.log('\n🌟 Shutting down theater...\n');
    this.popcorn.off();
    this.lights.brighten();
    this.screen.up();
    this.projector.off();
    this.amp.off();
    this.dvd.stop();
    this.dvd.eject();
    this.dvd.off();
  }

  pause() {
    this.dvd.stop();
    this.amp.setVolume(0);
    this.lights.dim(50);
  }

  resume(movie) {
    this.lights.dim(10);
    this.amp.setVolume(5);
    this.dvd.play(movie);
  }
}

const theater = new HomeTheaterFacade({
  dvd:       new DVDPlayer(),
  amp:       new Amplifier(),
  projector: new Projector(),
  lights:    new Lights(),
  screen:    new Screen(),
  popcorn:   new PopcornMaker(),
});

theater.watchMovie('Inception'); // One call instead of 11!
theater.endMovie();

// ─── Real-World: API Facade ───────────────────────
class OrderFacade {
  constructor({ userService, inventoryService, paymentService,
                emailService, shippingService }) {
    this.userService      = userService;
    this.inventoryService = inventoryService;
    this.paymentService   = paymentService;
    this.emailService     = emailService;
    this.shippingService  = shippingService;
  }

  // Simple interface for a complex multi-step process
  async placeOrder(userId, cartItems, paymentDetails) {
    // 1. Validate user
    const user = await this.userService.getUser(userId);
    if (!user) throw new Error('User not found');

    // 2. Check inventory
    const available = await this.inventoryService.checkAvailability(cartItems);
    if (!available) throw new Error('Some items out of stock');

    // 3. Calculate total
    const total = await this.inventoryService.calculateTotal(cartItems);

    // 4. Process payment
    const payment = await this.paymentService.charge(total, paymentDetails);
    if (!payment.success) throw new Error('Payment failed');

    // 5. Reserve inventory
    await this.inventoryService.reserve(cartItems);

    // 6. Create shipping order
    const shipping = await this.shippingService.createOrder(user.address, cartItems);

    // 7. Send confirmation email
    await this.emailService.sendOrderConfirmation(user.email, {
      items: cartItems,
      total,
      trackingNumber: shipping.trackingNumber,
    });

    return {
      orderId:        payment.transactionId,
      trackingNumber: shipping.trackingNumber,
      total,
    };
  }
}

// Client just calls ONE method:
const orderFacade = new OrderFacade({ /* inject services */ });
const result = await orderFacade.placeOrder(userId, cart, payment);
console.log(`Order placed! Tracking: ${result.trackingNumber}`);
```

---

## 15. Flyweight

### 🎯 Intent
Use **sharing to efficiently support large numbers of fine-grained objects**. Separate intrinsic (shared, immutable) state from extrinsic (unique, context-dependent) state.

### 🌍 Real World Analogy
A **forest rendering** in a game. You have 100,000 trees. Each tree has the same texture/mesh (intrinsic), but different positions and sizes (extrinsic). Store the mesh ONCE, share it among all trees.

### 💡 Key Insight
The flyweight is the **intrinsic state** (shared). The **extrinsic state** is passed to methods or stored outside.

```javascript
// ─── Without Flyweight: Memory Problem ───────────
class TreeBefore {
  constructor(x, y, type, color, texture, mesh) {
    this.x = x;                  // unique
    this.y = y;                  // unique
    this.type = type;            // shared (e.g., 'oak')
    this.color = color;          // shared per type
    this.texture = texture;      // shared — HUGE data!
    this.mesh = mesh;            // shared — HUGE data!
  }
}
// 100,000 trees × (1MB texture) = 100GB! 💀

// ─── With Flyweight: Shared Intrinsic State ───────
class TreeType {
  // Intrinsic state — shared among all trees of this type
  constructor(name, color, texture) {
    this.name    = name;
    this.color   = color;
    this.texture = texture;  // Heavy data — only stored ONCE per type!
    console.log(`TreeType "${name}" created (heavy object)`);
  }

  // Extrinsic state (x, y, size) passed as parameters
  draw(x, y, size) {
    console.log(`Drawing ${this.name} tree at (${x},${y}) size=${size} color=${this.color}`);
  }
}

// Flyweight Factory — ensures we reuse existing flyweights
class TreeTypeFactory {
  static #types = new Map();

  static getTreeType(name, color, texture) {
    const key = `${name}_${color}`;
    if (!this.#types.has(key)) {
      this.#types.set(key, new TreeType(name, color, texture));
    }
    return this.#types.get(key);
  }

  static get count() { return this.#types.size; }
}

// Context — stores extrinsic state (position, size)
class Tree {
  constructor(x, y, size, type) {
    this.x    = x;       // Extrinsic
    this.y    = y;       // Extrinsic
    this.size = size;    // Extrinsic
    this.type = type;    // Reference to shared flyweight
  }

  draw() {
    this.type.draw(this.x, this.y, this.size); // Passes extrinsic to flyweight
  }
}

// Forest
class Forest {
  constructor() {
    this.trees = [];
  }

  plantTree(x, y, size, name, color, texture) {
    const type = TreeTypeFactory.getTreeType(name, color, texture);
    const tree = new Tree(x, y, size, type);
    this.trees.push(tree);
  }

  draw() {
    this.trees.forEach(t => t.draw());
  }
}

const forest = new Forest();

// Plant 100,000 trees
for (let i = 0; i < 100000; i++) {
  const types = ['Oak', 'Pine', 'Maple', 'Birch', 'Cedar'];
  const colors = ['dark-green', 'light-green', 'autumn-red', 'white', 'olive'];
  const idx = Math.floor(Math.random() * types.length);

  forest.plantTree(
    Math.random() * 5000,  // x
    Math.random() * 5000,  // y
    Math.random() * 50 + 10, // size
    types[idx],
    colors[idx],
    `texture_data_for_${types[idx].toLowerCase()}`
  );
}

console.log(`Trees planted: 100,000`);
console.log(`TreeType objects created: ${TreeTypeFactory.count}`); // Only 5!
// Memory: 5 × heavy_texture + 100,000 × small_position = tiny!
```

---

## 16. Proxy

### 🎯 Intent
Provide a **surrogate or placeholder** for another object to control access to it. The proxy has the same interface as the real object.

### 🌍 Real World Analogy
A **credit card** is a proxy for your bank account. It has the same interface (you can pay with it), but it adds control (spending limits, fraud detection, logging).

### 💡 Key Insight
The proxy intercepts requests to the real object and can add behavior: access control, caching, logging, lazy initialization, remote access.

```javascript
// ─── Types of Proxy ───────────────────────────────

// 1. Virtual Proxy — Lazy initialization
class HeavyReport {
  constructor() {
    console.log('⚠️  Loading 100MB of data from database...');
    this.data = 'HUGE_DATASET'; // Simulating expensive initialization
  }

  generate() {
    return `Report: ${this.data}`;
  }
}

class ReportProxy {
  constructor() {
    this.report = null;  // Not loaded yet!
  }

  generate() {
    if (!this.report) {
      console.log('First access — loading report...');
      this.report = new HeavyReport();  // Lazy initialization
    }
    return this.report.generate();
  }
}

const report = new ReportProxy(); // Instant — nothing loaded yet!
// ... sometime later, when actually needed:
console.log(report.generate()); // Now it loads

// 2. Protection Proxy — Access control
class BankAccount {
  constructor(balance) {
    this.balance = balance;
  }

  deposit(amount)  { this.balance += amount; return this.balance; }
  withdraw(amount) { this.balance -= amount; return this.balance; }
  getBalance()     { return this.balance; }
}

class BankAccountProxy {
  constructor(account, userRole) {
    this.account  = account;
    this.userRole = userRole;
  }

  deposit(amount) {
    this.#log('deposit', amount);
    if (this.userRole === 'viewer') throw new Error('Viewers cannot deposit!');
    return this.account.deposit(amount);
  }

  withdraw(amount) {
    this.#log('withdraw', amount);
    if (this.userRole === 'viewer') throw new Error('Viewers cannot withdraw!');
    if (amount > 10000 && this.userRole !== 'admin') {
      throw new Error('Large withdrawals require admin role!');
    }
    return this.account.withdraw(amount);
  }

  getBalance() {
    this.#log('getBalance');
    return this.account.getBalance();  // Everyone can check balance
  }

  #log(action, amount = null) {
    const msg = amount !== null
      ? `[AUDIT] ${this.userRole} performed ${action}($${amount})`
      : `[AUDIT] ${this.userRole} performed ${action}`;
    console.log(msg);
  }
}

const account    = new BankAccount(1000);
const userProxy  = new BankAccountProxy(account, 'user');
const adminProxy = new BankAccountProxy(account, 'admin');

userProxy.deposit(200);          // Logs: user performed deposit($200)
userProxy.getBalance();          // 1200

try {
  userProxy.withdraw(15000);     // Error: Large withdrawals require admin role!
} catch (e) { console.error(e.message); }

adminProxy.withdraw(15000);      // OK — admin can do large withdrawals

// 3. Caching Proxy
class WeatherService {
  async getWeather(city) {
    console.log(`[API] Fetching weather for ${city}...`);
    // Simulate API call
    await new Promise(r => setTimeout(r, 1000));
    return { city, temp: 22, condition: 'Sunny' };
  }
}

class WeatherProxy {
  constructor(service, cacheTimeMs = 300000) {
    this.service = service;
    this.cache   = new Map();
    this.ttl     = cacheTimeMs;
  }

  async getWeather(city) {
    const cached = this.cache.get(city);
    if (cached && Date.now() - cached.time < this.ttl) {
      console.log(`[CACHE] Returning cached weather for ${city}`);
      return cached.data;
    }

    const data = await this.service.getWeather(city);
    this.cache.set(city, { data, time: Date.now() });
    return data;
  }
}

const weather = new WeatherProxy(new WeatherService());
await weather.getWeather('London');  // API call
await weather.getWeather('London');  // Cache hit! Instant.

// 4. JavaScript's built-in Proxy
const validator = new Proxy({}, {
  set(target, prop, value) {
    if (prop === 'age' && (typeof value !== 'number' || value < 0)) {
      throw new TypeError('Age must be a non-negative number');
    }
    if (prop === 'name' && typeof value !== 'string') {
      throw new TypeError('Name must be a string');
    }
    target[prop] = value;
    return true;
  },
  get(target, prop) {
    return prop in target ? target[prop] : `Property "${prop}" not found`;
  }
});

validator.name = 'Alice';  // OK
validator.age  = 30;       // OK
validator.age  = -5;       // TypeError!
```

---

# PART 4 — BEHAVIORAL PATTERNS

---

## 17. Chain of Responsibility

### 🎯 Intent
Pass requests along a **chain of handlers**. Each handler decides to either process the request or pass it to the next handler in the chain.

### 🌍 Real World Analogy
**Customer service escalation** — You call support, the chatbot tries to help. If it can't, escalates to Level 1 agent. If too complex, escalates to Level 2. Eventually reaches a manager.

```javascript
// ─── Handler base class ───────────────────────────
class Handler {
  constructor() {
    this.next = null;
  }

  setNext(handler) {
    this.next = handler;
    return handler; // Enable chaining: h1.setNext(h2).setNext(h3)
  }

  handle(request) {
    if (this.next) return this.next.handle(request);
    return null; // End of chain
  }
}

// ─── Concrete Handlers ────────────────────────────
class AuthHandler extends Handler {
  handle(request) {
    if (!request.token) {
      return { error: 401, message: 'Unauthorized: No token provided' };
    }
    if (request.token !== 'valid-token') {
      return { error: 401, message: 'Unauthorized: Invalid token' };
    }
    console.log('[Auth] ✓ Token valid');
    return super.handle(request); // Pass to next handler
  }
}

class RateLimitHandler extends Handler {
  constructor(maxRequests = 100) {
    super();
    this.requests = new Map(); // IP → count
    this.maxRequests = maxRequests;
  }

  handle(request) {
    const count = (this.requests.get(request.ip) || 0) + 1;
    this.requests.set(request.ip, count);

    if (count > this.maxRequests) {
      return { error: 429, message: 'Too Many Requests' };
    }
    console.log(`[RateLimit] ✓ ${count}/${this.maxRequests} requests from ${request.ip}`);
    return super.handle(request);
  }
}

class ValidationHandler extends Handler {
  handle(request) {
    if (!request.body) {
      return { error: 400, message: 'Bad Request: Missing body' };
    }
    if (!request.body.email?.includes('@')) {
      return { error: 400, message: 'Bad Request: Invalid email' };
    }
    console.log('[Validation] ✓ Request is valid');
    return super.handle(request);
  }
}

class BusinessLogicHandler extends Handler {
  handle(request) {
    console.log('[BusinessLogic] Processing request...');
    return { status: 200, data: { userId: 42, message: 'Success!' } };
  }
}

// Build the chain
const auth        = new AuthHandler();
const rateLimit   = new RateLimitHandler(100);
const validation  = new ValidationHandler();
const business    = new BusinessLogicHandler();

// Link the chain
auth.setNext(rateLimit).setNext(validation).setNext(business);

// Test
const request1 = { token: 'valid-token', ip: '192.168.1.1', body: { email: 'a@b.com' } };
console.log(auth.handle(request1)); // { status: 200, data: {...} }

const request2 = { ip: '192.168.1.1', body: { email: 'a@b.com' } }; // No token
console.log(auth.handle(request2)); // { error: 401, message: 'Unauthorized...' }

// ─── Discount Chain ───────────────────────────────
class DiscountHandler {
  constructor(minOrderValue, discountPercent) {
    this.minOrderValue   = minOrderValue;
    this.discountPercent = discountPercent;
    this.next = null;
  }

  setNext(handler) { this.next = handler; return handler; }

  handle(order) {
    if (order.total >= this.minOrderValue) {
      const discount = order.total * (this.discountPercent / 100);
      console.log(`Applying ${this.discountPercent}% discount: -$${discount.toFixed(2)}`);
      return { discount, finalTotal: order.total - discount };
    }
    return this.next ? this.next.handle(order) : { discount: 0, finalTotal: order.total };
  }
}

const premiumDiscount = new DiscountHandler(1000, 20); // 20% off orders > $1000
const goldDiscount    = new DiscountHandler(500,  15); // 15% off orders > $500
const silverDiscount  = new DiscountHandler(100,  10); // 10% off orders > $100
const noDiscount      = new DiscountHandler(0,     0); // No discount

premiumDiscount.setNext(goldDiscount).setNext(silverDiscount).setNext(noDiscount);

console.log(premiumDiscount.handle({ total: 1500 })); // 20% off
console.log(premiumDiscount.handle({ total: 600 }));  // 15% off
console.log(premiumDiscount.handle({ total: 150 }));  // 10% off
console.log(premiumDiscount.handle({ total: 50 }));   // 0% off
```

---

## 18. Command

### 🎯 Intent
Encapsulate a request as an **object**, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

### 🌍 Real World Analogy
A **restaurant order slip** — the waiter writes your order (command object), hands it to the kitchen (invoker), and the cook (receiver) prepares it. You can cancel an order, or even replay past orders.

```javascript
// ─── Command Interface ────────────────────────────
class Command {
  execute() { throw new Error('implement execute()'); }
  undo()    { throw new Error('implement undo()'); }
}

// ─── Receiver ─────────────────────────────────────
class TextEditor {
  constructor() {
    this.content = '';
    this.cursor  = 0;
  }

  insertText(text, position) {
    this.content = this.content.slice(0, position)
                 + text
                 + this.content.slice(position);
    this.cursor = position + text.length;
  }

  deleteText(start, length) {
    const deleted = this.content.slice(start, start + length);
    this.content  = this.content.slice(0, start) + this.content.slice(start + length);
    return deleted;
  }

  getText() { return this.content; }
}

// ─── Concrete Commands ────────────────────────────
class InsertCommand extends Command {
  constructor(editor, text, position) {
    super();
    this.editor   = editor;
    this.text     = text;
    this.position = position;
  }

  execute() {
    this.editor.insertText(this.text, this.position);
  }

  undo() {
    this.editor.deleteText(this.position, this.text.length);
  }
}

class DeleteCommand extends Command {
  constructor(editor, start, length) {
    super();
    this.editor  = editor;
    this.start   = start;
    this.length  = length;
    this.deleted = ''; // Store for undo
  }

  execute() {
    this.deleted = this.editor.deleteText(this.start, this.length);
  }

  undo() {
    this.editor.insertText(this.deleted, this.start);
  }
}

class BoldCommand extends Command {
  constructor(editor, start, end) {
    super();
    this.editor = editor;
    this.start  = start;
    this.end    = end;
  }

  execute() {
    const text = this.editor.content.slice(this.start, this.end);
    this.editor.content =
      this.editor.content.slice(0, this.start)
      + `**${text}**`
      + this.editor.content.slice(this.end);
  }

  undo() {
    // Remove the ** wrappers
    this.editor.content = this.editor.content.replace(/\*\*(.*?)\*\*/g, '$1');
  }
}

// ─── Invoker: manages command history ────────────
class CommandHistory {
  constructor() {
    this.undoStack = [];
    this.redoStack = [];
  }

  execute(command) {
    command.execute();
    this.undoStack.push(command);
    this.redoStack = []; // Clear redo stack after new action
    console.log(`Executed: ${command.constructor.name}`);
  }

  undo() {
    if (this.undoStack.length === 0) {
      console.log('Nothing to undo');
      return;
    }
    const command = this.undoStack.pop();
    command.undo();
    this.redoStack.push(command);
    console.log(`Undone: ${command.constructor.name}`);
  }

  redo() {
    if (this.redoStack.length === 0) {
      console.log('Nothing to redo');
      return;
    }
    const command = this.redoStack.pop();
    command.execute();
    this.undoStack.push(command);
    console.log(`Redone: ${command.constructor.name}`);
  }

  canUndo() { return this.undoStack.length > 0; }
  canRedo() { return this.redoStack.length > 0; }
}

// ─── Usage ────────────────────────────────────────
const editor  = new TextEditor();
const history = new CommandHistory();

history.execute(new InsertCommand(editor, 'Hello', 0));
history.execute(new InsertCommand(editor, ' World', 5));
console.log(editor.getText()); // "Hello World"

history.execute(new DeleteCommand(editor, 5, 6));
console.log(editor.getText()); // "Hello"

history.undo(); // Undo delete
console.log(editor.getText()); // "Hello World"

history.undo(); // Undo " World" insert
console.log(editor.getText()); // "Hello"

history.redo(); // Redo " World" insert
console.log(editor.getText()); // "Hello World"

// ─── Command Queue / Macro Commands ───────────────
class MacroCommand extends Command {
  constructor(commands) {
    super();
    this.commands = commands;
  }

  execute() {
    this.commands.forEach(cmd => cmd.execute());
  }

  undo() {
    // Undo in reverse order!
    [...this.commands].reverse().forEach(cmd => cmd.undo());
  }
}

// Execute multiple commands as one atomic operation
const macro = new MacroCommand([
  new InsertCommand(editor, 'Title: ', 0),
  new BoldCommand(editor, 0, 7),
]);

history.execute(macro);
history.undo(); // Undoes both at once
```

---

## 19. Iterator

### 🎯 Intent
Provide a way to **sequentially access elements** of a collection without exposing its underlying structure.

### 💡 Key Insight
Separate the traversal algorithm from the collection. The collection provides an iterator; clients use `next()` without caring how data is stored.

```javascript
// ─── Custom Iterator ──────────────────────────────
class Range {
  constructor(start, end, step = 1) {
    this.start = start;
    this.end   = end;
    this.step  = step;
  }

  // Make the Range iterable (JavaScript protocol)
  [Symbol.iterator]() {
    let current = this.start;
    const end   = this.end;
    const step  = this.step;

    return {
      next() {
        if (current <= end) {
          const value = current;
          current += step;
          return { value, done: false };
        }
        return { value: undefined, done: true };
      }
    };
  }
}

const range = new Range(1, 10, 2);
for (const n of range) {
  process.stdout.write(n + ' '); // 1 3 5 7 9
}
console.log([...range]); // [1, 3, 5, 7, 9]

// ─── Tree Iterator ────────────────────────────────
class BinaryTree {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left  = left;
    this.right = right;
  }

  // Inorder iterator (Left, Root, Right)
  *[Symbol.iterator]() {
    if (this.left)  yield* this.left;   // recurse left
    yield this.value;                    // visit root
    if (this.right) yield* this.right;  // recurse right
  }

  // BFS iterator
  *bfs() {
    const queue = [this];
    while (queue.length > 0) {
      const node = queue.shift();
      yield node.value;
      if (node.left)  queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
  }
}

const tree = new BinaryTree(4,
  new BinaryTree(2,
    new BinaryTree(1),
    new BinaryTree(3)
  ),
  new BinaryTree(6,
    new BinaryTree(5),
    new BinaryTree(7)
  )
);

console.log([...tree]);         // [1, 2, 3, 4, 5, 6, 7] (inorder = sorted!)
console.log([...tree.bfs()]);   // [4, 2, 6, 1, 3, 5, 7] (level-order)

// ─── Infinite Iterator ────────────────────────────
function* fibonacci() {
  let [a, b] = [0, 1];
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

function take(iterator, n) {
  const result = [];
  for (const value of iterator) {
    result.push(value);
    if (result.length >= n) break;
  }
  return result;
}

console.log(take(fibonacci(), 10)); // [0,1,1,2,3,5,8,13,21,34]
```

---

## 20. Mediator

### 🎯 Intent
Define an object that **encapsulates how a set of objects interact**. Objects don't communicate directly; they go through the mediator. Reduces chaotic many-to-many dependencies into a clean star pattern.

### 🌍 Real World Analogy
An **air traffic controller** — planes don't talk to each other directly (chaos!). All communication goes through the control tower (mediator).

```javascript
// ─── Chat Room Example ────────────────────────────
class ChatRoom {
  constructor() {
    this.users = new Map();
    this.history = [];
  }

  register(user) {
    this.users.set(user.name, user);
    user.setRoom(this);
    this.#broadcast(`${user.name} joined the room`, null);
  }

  leave(user) {
    this.users.delete(user.name);
    this.#broadcast(`${user.name} left the room`, null);
  }

  sendMessage(from, message, to = null) {
    const entry = {
      from:      from.name,
      message,
      to:        to?.name || 'everyone',
      timestamp: new Date().toISOString()
    };
    this.history.push(entry);

    if (to) {
      // Private message
      const recipient = this.users.get(to.name);
      if (recipient) recipient.receive(from.name, message, true);
    } else {
      // Broadcast
      this.#broadcast(message, from.name);
    }
  }

  #broadcast(message, fromName) {
    this.users.forEach((user, name) => {
      if (name !== fromName) user.receive(fromName || 'System', message, false);
    });
  }
}

class ChatUser {
  constructor(name) {
    this.name = name;
    this.room = null;
  }

  setRoom(room)   { this.room = room; }
  send(message, to = null) { this.room?.sendMessage(this, message, to); }

  receive(from, message, isPrivate) {
    const prefix = isPrivate ? '[DM]' : '';
    console.log(`${this.name} received ${prefix} from ${from}: "${message}"`);
  }
}

// Users don't know about each other — only the mediator
const room  = new ChatRoom();
const alice = new ChatUser('Alice');
const bob   = new ChatUser('Bob');
const carol = new ChatUser('Carol');

room.register(alice);
room.register(bob);
room.register(carol);

alice.send('Hello everyone!');           // Bob and Carol receive it
bob.send('Hi Alice!', alice);            // Only Alice receives this (private)
carol.send('Good morning!');             // Alice and Bob receive it

// ─── Event Bus / Mediator ─────────────────────────
class EventBus {
  #handlers = new Map();

  subscribe(event, handler, id = null) {
    if (!this.#handlers.has(event)) this.#handlers.set(event, []);
    const subscription = { handler, id: id || Symbol() };
    this.#handlers.get(event).push(subscription);
    return subscription.id;
  }

  unsubscribe(event, id) {
    const handlers = this.#handlers.get(event) || [];
    this.#handlers.set(event, handlers.filter(h => h.id !== id));
  }

  publish(event, data = null) {
    const handlers = this.#handlers.get(event) || [];
    handlers.forEach(({ handler }) => {
      try {
        handler(data);
      } catch (error) {
        console.error(`Error in handler for "${event}":`, error);
      }
    });
  }

  once(event, handler) {
    const id = this.subscribe(event, (data) => {
      handler(data);
      this.unsubscribe(event, id);
    });
    return id;
  }
}

const bus = new EventBus();

// Components subscribe to events without knowing about each other
bus.subscribe('user:login', ({ userId, name }) => {
  console.log(`Dashboard: Welcome back, ${name}!`);
});

bus.subscribe('user:login', ({ userId }) => {
  console.log(`Analytics: User ${userId} logged in`);
});

bus.subscribe('order:placed', ({ orderId, total }) => {
  console.log(`Email: Sending confirmation for order ${orderId}`);
});

bus.subscribe('order:placed', ({ orderId, total }) => {
  console.log(`Inventory: Reserving items for order ${orderId}`);
});

// Publish events — publisher doesn't know who handles them
bus.publish('user:login',  { userId: 1, name: 'Alice' });
bus.publish('order:placed', { orderId: 'ORD-001', total: 99.99 });
```

---

## 21. Memento

### 🎯 Intent
**Capture and externalize an object's internal state** so it can be restored later, without violating encapsulation.

### 🌍 Real World Analogy
**Video game save points** — you save your game state, play, die, and restore from the save point. The save file stores your state without exposing the game's internals.

```javascript
// ─── Memento ──────────────────────────────────────
class EditorMemento {
  // Immutable snapshot of state
  constructor(content, cursorPos, selection) {
    this.#content   = content;
    this.#cursorPos = cursorPos;
    this.#selection = selection;
    this.#timestamp = new Date().toISOString();
    Object.freeze(this); // Immutable!
  }

  #content;
  #cursorPos;
  #selection;
  #timestamp;

  get content()   { return this.#content; }
  get cursorPos() { return this.#cursorPos; }
  get selection() { return this.#selection; }
  get timestamp() { return this.#timestamp; }
}

// ─── Originator: object whose state we save ───────
class TextEditor {
  #content   = '';
  #cursorPos = 0;
  #selection = null;

  type(text) {
    this.#content = this.#content.slice(0, this.#cursorPos)
                  + text
                  + this.#content.slice(this.#cursorPos);
    this.#cursorPos += text.length;
  }

  select(start, end) {
    this.#selection = { start, end };
    this.#cursorPos = end;
  }

  delete() {
    if (this.#selection) {
      const { start, end } = this.#selection;
      this.#content   = this.#content.slice(0, start) + this.#content.slice(end);
      this.#cursorPos = start;
      this.#selection = null;
    } else if (this.#cursorPos > 0) {
      this.#content = this.#content.slice(0, this.#cursorPos - 1)
                    + this.#content.slice(this.#cursorPos);
      this.#cursorPos--;
    }
  }

  // Save state to memento
  save() {
    return new EditorMemento(
      this.#content,
      this.#cursorPos,
      this.#selection ? { ...this.#selection } : null
    );
  }

  // Restore state from memento
  restore(memento) {
    this.#content   = memento.content;
    this.#cursorPos = memento.cursorPos;
    this.#selection = memento.selection;
  }

  get content() { return this.#content; }
}

// ─── Caretaker: manages the undo/redo history ─────
class History {
  #undoStack = [];
  #redoStack = [];

  save(memento) {
    this.#undoStack.push(memento);
    this.#redoStack = []; // Clear redo on new save
  }

  undo(currentMemento) {
    if (this.#undoStack.length === 0) return null;
    this.#redoStack.push(currentMemento); // Save current for redo
    return this.#undoStack.pop();
  }

  redo(currentMemento) {
    if (this.#redoStack.length === 0) return null;
    this.#undoStack.push(currentMemento); // Save current for undo
    return this.#redoStack.pop();
  }

  get snapshots() { return this.#undoStack.map(m => m.timestamp); }
}

// Usage
const editor  = new TextEditor();
const history = new History();

const snap = () => history.save(editor.save());
const undo  = () => {
  const prev = history.undo(editor.save());
  if (prev) editor.restore(prev);
};

snap(); editor.type('Hello');
snap(); editor.type(' World');
snap(); editor.type('!');

console.log(editor.content); // "Hello World!"

undo();
console.log(editor.content); // "Hello World"

undo();
console.log(editor.content); // "Hello"

undo();
console.log(editor.content); // ""
```

---

## 22. Observer

### 🎯 Intent
Define a one-to-many dependency so that when one object changes state, **all its dependents are notified automatically**.

### 🌍 Real World Analogy
A **newspaper subscription** — you subscribe to a newspaper. Whenever a new edition is published (subject changes), all subscribers receive it automatically. You can unsubscribe anytime.

### 💡 Key Insight
The subject (publisher) maintains a list of observers (subscribers) and notifies them on changes. Neither knows the concrete type of the other.

```javascript
// ─── Core Observer Implementation ────────────────
class EventEmitter {
  #events = new Map();

  on(event, listener) {
    if (!this.#events.has(event)) this.#events.set(event, new Set());
    this.#events.get(event).add(listener);
    return () => this.off(event, listener); // Return unsubscribe function
  }

  off(event, listener) {
    this.#events.get(event)?.delete(listener);
  }

  once(event, listener) {
    const wrapper = (...args) => {
      listener(...args);
      this.off(event, wrapper);
    };
    return this.on(event, wrapper);
  }

  emit(event, ...args) {
    this.#events.get(event)?.forEach(listener => {
      try { listener(...args); }
      catch(e) { console.error(`Error in "${event}" listener:`, e); }
    });
  }

  listenerCount(event) {
    return this.#events.get(event)?.size || 0;
  }
}

// ─── Observable Store (like MobX/Zustand) ─────────
class Store extends EventEmitter {
  #state;

  constructor(initialState) {
    super();
    this.#state = { ...initialState };
  }

  get state() {
    return { ...this.#state }; // Return copy — immutable access
  }

  setState(updater) {
    const prevState = { ...this.#state };
    const newState  = typeof updater === 'function'
      ? updater(this.#state)
      : updater;

    this.#state = { ...this.#state, ...newState };

    // Notify observers of what changed
    const changed = Object.keys(newState).filter(
      key => prevState[key] !== this.#state[key]
    );
    changed.forEach(key => this.emit(`change:${key}`, this.#state[key], prevState[key]));
    if (changed.length > 0) this.emit('change', this.#state, prevState);
  }
}

// Usage — reactive UI
const store = new Store({ user: null, cart: [], theme: 'light', count: 0 });

// Subscribe to specific property changes
store.on('change:user', (newUser, prevUser) => {
  console.log(`[UI] User changed:`, newUser ? newUser.name : 'logged out');
});

store.on('change:theme', (newTheme) => {
  console.log(`[UI] Theme switched to: ${newTheme}`);
  document.body?.classList.toggle('dark', newTheme === 'dark');
});

store.on('change:cart', (newCart) => {
  console.log(`[UI] Cart updated: ${newCart.length} items`);
});

// Log all state changes
store.on('change', (newState) => {
  console.log('[DEBUG] State updated:', newState);
});

// Mutations trigger notifications automatically
store.setState({ user: { id: 1, name: 'Alice' } }); // Logs user change
store.setState({ theme: 'dark' });                    // Logs theme change
store.setState(state => ({ count: state.count + 1 })); // Functional update

// ─── Real-World: Stock Price Monitor ──────────────
class StockMarket extends EventEmitter {
  #stocks = new Map();

  updatePrice(symbol, price) {
    const previous = this.#stocks.get(symbol) || price;
    this.#stocks.set(symbol, price);
    const change    = price - previous;
    const changePct = ((change / previous) * 100).toFixed(2);

    this.emit('price-update', { symbol, price, previous, change, changePct });

    if (Math.abs(change) / previous > 0.05) {
      this.emit('big-move', { symbol, changePct });
    }
  }
}

const market = new StockMarket();

// Portfolio tracker
market.on('price-update', ({ symbol, price, changePct }) => {
  const arrow = changePct > 0 ? '↑' : '↓';
  console.log(`[Portfolio] ${symbol}: $${price} ${arrow}${Math.abs(changePct)}%`);
});

// Alert system
market.on('big-move', ({ symbol, changePct }) => {
  console.log(`⚠️  [Alert] ${symbol} moved ${changePct}% — unusual activity!`);
});

market.updatePrice('AAPL', 150.00);
market.updatePrice('AAPL', 160.00); // 6.67% jump → triggers big-move!
```

---

## 23. State

### 🎯 Intent
Allow an object to **alter its behavior when its internal state changes**. The object will appear to change its class.

### 🌍 Real World Analogy
A **traffic light** — it behaves differently in each state (Red: stop, Green: go, Yellow: slow down). The same "light" object changes behavior based on state.

```javascript
// ─── Traffic Light ────────────────────────────────
class TrafficLightState {
  constructor(light) {
    this.light = light;
  }
  handle() { throw new Error('implement'); }
  toString() { throw new Error('implement'); }
}

class RedState extends TrafficLightState {
  handle() {
    console.log('🔴 RED — STOP! Waiting...');
    setTimeout(() => {
      this.light.setState(new GreenState(this.light));
    }, 3000);
  }
  toString() { return 'RED'; }
}

class GreenState extends TrafficLightState {
  handle() {
    console.log('🟢 GREEN — GO!');
    setTimeout(() => {
      this.light.setState(new YellowState(this.light));
    }, 3000);
  }
  toString() { return 'GREEN'; }
}

class YellowState extends TrafficLightState {
  handle() {
    console.log('🟡 YELLOW — SLOW DOWN');
    setTimeout(() => {
      this.light.setState(new RedState(this.light));
    }, 1000);
  }
  toString() { return 'YELLOW'; }
}

class TrafficLight {
  constructor() {
    this.state = new RedState(this);
  }

  setState(state) {
    console.log(`State: ${this.state} → ${state}`);
    this.state = state;
    this.state.handle();
  }

  start() {
    this.state.handle();
  }
}

// ─── Vending Machine (rich state example) ─────────
class VendingMachineState {
  insertCoin(machine, amount) { console.log('Invalid action in this state'); }
  selectProduct(machine, product) { console.log('Invalid action in this state'); }
  dispense(machine) { console.log('Invalid action in this state'); }
  refund(machine) { console.log('Invalid action in this state'); }
}

class IdleState extends VendingMachineState {
  insertCoin(machine, amount) {
    machine.balance += amount;
    console.log(`Coin inserted: $${amount}. Balance: $${machine.balance}`);
    machine.setState(new HasCoinState());
  }

  refund(machine) {
    console.log('No coins inserted');
  }
}

class HasCoinState extends VendingMachineState {
  insertCoin(machine, amount) {
    machine.balance += amount;
    console.log(`Additional coin: $${amount}. Balance: $${machine.balance}`);
  }

  selectProduct(machine, product) {
    if (!machine.products[product]) {
      console.log(`Product "${product}" not available`);
      return;
    }
    const price = machine.products[product].price;
    if (machine.balance < price) {
      console.log(`Insufficient funds. Need $${price - machine.balance} more`);
      return;
    }
    machine.selectedProduct = product;
    console.log(`Selected: ${product} ($${price})`);
    machine.setState(new DispensingState());
    machine.state.dispense(machine);
  }

  refund(machine) {
    console.log(`Refunding $${machine.balance}`);
    machine.balance = 0;
    machine.setState(new IdleState());
  }
}

class DispensingState extends VendingMachineState {
  dispense(machine) {
    const product = machine.selectedProduct;
    const price   = machine.products[product].price;
    machine.balance -= price;
    machine.products[product].quantity--;
    console.log(`🥤 Dispensing ${product}! Change: $${machine.balance}`);
    machine.balance = 0;

    if (machine.products[product].quantity === 0) {
      delete machine.products[product];
    }
    machine.selectedProduct = null;
    machine.setState(new IdleState());
  }
}

class VendingMachine {
  constructor() {
    this.state   = new IdleState();
    this.balance = 0;
    this.selectedProduct = null;
    this.products = {
      'Cola':   { price: 1.50, quantity: 5 },
      'Water':  { price: 1.00, quantity: 3 },
      'Chips':  { price: 2.00, quantity: 2 },
    };
  }

  setState(state) { this.state = state; }

  insertCoin(amount)      { this.state.insertCoin(this, amount); }
  selectProduct(product)  { this.state.selectProduct(this, product); }
  refund()                { this.state.refund(this); }
}

const vm = new VendingMachine();
vm.insertCoin(1.00);
vm.insertCoin(0.50);
vm.selectProduct('Cola');  // $1.50 — exact change!
vm.insertCoin(2.00);
vm.selectProduct('Chips'); // $2.00 — exact change!
```

---

## 24. Strategy

### 🎯 Intent
Define a family of algorithms, encapsulate each one, and make them **interchangeable**. Strategy lets the algorithm vary independently from clients that use it.

### 🌍 Real World Analogy
**Sorting options on Amazon** — "Sort by: Price Low to High", "Sort by: Relevance", "Sort by: Customer Rating". The list is the same; only the sorting strategy changes. You can swap strategies at runtime.

```javascript
// ─── Sorting Strategy ─────────────────────────────
class SortStrategy {
  sort(data) { throw new Error('implement sort()'); }
  get name()  { throw new Error('implement name'); }
}

class BubbleSortStrategy extends SortStrategy {
  get name() { return 'Bubble Sort O(n²)'; }
  sort(data) {
    const arr = [...data];
    for (let i = 0; i < arr.length - 1; i++)
      for (let j = 0; j < arr.length - i - 1; j++)
        if (arr[j] > arr[j+1]) [arr[j], arr[j+1]] = [arr[j+1], arr[j]];
    return arr;
  }
}

class QuickSortStrategy extends SortStrategy {
  get name() { return 'Quick Sort O(n log n)'; }
  sort(data) {
    if (data.length <= 1) return data;
    const pivot = data[Math.floor(data.length / 2)];
    const left  = data.filter(x => x < pivot);
    const mid   = data.filter(x => x === pivot);
    const right = data.filter(x => x > pivot);
    return [...this.sort(left), ...mid, ...this.sort(right)];
  }
}

class MergeSortStrategy extends SortStrategy {
  get name() { return 'Merge Sort O(n log n)'; }
  sort(data) {
    if (data.length <= 1) return data;
    const mid   = Math.floor(data.length / 2);
    const left  = this.sort(data.slice(0, mid));
    const right = this.sort(data.slice(mid));
    return this.#merge(left, right);
  }
  #merge(left, right) {
    const result = [];
    let i = 0, j = 0;
    while (i < left.length && j < right.length)
      result.push(left[i] <= right[j] ? left[i++] : right[j++]);
    return [...result, ...left.slice(i), ...right.slice(j)];
  }
}

class Sorter {
  #strategy;

  constructor(strategy) {
    this.#strategy = strategy;
  }

  setStrategy(strategy) {
    this.#strategy = strategy;
  }

  sort(data) {
    console.log(`Sorting with: ${this.#strategy.name}`);
    const start  = performance.now();
    const result = this.#strategy.sort(data);
    const end    = performance.now();
    console.log(`Time: ${(end - start).toFixed(3)}ms`);
    return result;
  }
}

const data   = Array.from({ length: 1000 }, () => Math.floor(Math.random() * 1000));
const sorter = new Sorter(new QuickSortStrategy());

sorter.sort(data);
sorter.setStrategy(new MergeSortStrategy());
sorter.sort(data);

// ─── Payment Strategy ─────────────────────────────
class PaymentStrategy {
  pay(amount) { throw new Error('implement pay()'); }
}

class CreditCardStrategy extends PaymentStrategy {
  constructor(cardNumber, cvv, expiry) {
    super();
    this.cardNumber = cardNumber;
    this.cvv        = cvv;
    this.expiry     = expiry;
  }
  pay(amount) {
    console.log(`💳 Charged $${amount} to card ending in ${this.cardNumber.slice(-4)}`);
    return { success: true, method: 'credit_card', amount };
  }
}

class PayPalStrategy extends PaymentStrategy {
  constructor(email) {
    super();
    this.email = email;
  }
  pay(amount) {
    console.log(`🅿️  Charged $${amount} via PayPal to ${this.email}`);
    return { success: true, method: 'paypal', amount };
  }
}

class CryptoStrategy extends PaymentStrategy {
  constructor(walletAddress) {
    super();
    this.wallet = walletAddress;
  }
  pay(amount) {
    const btcAmount = (amount / 65000).toFixed(8); // Fake BTC price
    console.log(`₿ Sent ${btcAmount} BTC to ${this.wallet.slice(0, 8)}...`);
    return { success: true, method: 'crypto', amount };
  }
}

class ShoppingCart {
  constructor() {
    this.items   = [];
    this.payment = null;
  }

  addItem(name, price) {
    this.items.push({ name, price });
    return this;
  }

  setPaymentMethod(strategy) {
    this.payment = strategy;
    return this;
  }

  checkout() {
    const total = this.items.reduce((sum, item) => sum + item.price, 0);
    console.log(`\nOrder total: $${total.toFixed(2)}`);
    return this.payment.pay(total);
  }
}

const cart = new ShoppingCart();
cart.addItem('Laptop', 999.99).addItem('Mouse', 29.99);

// Switch payment strategy at runtime
cart.setPaymentMethod(new CreditCardStrategy('4111111111111111', '123', '12/26'));
cart.checkout();

cart.setPaymentMethod(new PayPalStrategy('alice@example.com'));
cart.checkout();
```

---

## 25. Template Method

### 🎯 Intent
Define the **skeleton of an algorithm** in a base class, deferring some steps to subclasses. Subclasses can override specific steps without changing the algorithm's structure.

### 🌍 Real World Analogy
A **recipe** — the cooking steps are the same (prep, cook, plate, serve) but the specific ingredients and techniques differ per dish.

```javascript
// ─── Data Export Template ─────────────────────────
class DataExporter {
  // Template Method — defines the algorithm skeleton
  export(data, filename) {
    console.log(`\nExporting ${data.length} records...`);

    const validated  = this.validate(data);     // Step 1
    const processed  = this.process(validated);  // Step 2
    const formatted  = this.format(processed);   // Step 3 — subclass overrides
    const header     = this.getHeader();         // Step 4 — subclass overrides
    const footer     = this.getFooter();         // Step 5 — subclass overrides
    const output     = header + formatted + footer;

    this.write(output, filename);                // Step 6
    console.log(`Export complete: ${filename}`);
    return output;
  }

  // Default implementation (can be overridden)
  validate(data) {
    return data.filter(row => row !== null && row !== undefined);
  }

  process(data) {
    return data; // No processing by default
  }

  // Abstract methods — MUST be overridden
  format(data) { throw new Error('format() must be implemented'); }
  getHeader()  { return ''; }
  getFooter()  { return ''; }

  write(content, filename) {
    console.log(`Writing to ${filename}:\n${content.substring(0, 100)}...`);
  }
}

class CSVExporter extends DataExporter {
  format(data) {
    if (data.length === 0) return '';
    const headers = Object.keys(data[0]).join(',');
    const rows    = data.map(row => Object.values(row).join(','));
    return [headers, ...rows].join('\n');
  }

  getHeader() { return ''; } // CSV has no file header
  getFooter() { return '\n'; }
}

class JSONExporter extends DataExporter {
  format(data) {
    return JSON.stringify(data, null, 2);
  }

  getHeader() { return ''; }
  getFooter() { return '\n'; }
}

class XMLExporter extends DataExporter {
  format(data) {
    const rows = data.map(row => {
      const fields = Object.entries(row)
        .map(([k, v]) => `    <${k}>${v}</${k}>`)
        .join('\n');
      return `  <record>\n${fields}\n  </record>`;
    });
    return rows.join('\n');
  }

  getHeader() { return '<?xml version="1.0" encoding="UTF-8"?>\n<data>\n'; }
  getFooter() { return '\n</data>\n'; }
}

const users = [
  { id: 1, name: 'Alice', email: 'alice@example.com', age: 30 },
  { id: 2, name: 'Bob',   email: 'bob@example.com',   age: 25 },
];

new CSVExporter().export(users, 'users.csv');
new JSONExporter().export(users, 'users.json');
new XMLExporter().export(users, 'users.xml');
```

---

## 26. Visitor

### 🎯 Intent
Represent an operation to be performed on elements of an object structure. Visitor lets you define a **new operation without changing the classes** of the elements on which it operates.

### 🌍 Real World Analogy
A **tax auditor** visiting different types of businesses. The auditor (visitor) applies the same visit process but does different calculations depending on whether they're visiting a restaurant, retail store, or software company.

```javascript
// ─── Document Structure ───────────────────────────
class DocumentNode {
  accept(visitor) { throw new Error('implement accept()'); }
}

class Paragraph extends DocumentNode {
  constructor(text, style = 'normal') {
    super();
    this.text  = text;
    this.style = style;
  }
  accept(visitor) { return visitor.visitParagraph(this); }
}

class Heading extends DocumentNode {
  constructor(text, level = 1) {
    super();
    this.text  = text;
    this.level = level;
  }
  accept(visitor) { return visitor.visitHeading(this); }
}

class Image extends DocumentNode {
  constructor(src, alt, width, height) {
    super();
    this.src    = src;
    this.alt    = alt;
    this.width  = width;
    this.height = height;
  }
  accept(visitor) { return visitor.visitImage(this); }
}

class Table extends DocumentNode {
  constructor(rows, cols, data) {
    super();
    this.rows = rows;
    this.cols = cols;
    this.data = data;
  }
  accept(visitor) { return visitor.visitTable(this); }
}

class Document {
  constructor(nodes = []) {
    this.nodes = nodes;
  }
  add(node) { this.nodes.push(node); return this; }
  accept(visitor) {
    return this.nodes.map(node => node.accept(visitor));
  }
}

// ─── Visitors ─────────────────────────────────────
class HTMLExportVisitor {
  visitParagraph(p) {
    return `<p class="${p.style}">${p.text}</p>`;
  }
  visitHeading(h) {
    return `<h${h.level}>${h.text}</h${h.level}>`;
  }
  visitImage(img) {
    return `<img src="${img.src}" alt="${img.alt}" width="${img.width}" height="${img.height}"/>`;
  }
  visitTable(t) {
    const rows = t.data.map(row =>
      `<tr>${row.map(cell => `<td>${cell}</td>`).join('')}</tr>`
    ).join('');
    return `<table>${rows}</table>`;
  }
}

class MarkdownExportVisitor {
  visitParagraph(p) { return `\n${p.text}\n`; }
  visitHeading(h)   { return `${'#'.repeat(h.level)} ${h.text}`; }
  visitImage(img)   { return `![${img.alt}](${img.src})`; }
  visitTable(t) {
    const header   = t.data[0].join(' | ');
    const divider  = t.data[0].map(() => '---').join(' | ');
    const rows     = t.data.slice(1).map(row => row.join(' | '));
    return [header, divider, ...rows].join('\n');
  }
}

class WordCountVisitor {
  constructor() { this.count = 0; }
  visitParagraph(p) { this.count += p.text.split(' ').length; return this.count; }
  visitHeading(h)   { this.count += h.text.split(' ').length; return this.count; }
  visitImage(img)   { this.count += 1; return this.count; } // count as 1 word
  visitTable(t) {
    t.data.flat().forEach(cell => this.count += String(cell).split(' ').length);
    return this.count;
  }
  getTotal() { return this.count; }
}

const doc = new Document()
  .add(new Heading('Design Patterns Guide', 1))
  .add(new Paragraph('Design patterns are reusable solutions to common problems.'))
  .add(new Heading('Creational Patterns', 2))
  .add(new Paragraph('These patterns deal with object creation.', 'intro'))
  .add(new Image('/patterns.png', 'Pattern Diagram', 800, 600))
  .add(new Table(2, 3, [['Pattern', 'Type', 'Purpose'],
                         ['Singleton', 'Creational', 'Single instance']]));

console.log('=== HTML ===');
console.log(doc.accept(new HTMLExportVisitor()).join('\n'));

console.log('\n=== Markdown ===');
console.log(doc.accept(new MarkdownExportVisitor()).join('\n'));

const wordCounter = new WordCountVisitor();
doc.accept(wordCounter);
console.log(`\nWord count: ${wordCounter.getTotal()}`);
```

---

## 27. Interpreter

### 🎯 Intent
Given a language, define a **grammar** and an **interpreter** for that language.

```javascript
// ─── Simple Expression Interpreter ───────────────
class NumberExpression {
  constructor(value) { this.value = value; }
  interpret(context) { return this.value; }
}

class VariableExpression {
  constructor(name) { this.name = name; }
  interpret(context) {
    if (!(this.name in context)) throw new Error(`Variable "${this.name}" not defined`);
    return context[this.name];
  }
}

class AddExpression {
  constructor(left, right) { this.left = left; this.right = right; }
  interpret(context) { return this.left.interpret(context) + this.right.interpret(context); }
}

class MultiplyExpression {
  constructor(left, right) { this.left = left; this.right = right; }
  interpret(context) { return this.left.interpret(context) * this.right.interpret(context); }
}

class GreaterThanExpression {
  constructor(left, right) { this.left = left; this.right = right; }
  interpret(context) { return this.left.interpret(context) > this.right.interpret(context); }
}

// Build expression: (x + 5) * y > 10
const expr = new GreaterThanExpression(
  new MultiplyExpression(
    new AddExpression(new VariableExpression('x'), new NumberExpression(5)),
    new VariableExpression('y')
  ),
  new NumberExpression(10)
);

console.log(expr.interpret({ x: 2, y: 2 })); // (2+5)*2 = 14 > 10 → true
console.log(expr.interpret({ x: 1, y: 1 })); // (1+5)*1 = 6 > 10 → false
```

---

# PART 5 — ARCHITECTURAL PATTERNS

---

## 28. MVC — Model View Controller

### 🎯 Intent
Separate application into three components: **Model** (data), **View** (UI), **Controller** (logic that connects them).

```javascript
// ─── MVC Todo App ─────────────────────────────────
// MODEL: Manages data and business logic
class TodoModel {
  #todos = [];
  #nextId = 1;
  #observers = [];

  addObserver(fn)    { this.#observers.push(fn); }
  #notify(event)     { this.#observers.forEach(fn => fn(event, this.#todos)); }

  addTodo(text) {
    const todo = { id: this.#nextId++, text, completed: false, createdAt: new Date() };
    this.#todos.push(todo);
    this.#notify('add');
    return todo;
  }

  toggleTodo(id) {
    const todo = this.#todos.find(t => t.id === id);
    if (todo) { todo.completed = !todo.completed; this.#notify('toggle'); }
  }

  deleteTodo(id) {
    this.#todos = this.#todos.filter(t => t.id !== id);
    this.#notify('delete');
  }

  getTodos(filter = 'all') {
    switch (filter) {
      case 'active':    return this.#todos.filter(t => !t.completed);
      case 'completed': return this.#todos.filter(t => t.completed);
      default:          return [...this.#todos];
    }
  }

  get stats() {
    const total     = this.#todos.length;
    const completed = this.#todos.filter(t => t.completed).length;
    return { total, completed, active: total - completed };
  }
}

// VIEW: Renders UI based on model data
class TodoView {
  render(todos, stats) {
    console.log(`\n═══ Todo List (${stats.active} active) ═══`);
    if (todos.length === 0) {
      console.log('  No todos yet!');
    } else {
      todos.forEach(todo => {
        const check = todo.completed ? '✅' : '⬜';
        console.log(`  [${todo.id}] ${check} ${todo.text}`);
      });
    }
    console.log(`  ─────────────────`);
    console.log(`  Total: ${stats.total} | Done: ${stats.completed} | Active: ${stats.active}`);
  }

  showError(message)   { console.error(`❌ Error: ${message}`); }
  showSuccess(message) { console.log(`✅ ${message}`); }
}

// CONTROLLER: Handles user input, updates model, tells view to re-render
class TodoController {
  constructor(model, view) {
    this.model = model;
    this.view  = view;

    // Observe model changes
    this.model.addObserver(() => this.#render());
  }

  addTodo(text) {
    if (!text || text.trim().length === 0) {
      this.view.showError('Todo text cannot be empty');
      return;
    }
    this.model.addTodo(text.trim());
    this.view.showSuccess(`Todo added: "${text}"`);
  }

  toggleTodo(id)  { this.model.toggleTodo(id); }
  deleteTodo(id)  { this.model.deleteTodo(id); }

  showAll()       { this.view.render(this.model.getTodos('all'),       this.model.stats); }
  showActive()    { this.view.render(this.model.getTodos('active'),    this.model.stats); }
  showCompleted() { this.view.render(this.model.getTodos('completed'), this.model.stats); }

  #render() {
    this.view.render(this.model.getTodos(), this.model.stats);
  }
}

// Usage
const model      = new TodoModel();
const view       = new TodoView();
const controller = new TodoController(model, view);

controller.addTodo('Learn Design Patterns');
controller.addTodo('Build a project');
controller.addTodo('Read documentation');
controller.toggleTodo(1);
controller.toggleTodo(2);
controller.showCompleted();
controller.deleteTodo(3);
```

---

## 29. MVP — Model View Presenter

MVP is like MVC but the **Presenter** knows about the View's interface, and the **View** is completely passive (no logic).

```javascript
// VIEW INTERFACE — the Presenter calls these
class IUserView {
  showUsers(users) { throw new Error('implement'); }
  showLoading()    { throw new Error('implement'); }
  hideLoading()    { throw new Error('implement'); }
  showError(msg)   { throw new Error('implement'); }
  showSuccess(msg) { throw new Error('implement'); }
}

// CONCRETE VIEW (can be swapped for testing)
class ConsoleUserView extends IUserView {
  showUsers(users) {
    console.log('Users:', users.map(u => u.name).join(', '));
  }
  showLoading()    { console.log('Loading...'); }
  hideLoading()    { console.log('Done.'); }
  showError(msg)   { console.error('ERROR:', msg); }
  showSuccess(msg) { console.log('SUCCESS:', msg); }
}

// Mock view for testing
class MockUserView extends IUserView {
  constructor() {
    super();
    this.calls = [];
  }
  showUsers(users) { this.calls.push({ method: 'showUsers', args: users }); }
  showLoading()    { this.calls.push({ method: 'showLoading' }); }
  hideLoading()    { this.calls.push({ method: 'hideLoading' }); }
  showError(msg)   { this.calls.push({ method: 'showError', args: msg }); }
  showSuccess(msg) { this.calls.push({ method: 'showSuccess', args: msg }); }
}

// MODEL
class UserModel {
  async getUsers() {
    return [{ id: 1, name: 'Alice' }, { id: 2, name: 'Bob' }];
  }
  async saveUser(user) {
    return { ...user, id: Date.now() };
  }
}

// PRESENTER — all the logic lives here
class UserPresenter {
  constructor(model, view) {
    this.model = model;
    this.view  = view;
  }

  async loadUsers() {
    this.view.showLoading();
    try {
      const users = await this.model.getUsers();
      this.view.showUsers(users);
    } catch (e) {
      this.view.showError(`Failed to load users: ${e.message}`);
    } finally {
      this.view.hideLoading();
    }
  }

  async saveUser(userData) {
    if (!userData.name) { this.view.showError('Name is required'); return; }
    try {
      await this.model.saveUser(userData);
      this.view.showSuccess(`User "${userData.name}" saved!`);
      await this.loadUsers();
    } catch (e) {
      this.view.showError(`Failed to save: ${e.message}`);
    }
  }
}

const presenter = new UserPresenter(new UserModel(), new ConsoleUserView());
await presenter.loadUsers();
await presenter.saveUser({ name: 'Carol' });
```

---

## 30. MVVM — Model View ViewModel

MVVM uses **data binding**: the ViewModel exposes data that the View binds to automatically.

```javascript
// ─── Simple Observable/Reactive System ────────────
class Observable {
  constructor(value) {
    this._value     = value;
    this._observers = [];
  }

  get value() { return this._value; }

  set value(newValue) {
    if (this._value !== newValue) {
      const old   = this._value;
      this._value = newValue;
      this._observers.forEach(fn => fn(newValue, old));
    }
  }

  subscribe(fn) {
    this._observers.push(fn);
    return () => { this._observers = this._observers.filter(o => o !== fn); };
  }
}

// MODEL
class ProductModel {
  async getProducts() {
    return [
      { id: 1, name: 'Laptop', price: 999, stock: 5 },
      { id: 2, name: 'Mouse',  price: 29,  stock: 50 },
    ];
  }
}

// VIEWMODEL — exposes observables that View binds to
class ProductViewModel {
  products    = new Observable([]);
  isLoading   = new Observable(false);
  error       = new Observable(null);
  searchQuery = new Observable('');
  sortBy      = new Observable('name');

  constructor(model) {
    this.model = model;
    // Auto-filter when search or sort changes
    this.searchQuery.subscribe(() => this.#filter());
    this.sortBy.subscribe(() => this.#filter());
  }

  #allProducts = [];

  async loadProducts() {
    this.isLoading.value = true;
    this.error.value     = null;
    try {
      this.#allProducts  = await this.model.getProducts();
      this.#filter();
    } catch (e) {
      this.error.value   = e.message;
    } finally {
      this.isLoading.value = false;
    }
  }

  #filter() {
    const query = this.searchQuery.value.toLowerCase();
    let result  = this.#allProducts.filter(p =>
      p.name.toLowerCase().includes(query)
    );
    result.sort((a, b) =>
      typeof a[this.sortBy.value] === 'string'
        ? a[this.sortBy.value].localeCompare(b[this.sortBy.value])
        : a[this.sortBy.value] - b[this.sortBy.value]
    );
    this.products.value = result;
  }

  search(query) { this.searchQuery.value = query; }
  sort(field)   { this.sortBy.value = field; }
}

// VIEW — binds to ViewModel observables
class ProductView {
  constructor(viewModel) {
    this.vm = viewModel;
    // Bind to observables
    viewModel.isLoading.subscribe(loading => {
      console.log(loading ? '⏳ Loading...' : '✅ Loaded');
    });
    viewModel.products.subscribe(products => {
      console.log(`\n📦 Products (${products.length}):`);
      products.forEach(p => console.log(`  ${p.name}: $${p.price} (${p.stock} in stock)`));
    });
    viewModel.error.subscribe(err => {
      if (err) console.error('❌ Error:', err);
    });
  }

  async init() { await this.vm.loadProducts(); }
  search(q)    { this.vm.search(q); }
  sort(field)  { this.vm.sort(field); }
}

const vm   = new ProductViewModel(new ProductModel());
const view = new ProductView(vm);
await view.init();
view.search('');
view.sort('price');
```

---

## 31. Repository Pattern

Abstract the data layer behind a **repository interface**. Business logic talks to the repository, not directly to the database.

```javascript
// ENTITY
class User {
  constructor({ id = null, name, email, createdAt = new Date() }) {
    this.id        = id;
    this.name      = name;
    this.email     = email;
    this.createdAt = createdAt;
  }
}

// REPOSITORY INTERFACE
class UserRepository {
  async findById(id)       { throw new Error('implement'); }
  async findAll()          { throw new Error('implement'); }
  async findByEmail(email) { throw new Error('implement'); }
  async save(user)         { throw new Error('implement'); }
  async update(id, data)   { throw new Error('implement'); }
  async delete(id)         { throw new Error('implement'); }
}

// IN-MEMORY REPOSITORY (great for testing)
class InMemoryUserRepository extends UserRepository {
  constructor() {
    super();
    this.users  = new Map();
    this.nextId = 1;
  }

  async findById(id) {
    return this.users.get(id) || null;
  }

  async findAll() {
    return [...this.users.values()];
  }

  async findByEmail(email) {
    return [...this.users.values()].find(u => u.email === email) || null;
  }

  async save(user) {
    const newUser = new User({ ...user, id: this.nextId++ });
    this.users.set(newUser.id, newUser);
    return newUser;
  }

  async update(id, data) {
    const user = await this.findById(id);
    if (!user) throw new Error(`User ${id} not found`);
    Object.assign(user, data);
    return user;
  }

  async delete(id) {
    const existed = this.users.has(id);
    this.users.delete(id);
    return existed;
  }
}

// SQL REPOSITORY (production)
class SQLUserRepository extends UserRepository {
  constructor(db) { super(); this.db = db; }

  async findById(id) {
    const row = await this.db.query('SELECT * FROM users WHERE id = ?', [id]);
    return row ? new User(row) : null;
  }

  async save(user) {
    const result = await this.db.query(
      'INSERT INTO users (name, email) VALUES (?, ?)',
      [user.name, user.email]
    );
    return new User({ ...user, id: result.insertId });
  }

  // ... other methods
}

// BUSINESS LOGIC — only knows about the repository interface
class UserService {
  constructor(userRepository) {
    this.repo = userRepository;
  }

  async createUser(name, email) {
    const existing = await this.repo.findByEmail(email);
    if (existing) throw new Error(`Email "${email}" already registered`);
    return this.repo.save(new User({ name, email }));
  }

  async getUserById(id) {
    const user = await this.repo.findById(id);
    if (!user) throw new Error(`User ${id} not found`);
    return user;
  }

  async getAllUsers() {
    return this.repo.findAll();
  }
}

// Production: use SQL repo
// const service = new UserService(new SQLUserRepository(db));

// Testing: use in-memory repo (no DB needed!)
const service = new UserService(new InMemoryUserRepository());
const alice   = await service.createUser('Alice', 'alice@example.com');
const bob     = await service.createUser('Bob', 'bob@example.com');
console.log(await service.getAllUsers());
```

---

## 32. Service Layer Pattern

Defines an application's boundary with a set of available operations. Coordinates responses across various repositories and domain objects.

```javascript
class OrderService {
  constructor({ orderRepository, productRepository, userRepository,
                paymentService, emailService, inventoryService }) {
    this.orderRepo     = orderRepository;
    this.productRepo   = productRepository;
    this.userRepo      = userRepository;
    this.paymentSvc    = paymentService;
    this.emailSvc      = emailService;
    this.inventorySvc  = inventoryService;
  }

  async createOrder({ userId, items, paymentDetails }) {
    // Orchestrate the operation across multiple services
    const user    = await this.userRepo.findById(userId);
    if (!user) throw new Error('User not found');

    const products = await Promise.all(
      items.map(item => this.productRepo.findById(item.productId))
    );

    const orderItems = items.map((item, i) => ({
      product:  products[i],
      quantity: item.quantity,
      price:    products[i].price * item.quantity,
    }));

    const total = orderItems.reduce((sum, i) => sum + i.price, 0);

    const payment = await this.paymentSvc.charge(total, paymentDetails);
    if (!payment.success) throw new Error('Payment failed');

    const order = await this.orderRepo.save({
      userId, items: orderItems, total,
      status: 'confirmed', paymentId: payment.transactionId,
    });

    await this.inventorySvc.reserve(items);
    await this.emailSvc.sendOrderConfirmation(user.email, order);

    return order;
  }
}
```

---

## 33. Event Sourcing

Instead of storing current state, store a **sequence of events** that led to that state. State is derived by replaying events.

```javascript
class EventStore {
  #events = [];

  append(aggregateId, event) {
    const stored = {
      id:          crypto.randomUUID(),
      aggregateId,
      type:        event.type,
      data:        event.data,
      timestamp:   new Date().toISOString(),
      version:     this.#events.filter(e => e.aggregateId === aggregateId).length + 1,
    };
    this.#events.push(stored);
    return stored;
  }

  getEvents(aggregateId) {
    return this.#events.filter(e => e.aggregateId === aggregateId);
  }

  getAllEvents() { return [...this.#events]; }
}

class BankAccount {
  constructor(id) {
    this.id      = id;
    this.balance = 0;
    this.owner   = null;
    this.version = 0;
    this.isOpen  = false;
  }

  // Rebuild state by replaying events
  static fromEvents(events) {
    const account = new BankAccount(events[0]?.aggregateId);
    events.forEach(event => account.apply(event));
    return account;
  }

  apply(event) {
    switch (event.type) {
      case 'AccountOpened':
        this.owner  = event.data.owner;
        this.isOpen = true;
        this.balance = 0;
        break;
      case 'MoneyDeposited':
        this.balance += event.data.amount;
        break;
      case 'MoneyWithdrawn':
        this.balance -= event.data.amount;
        break;
      case 'AccountClosed':
        this.isOpen = false;
        break;
    }
    this.version = event.version;
  }
}

// Usage
const store = new EventStore();
const id    = 'account-1';

store.append(id, { type: 'AccountOpened',  data: { owner: 'Alice' } });
store.append(id, { type: 'MoneyDeposited', data: { amount: 1000 } });
store.append(id, { type: 'MoneyDeposited', data: { amount: 500 } });
store.append(id, { type: 'MoneyWithdrawn', data: { amount: 200 } });

// Reconstruct current state from events
const events  = store.getEvents(id);
const account = BankAccount.fromEvents(events);
console.log(`Balance: $${account.balance}`);  // $1300
console.log(`Owner: ${account.owner}`);         // Alice

// Time travel: state at any point in history!
const afterFirstDeposit = BankAccount.fromEvents(events.slice(0, 2));
console.log(`Balance after 1st deposit: $${afterFirstDeposit.balance}`); // $1000
```

---

## 34. CQRS — Command Query Responsibility Segregation

Separate **read** operations (Queries) from **write** operations (Commands). They can use different models, databases, even services.

```javascript
// ─── WRITE SIDE (Commands) ────────────────────────
class CreateUserCommand {
  constructor(name, email, role = 'user') {
    this.name  = name;
    this.email = email;
    this.role  = role;
  }
}

class UpdateUserCommand {
  constructor(id, updates) {
    this.id      = id;
    this.updates = updates;
  }
}

class CommandHandler {
  constructor(writeDb, eventBus) {
    this.db       = writeDb;
    this.eventBus = eventBus;
  }

  async handle(command) {
    if (command instanceof CreateUserCommand) {
      const user = await this.db.users.insert({
        name: command.name, email: command.email, role: command.role
      });
      this.eventBus.publish('user:created', user); // Notify read side to update
      return user;
    }
    if (command instanceof UpdateUserCommand) {
      const user = await this.db.users.update(command.id, command.updates);
      this.eventBus.publish('user:updated', user);
      return user;
    }
    throw new Error(`Unknown command: ${command.constructor.name}`);
  }
}

// ─── READ SIDE (Queries) ──────────────────────────
// Optimized read model (denormalized, cached, etc.)
class UserReadModel {
  #cache = new Map();

  updateFromEvent(event, data) {
    if (event === 'user:created' || event === 'user:updated') {
      this.#cache.set(data.id, {
        id:         data.id,
        name:       data.name,
        email:      data.email,
        role:       data.role,
        displayName: `${data.name} <${data.email}>`, // Denormalized!
      });
    }
  }

  // Read operations are simple and fast
  getUserById(id)    { return this.#cache.get(id) || null; }
  getAllUsers()      { return [...this.#cache.values()]; }
  getUsersByRole(r)  { return this.getAllUsers().filter(u => u.role === r); }
}

// Wire them together
const bus        = new EventBus();
const readModel  = new UserReadModel();
const cmdHandler = new CommandHandler(/* writeDb */, bus);

// Read model subscribes to events from write side
bus.subscribe('user:created', data => readModel.updateFromEvent('user:created', data));
bus.subscribe('user:updated', data => readModel.updateFromEvent('user:updated', data));

// Write: sends a command
await cmdHandler.handle(new CreateUserCommand('Alice', 'alice@example.com', 'admin'));

// Read: queries the optimized read model
const users = readModel.getAllUsers();   // Fast! From optimized read model
```

---

# PART 6 — ADVANCED & MODERN PATTERNS

---

## 35. Dependency Injection

Provide dependencies to an object **from the outside** rather than creating them inside.

```javascript
// ─── DI Container ─────────────────────────────────
class Container {
  #bindings   = new Map();
  #singletons = new Map();

  // Register a factory function
  bind(name, factory) {
    this.#bindings.set(name, { factory, singleton: false });
    return this;
  }

  // Register as singleton — created once, reused
  singleton(name, factory) {
    this.#bindings.set(name, { factory, singleton: true });
    return this;
  }

  // Resolve a dependency
  make(name) {
    const binding = this.#bindings.get(name);
    if (!binding) throw new Error(`No binding found for: "${name}"`);

    if (binding.singleton) {
      if (!this.#singletons.has(name)) {
        this.#singletons.set(name, binding.factory(this));
      }
      return this.#singletons.get(name);
    }

    return binding.factory(this);
  }
}

// Services
class Logger {
  log(msg) { console.log(`[LOG] ${new Date().toISOString()}: ${msg}`); }
}

class UserRepository {
  constructor(db) { this.db = db; }
  async findAll() { return this.db.query('SELECT * FROM users'); }
}

class UserService {
  constructor(userRepository, logger) {
    this.repo   = userRepository;
    this.logger = logger;
  }

  async getAllUsers() {
    this.logger.log('Fetching all users...');
    return this.repo.findAll();
  }
}

// Configure the container
const container = new Container();

container
  .singleton('logger',     () => new Logger())
  .singleton('db',         () => ({ query: sql => console.log('DB:', sql) || [] }))
  .bind('userRepository',  c  => new UserRepository(c.make('db')))
  .bind('userService',     c  => new UserService(c.make('userRepository'), c.make('logger')));

// Resolve with all dependencies automatically wired
const service = container.make('userService');
service.getAllUsers();
```

---

## 36. Decorator vs HOC (React Pattern)

```javascript
// ─── Higher-Order Component Pattern ───────────────
// HOC: a function that takes a component and returns an enhanced component

// withLoading HOC
function withLoading(Component) {
  return function LoadingWrapper({ isLoading, ...props }) {
    if (isLoading) return '<div class="spinner">Loading...</div>';
    return Component(props);
  };
}

// withAuth HOC
function withAuth(Component) {
  return function AuthWrapper({ user, ...props }) {
    if (!user) return '<div>Please log in</div>';
    return Component({ user, ...props });
  };
}

// withError HOC
function withError(Component) {
  return function ErrorWrapper({ error, ...props }) {
    if (error) return `<div class="error">${error}</div>`;
    return Component(props);
  };
}

// Base component
function UserList({ users }) {
  return `<ul>${users.map(u => `<li>${u.name}</li>`).join('')}</ul>`;
}

// Compose HOCs — wrap outside-in
const EnhancedUserList = withError(withAuth(withLoading(UserList)));

console.log(EnhancedUserList({ isLoading: true }));
console.log(EnhancedUserList({ isLoading: false, user: null }));
console.log(EnhancedUserList({
  isLoading: false,
  user: { name: 'Admin' },
  error: null,
  users: [{ name: 'Alice' }, { name: 'Bob' }]
}));
```

---

## 37. Module Pattern

Encapsulate private state and expose a public API.

```javascript
// ─── Classic Module Pattern (IIFE) ────────────────
const ShoppingCart = (() => {
  // Private state
  let items       = [];
  let discount    = 0;
  const TAX_RATE  = 0.1;

  // Private functions
  function calculateSubtotal() {
    return items.reduce((sum, item) => sum + item.price * item.qty, 0);
  }

  function applyDiscount(subtotal) {
    return subtotal * (1 - discount / 100);
  }

  // Public API
  return {
    addItem(name, price, qty = 1) {
      const existing = items.find(i => i.name === name);
      if (existing) existing.qty += qty;
      else items.push({ name, price, qty });
      return this;
    },

    removeItem(name) {
      items = items.filter(i => i.name !== name);
      return this;
    },

    setDiscount(pct) {
      if (pct < 0 || pct > 100) throw new Error('Invalid discount');
      discount = pct;
      return this;
    },

    getTotal() {
      const subtotal    = calculateSubtotal();
      const discounted  = applyDiscount(subtotal);
      const tax         = discounted * TAX_RATE;
      return { subtotal, discount, discounted, tax, total: discounted + tax };
    },

    getItems() { return [...items]; },

    clear() {
      items    = [];
      discount = 0;
      return this;
    },
  };
})();

ShoppingCart.addItem('Laptop', 999, 1).addItem('Mouse', 29, 2).setDiscount(10);
console.log(ShoppingCart.getTotal());
```

---

## 38. Mixin Pattern

Add methods from multiple sources to a class **without inheritance**.

```javascript
// ─── Mixins ───────────────────────────────────────
const Serializable = (Base) => class extends Base {
  serialize()             { return JSON.stringify(this); }
  static deserialize(json) { return Object.assign(new this(), JSON.parse(json)); }
};

const Timestamped = (Base) => class extends Base {
  constructor(...args) {
    super(...args);
    this.createdAt = new Date().toISOString();
    this.updatedAt = new Date().toISOString();
  }
  touch() { this.updatedAt = new Date().toISOString(); }
};

const Validatable = (Base) => class extends Base {
  validate() {
    const errors = [];
    Object.entries(this.constructor.validations || {}).forEach(([field, rules]) => {
      const value = this[field];
      if (rules.required && !value) errors.push(`${field} is required`);
      if (rules.minLength && value?.length < rules.minLength)
        errors.push(`${field} must be at least ${rules.minLength} chars`);
      if (rules.pattern && !rules.pattern.test(value))
        errors.push(`${field} format is invalid`);
    });
    return { valid: errors.length === 0, errors };
  }
};

const Eventable = (Base) => class extends Base {
  constructor(...args) {
    super(...args);
    this._handlers = {};
  }
  on(event, fn)  { (this._handlers[event] = this._handlers[event] || []).push(fn); }
  emit(event, d) { (this._handlers[event] || []).forEach(fn => fn(d)); }
};

// Compose a class from multiple mixins
class User extends Serializable(Timestamped(Validatable(Eventable(class {})))) {
  static validations = {
    name:  { required: true, minLength: 2 },
    email: { required: true, pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/ },
  };

  constructor(name, email) {
    super();
    this.name  = name;
    this.email = email;
  }
}

const user = new User('Alice', 'alice@example.com');
user.on('save', (u) => console.log('Saved:', u.name));

const { valid, errors } = user.validate();
console.log('Valid:', valid);   // true

const json    = user.serialize();
console.log(json);

const restored = User.deserialize(json);
console.log(restored instanceof User); // true
```

---

## 39. Null Object Pattern

Provide a default object that does nothing — eliminates null checks.

```javascript
// ─── Without Null Object ──────────────────────────
function processUser(user) {
  if (user && user.canView()) {        // Null checks everywhere!
    user.log('viewed');
    return user.getName();
  }
  return 'Guest';
}

// ─── With Null Object ─────────────────────────────
class User {
  constructor(name, permissions) {
    this.name        = name;
    this.permissions = permissions;
  }
  getName()         { return this.name; }
  canView()         { return this.permissions.includes('view'); }
  canEdit()         { return this.permissions.includes('edit'); }
  log(action)       { console.log(`[AUDIT] ${this.name} performed: ${action}`); }
  isNull()          { return false; }
}

// Null Object — has the same interface, does nothing
class NullUser {
  getName()         { return 'Guest'; }
  canView()         { return false; }
  canEdit()         { return false; }
  log(action)       { /* do nothing — guests aren't logged */ }
  isNull()          { return true; }
}

function getCurrentUser(session) {
  return session?.user ? new User(session.user.name, session.user.permissions)
                       : new NullUser();
}

// No null checks needed!
function processUser(session) {
  const user = getCurrentUser(session);
  if (user.canView()) {
    user.log('viewed');
    return `Welcome, ${user.getName()}!`;
  }
  return `Hello, ${user.getName()}. Please log in.`;
}

console.log(processUser(null));                    // Guest flow, no crash
console.log(processUser({ user: { name: 'Alice', permissions: ['view', 'edit'] } }));
```

---

## 40. Specification Pattern

Encapsulate business rules as **composable specification objects**.

```javascript
class Specification {
  isSatisfiedBy(item) { throw new Error('implement'); }
  and(other)  { return new AndSpecification(this, other); }
  or(other)   { return new OrSpecification(this, other); }
  not()       { return new NotSpecification(this); }
}

class AndSpecification extends Specification {
  constructor(a, b) { super(); this.a = a; this.b = b; }
  isSatisfiedBy(item) { return this.a.isSatisfiedBy(item) && this.b.isSatisfiedBy(item); }
}

class OrSpecification extends Specification {
  constructor(a, b) { super(); this.a = a; this.b = b; }
  isSatisfiedBy(item) { return this.a.isSatisfiedBy(item) || this.b.isSatisfiedBy(item); }
}

class NotSpecification extends Specification {
  constructor(spec) { super(); this.spec = spec; }
  isSatisfiedBy(item) { return !this.spec.isSatisfiedBy(item); }
}

// Concrete specifications
class PriceSpec extends Specification {
  constructor(min, max) { super(); this.min = min; this.max = max; }
  isSatisfiedBy(p) { return p.price >= this.min && p.price <= this.max; }
}

class InStockSpec extends Specification {
  isSatisfiedBy(p) { return p.stock > 0; }
}

class CategorySpec extends Specification {
  constructor(cat) { super(); this.cat = cat; }
  isSatisfiedBy(p) { return p.category === this.cat; }
}

class RatingSpec extends Specification {
  constructor(min) { super(); this.min = min; }
  isSatisfiedBy(p) { return p.rating >= this.min; }
}

// Usage — compose complex queries from simple rules
const affordable = new PriceSpec(0, 100);
const inStock    = new InStockSpec();
const electronics = new CategorySpec('electronics');
const wellRated  = new RatingSpec(4.0);

// "Electronics under $100, in stock, rated 4+"
const query = affordable.and(inStock).and(electronics).and(wellRated);

const products = [
  { name: 'Mouse',   price: 29,   stock: 10, category: 'electronics', rating: 4.5 },
  { name: 'Laptop',  price: 999,  stock: 5,  category: 'electronics', rating: 4.8 },
  { name: 'Pen',     price: 2,    stock: 0,  category: 'stationery',  rating: 4.0 },
  { name: 'Headset', price: 79,   stock: 3,  category: 'electronics', rating: 3.5 },
  { name: 'Webcam',  price: 69,   stock: 8,  category: 'electronics', rating: 4.2 },
];

const results = products.filter(p => query.isSatisfiedBy(p));
console.log(results.map(p => p.name)); // ['Mouse', 'Webcam']
```

---

## 41. Pipeline Pattern

Process data through a **series of transformations**, each step passing its output to the next.

```javascript
class Pipeline {
  #stages = [];
  #errorHandler = null;

  pipe(fn) {
    this.#stages.push(fn);
    return this;
  }

  catch(fn) {
    this.#errorHandler = fn;
    return this;
  }

  async process(input) {
    let result = input;
    for (const stage of this.#stages) {
      try {
        result = await stage(result);
      } catch (error) {
        if (this.#errorHandler) return this.#errorHandler(error, result);
        throw error;
      }
    }
    return result;
  }
}

// ─── Data Processing Pipeline ─────────────────────
const userPipeline = new Pipeline()
  .pipe(input => {
    console.log('[1] Parsing input');
    return typeof input === 'string' ? JSON.parse(input) : input;
  })
  .pipe(data => {
    console.log('[2] Validating');
    if (!data.email?.includes('@')) throw new Error('Invalid email');
    if (!data.name || data.name.length < 2) throw new Error('Name too short');
    return data;
  })
  .pipe(data => {
    console.log('[3] Normalizing');
    return {
      name:      data.name.trim().toLowerCase(),
      email:     data.email.trim().toLowerCase(),
      createdAt: new Date().toISOString(),
    };
  })
  .pipe(async data => {
    console.log('[4] Enriching');
    // Could make API call here
    return { ...data, id: Math.floor(Math.random() * 1000), role: 'user' };
  })
  .pipe(data => {
    console.log('[5] Saving');
    // Save to database
    return { success: true, user: data };
  })
  .catch((error, data) => {
    console.error(`Pipeline failed: ${error.message}`);
    return { success: false, error: error.message, data };
  });

const result = await userPipeline.process('{"name":"Alice","email":"alice@example.com"}');
console.log('Result:', result);
```

---

## 42. Saga Pattern

Manage complex, multi-step processes with **compensating transactions** for each step (rollback on failure).

```javascript
class Saga {
  #steps  = [];
  #completed = [];

  step(name, execute, compensate) {
    this.#steps.push({ name, execute, compensate });
    return this;
  }

  async run(context) {
    console.log('Starting saga...');
    for (const step of this.#steps) {
      try {
        console.log(`  ▶ ${step.name}`);
        context = await step.execute(context);
        this.#completed.push(step);
      } catch (error) {
        console.error(`  ✗ ${step.name} failed: ${error.message}`);
        await this.#compensate(context);
        throw error;
      }
    }
    console.log('Saga completed successfully!');
    return context;
  }

  async #compensate(context) {
    console.log('Rolling back...');
    for (const step of [...this.#completed].reverse()) {
      try {
        console.log(`  ↩ Compensating: ${step.name}`);
        await step.compensate(context);
      } catch (e) {
        console.error(`  ! Compensation failed for ${step.name}: ${e.message}`);
      }
    }
  }
}

// Order placement saga
const orderSaga = new Saga()
  .step(
    'Reserve Inventory',
    async ctx => {
      console.log('    Reserving items...');
      ctx.reservationId = 'RES-001';
      return ctx;
    },
    async ctx => {
      console.log('    Releasing reservation:', ctx.reservationId);
    }
  )
  .step(
    'Charge Payment',
    async ctx => {
      console.log('    Charging $' + ctx.total);
      // Simulate payment failure
      if (ctx.simulatePaymentFailure) throw new Error('Card declined');
      ctx.paymentId = 'PAY-001';
      return ctx;
    },
    async ctx => {
      console.log('    Refunding payment:', ctx.paymentId);
    }
  )
  .step(
    'Create Shipment',
    async ctx => {
      console.log('    Creating shipment...');
      ctx.shipmentId = 'SHIP-001';
      return ctx;
    },
    async ctx => {
      console.log('    Cancelling shipment:', ctx.shipmentId);
    }
  );

// Success case
await orderSaga.run({ total: 99.99, userId: 1 });

// Failure case — rolls back all completed steps
try {
  await orderSaga.run({ total: 199.99, userId: 2, simulatePaymentFailure: true });
} catch (e) {
  console.log('Order failed, all steps rolled back');
}
```

---

## 43. Circuit Breaker Pattern

Prevent cascading failures by **stopping calls** to a failing service after too many failures.

```javascript
const STATE = { CLOSED: 'CLOSED', OPEN: 'OPEN', HALF_OPEN: 'HALF_OPEN' };

class CircuitBreaker {
  #state         = STATE.CLOSED;
  #failures      = 0;
  #successes     = 0;
  #lastFailTime  = null;
  #options;

  constructor(options = {}) {
    this.#options = {
      failureThreshold:  5,      // Open after 5 failures
      successThreshold:  2,      // Close after 2 successes in half-open
      timeout:           60000,  // Stay open for 60 seconds
      ...options,
    };
  }

  async call(fn) {
    if (this.#state === STATE.OPEN) {
      if (Date.now() - this.#lastFailTime > this.#options.timeout) {
        this.#state = STATE.HALF_OPEN;
        console.log('[Circuit Breaker] Half-open — testing service...');
      } else {
        throw new Error('Circuit is OPEN — service unavailable. Try again later.');
      }
    }

    try {
      const result = await fn();
      this.#onSuccess();
      return result;
    } catch (error) {
      this.#onFailure();
      throw error;
    }
  }

  #onSuccess() {
    this.#failures = 0;
    if (this.#state === STATE.HALF_OPEN) {
      this.#successes++;
      if (this.#successes >= this.#options.successThreshold) {
        this.#state    = STATE.CLOSED;
        this.#successes = 0;
        console.log('[Circuit Breaker] Closed — service recovered!');
      }
    }
  }

  #onFailure() {
    this.#failures++;
    this.#lastFailTime = Date.now();
    if (this.#failures >= this.#options.failureThreshold) {
      this.#state = STATE.OPEN;
      console.log(`[Circuit Breaker] OPEN — too many failures (${this.#failures})`);
    }
  }

  get state() { return this.#state; }
  get failures() { return this.#failures; }
}

// Usage
const breaker = new CircuitBreaker({ failureThreshold: 3, timeout: 5000 });
let failCount = 0;

const unreliableService = async () => {
  failCount++;
  if (failCount <= 4) throw new Error('Service unavailable');
  return { data: 'success' };
};

for (let i = 0; i < 7; i++) {
  try {
    const result = await breaker.call(unreliableService);
    console.log(`Success: ${JSON.stringify(result)} [State: ${breaker.state}]`);
  } catch (e) {
    console.log(`Error: ${e.message} [State: ${breaker.state}]`);
  }
  await new Promise(r => setTimeout(r, 100));
}
```

---

## 44. Anti-Patterns to Avoid

```javascript
// ╔═══════════════════════════════════════════════╗
// ║  1. God Object — knows/does too much          ║
// ╚═══════════════════════════════════════════════╝
// ❌ Don't
class Application {
  handleUserLogin()       {}
  validatePaymentCard()   {}
  sendEmailNotification() {}
  renderDashboard()       {}
  calculateTaxes()        {}
  writeToDatabase()       {}
  // 50 more methods...
}
// ✅ Do: Split into UserService, PaymentService, EmailService, etc.

// ╔═══════════════════════════════════════════════╗
// ║  2. Spaghetti Code — tangled control flow     ║
// ╚═══════════════════════════════════════════════╝
// ❌ Don't: deeply nested, hard to follow
async function process(req) {
  if (req.user) {
    if (req.user.isActive) {
      if (req.user.hasPermission('order')) {
        if (req.body.items && req.body.items.length > 0) {
          // actual logic buried 5 levels deep!
        }
      }
    }
  }
}
// ✅ Do: early returns / guard clauses
async function process(req) {
  if (!req.user)                             return { error: 401 };
  if (!req.user.isActive)                    return { error: 403, msg: 'Account inactive' };
  if (!req.user.hasPermission('order'))      return { error: 403, msg: 'No permission' };
  if (!req.body.items?.length)               return { error: 400, msg: 'No items' };
  // Happy path is now clean and obvious
  return processOrder(req.user, req.body.items);
}

// ╔═══════════════════════════════════════════════╗
// ║  3. Golden Hammer — use one pattern everywhere║
// ╚═══════════════════════════════════════════════╝
// ❌ Don't: "I just learned Singleton, everything is a Singleton!"
class UserService { /* Singleton */ }
class PaymentService { /* Singleton */ }
class CartService { /* Singleton */ }
class ProductCatalog { /* Singleton */ } // WHY?

// ╔═══════════════════════════════════════════════╗
// ║  4. Magic Numbers and Strings                 ║
// ╚═══════════════════════════════════════════════╝
// ❌ Don't
if (user.role === 2) { /* what is 2?? */ }
setTimeout(fn, 86400000); // what is this number?

// ✅ Do
const ROLES = { ADMIN: 2, USER: 1, VIEWER: 0 };
const ONE_DAY_MS = 24 * 60 * 60 * 1000;
if (user.role === ROLES.ADMIN) { }
setTimeout(fn, ONE_DAY_MS);

// ╔═══════════════════════════════════════════════╗
// ║  5. Premature Optimization               ║
// ╚═══════════════════════════════════════════════╝
// ❌ Don't optimize before measuring
// "Maybe I should use a custom hash map for better performance"
// (when you have 20 items — just use an array!)

// ╔═══════════════════════════════════════════════╗
// ║  6. Shotgun Surgery                           ║
// ╚═══════════════════════════════════════════════╝
// When you make one change that requires modifying many unrelated classes
// Fix: use better encapsulation, cohesion, and the right patterns

// ╔═══════════════════════════════════════════════╗
// ║  7. Callback Hell                             ║
// ╚═══════════════════════════════════════════════╝
// ❌ Don't
getUser(id, function(user) {
  getOrders(user.id, function(orders) {
    getOrderDetails(orders[0].id, function(details) {
      sendEmail(user.email, details, function(result) {
        // pyramid of doom!
      });
    });
  });
});

// ✅ Do: async/await
const user    = await getUser(id);
const orders  = await getOrders(user.id);
const details = await getOrderDetails(orders[0].id);
const result  = await sendEmail(user.email, details);
```

---

## 45. Pattern Selection Cheat Sheet

### When You Need…

```
┌─────────────────────────────────────────────────────────────────┐
│  PROBLEM                          → PATTERN                      │
├─────────────────────────────────────────────────────────────────┤
│  Only ONE instance of a class     → Singleton                    │
│  Create objects without specifying│                              │
│    exact class                    → Factory Method               │
│  Create families of related objs  → Abstract Factory             │
│  Complex object construction      → Builder                      │
│  Clone objects cheaply            → Prototype                    │
│  Reuse expensive objects          → Object Pool                  │
├─────────────────────────────────────────────────────────────────┤
│  Incompatible interfaces          → Adapter                      │
│  Decouple abstraction/impl        → Bridge                       │
│  Tree structures (part-whole)     → Composite                    │
│  Add behavior dynamically         → Decorator                    │
│  Simplify complex subsystem       → Facade                       │
│  Share fine-grained objects       → Flyweight                    │
│  Control access to an object      → Proxy                        │
├─────────────────────────────────────────────────────────────────┤
│  Pass requests along handlers     → Chain of Responsibility      │
│  Encapsulate actions as objects   → Command                      │
│  Sequential access to collection  → Iterator                     │
│  Reduce tight coupling            → Mediator                     │
│  Save/restore object state        → Memento                      │
│  Notify multiple objects          → Observer                     │
│  Change behavior per state        → State                        │
│  Swap algorithms at runtime       → Strategy                     │
│  Define algorithm skeleton        → Template Method              │
│  Add operations without modifying → Visitor                      │
├─────────────────────────────────────────────────────────────────┤
│  Separate UI concerns             → MVC / MVP / MVVM             │
│  Abstract data access             → Repository                   │
│  Separate read/write ops          → CQRS                         │
│  Store history of changes         → Event Sourcing               │
│  Manage dependencies              → Dependency Injection         │
│  Multi-step with rollback         → Saga                         │
│  Prevent cascading failures       → Circuit Breaker              │
│  Composable business rules        → Specification                │
│  Sequential data transformation   → Pipeline                     │
└─────────────────────────────────────────────────────────────────┘
```

### Pattern Relationships

```
Singleton ──uses──► Factory Method
Builder ──────────► can use Composite for result
Decorator ────────► same interface as Composite
Strategy ─────────► uses Template Method internally
Observer ─────────► used in Mediator, MVC, Event Sourcing
Proxy ────────────► similar to Decorator (but different intent)
Facade ───────────► hides multiple objects (like Mediator)
Chain of Resp ────► like Decorator but passes or stops
Command ──────────► used in CQRS, Event Sourcing
```

### Complexity vs Benefit Matrix

```
               Low Complexity   High Complexity
               ──────────────   ───────────────
High Benefit:  Observer          Visitor
               Strategy          Event Sourcing
               Factory           CQRS
               Builder           Saga

Low Benefit:   Prototype         God Object (anti-pattern!)
               Object Pool       
               Null Object       
```

### Quick Decision Tree

```
Need to create an object?
  └─ Complex construction?     → Builder
  └─ Family of objects?        → Abstract Factory
  └─ Which class unknown?      → Factory Method
  └─ Clone existing?           → Prototype
  └─ Single instance?          → Singleton

Need to compose objects?
  └─ Incompatible interface?   → Adapter
  └─ Add behavior dynamically? → Decorator
  └─ Simplify complex API?     → Facade
  └─ Tree structure?           → Composite
  └─ Control access?           → Proxy

Need to coordinate behavior?
  └─ Multiple handlers?        → Chain of Responsibility
  └─ Undo/redo?                → Command + Memento
  └─ Decouple dependencies?    → Observer / Mediator
  └─ Change per state?         → State
  └─ Swap algorithms?          → Strategy
  └─ Notify dependents?        → Observer
```

---

## Summary Table

| Pattern | Category | Complexity | Common In |
|---------|----------|------------|-----------|
| Singleton | Creational | ⭐ | Config, Logger, DB connection |
| Factory Method | Creational | ⭐⭐ | Frameworks, plugin systems |
| Abstract Factory | Creational | ⭐⭐⭐ | UI toolkits, cross-platform |
| Builder | Creational | ⭐⭐ | Query builders, HTTP clients |
| Prototype | Creational | ⭐⭐ | Game objects, config cloning |
| Adapter | Structural | ⭐⭐ | Third-party integrations |
| Decorator | Structural | ⭐⭐ | Middleware, HOCs, caching |
| Facade | Structural | ⭐ | APIs, SDK wrappers |
| Composite | Structural | ⭐⭐ | UI trees, file systems |
| Proxy | Structural | ⭐⭐ | Caching, auth, lazy loading |
| Observer | Behavioral | ⭐⭐ | Event systems, React, Vue |
| Strategy | Behavioral | ⭐ | Sorting, payment, validation |
| Command | Behavioral | ⭐⭐ | Undo/redo, queues, CQRS |
| State | Behavioral | ⭐⭐ | UI state machines, games |
| Chain of Resp | Behavioral | ⭐⭐ | Middleware, request pipelines |
| Template Method | Behavioral | ⭐ | Frameworks, data processors |
| Iterator | Behavioral | ⭐ | Collections, streams |
| Mediator | Behavioral | ⭐⭐⭐ | Chat, event buses, MVC |
| Memento | Behavioral | ⭐⭐ | Editors, game saves |
| Visitor | Behavioral | ⭐⭐⭐ | Compilers, export systems |
| MVC/MVP/MVVM | Architectural | ⭐⭐⭐ | Every web/app framework |
| Repository | Architectural | ⭐⭐ | Data access layers |
| CQRS | Architectural | ⭐⭐⭐ | High-scale applications |
| Event Sourcing | Architectural | ⭐⭐⭐⭐ | Audit logs, banking |
| Circuit Breaker | Advanced | ⭐⭐⭐ | Microservices |
| Saga | Advanced | ⭐⭐⭐ | Distributed transactions |

---

*Design Patterns Complete Reference Guide — Beginner to Super Advanced*

*"Each pattern describes a problem which occurs over and over again in our environment, and then describes the core of the solution to that problem."*
*— Christopher Alexander*

*Master the patterns. Recognize the problems. Write code that lasts. 🏗️*
