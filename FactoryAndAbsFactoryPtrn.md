## 📝 Factory Pattern 

The **Factory Design Pattern** is a *creational design pattern* used to centralize object creation logic. Instead of creating objects directly using `new`, the responsibility of creating objects is delegated to a factory class.

This helps in **decoupling client code from concrete implementations**, making the system more flexible, maintainable, and scalable.

The .net core dependecy injection use factory-like behavior internally. DI itself is not a factory pattern—it is a mechanism to achieve inversion of control.”
---

## 🔷 Key Idea

> “Encapsulate object creation and return objects through a common interface instead of exposing concrete classes.”

---

## 🔷 When to Use

* When object creation is complex
* When multiple implementations exist for a single interface
* When you want to avoid `if-else` or `switch` scattered in code
* When future extensibility is required
---

## 🔷 Real-Time Example: Notification System

We want to send notifications via:

* Email
* SMS
* Push Notification

Instead of creating objects everywhere, we centralize creation using a Factory.
---
## 🔷 Step 1: Common Interface (Product)

```csharp id="notif1"
public interface INotification
{
    void Send(string message);
}
```
---
## 🔷 Step 2: Concrete Implementations

```csharp id="notif2"
public class EmailNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine("Email sent: " + message);
    }
}

public class SmsNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine("SMS sent: " + message);
    }
}

public class PushNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine("Push Notification sent: " + message);
    }
}
```
---

## 🔷 Step 3: Factory Class

```csharp id="notif3"
public class NotificationFactory
{
    public static INotification Create(string type)
    {
        switch (type.ToLower())
        {
            case "email":
                return new EmailNotification();

            case "sms":
                return new SmsNotification();

            case "push":
                return new PushNotification();

            default:
                throw new ArgumentException("Invalid notification type");
        }
    }
}
```
---

## 🔷 Step 4: Usage (Client Code)

```csharp id="notif4"
class Program
{
    static void Main()
    {
        INotification notification;

        notification = NotificationFactory.Create("email");
        notification.Send("Welcome to our system!");

        notification = NotificationFactory.Create("sms");
        notification.Send("Your OTP is 1234");
    }
}
```

---

## 🔷 Output

```
Email sent: Welcome to our system!
SMS sent: Your OTP is 1234
```

---

## 🔷 Advantages

✔ Centralized object creation
✔ Loose coupling between client and concrete classes
✔ Easy to extend (add new notification types easily)
✔ Removes repeated `new` and `if-else` logic
✔ Improves maintainability

---

## 🔷 Disadvantages

❌ Can introduce extra abstraction (overhead for small apps)
❌ Factory may become a “God class” if not designed well
❌ Switch-case needs modification for new types (Simple Factory limitation)
❌ String-based selection can cause runtime errors

---

## 🔷 Key Interview Summary

> “Factory Pattern centralizes object creation by providing a single place to instantiate objects. It hides concrete implementations from the client, reduces coupling, and improves extensibility. However, it should be used wisely as it can introduce extra complexity and maintenance overhead if overused.”

---

## 🔷 One-Line Definition (Memorize)

> “Factory Pattern provides a centralized way to create objects without exposing the instantiation logic to the client, promoting loose coupling and flexibility.”

---

If you want, I can next give you:
👉 Factory vs DI vs Strategy pattern (very common interview comparison)
👉 Or real enterprise architecture example using Factory in .NET Core services
