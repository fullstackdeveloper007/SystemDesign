# Design Pattern 

## 1. What is Repository Pattern?

**Repository Pattern** is a **design pattern** that acts as an **abstraction layer between the business logic and the data access logic**.
In this pattern we dont expose context classes to business logic/service layer. on top of dataaccess layer we will have repository classes and repostory classes will be exposed to services. 
In case of change of dataAccess from Entity framework to DApper or any other, we can just update the underlying EF layer and the service will not be impacted.
> *Your application talks to a repository, not directly to the database or ORM.*

## 2. Why do we need Repository Pattern?

### Problems without Repository

* Business logic depends directly on **EF / SQL / Dapper**
* Hard to **unit test**
* Data access code is **scattered**
* Switching DB or ORM is painful

### Benefits

✅ Separation of concerns
✅ Cleaner business logic
✅ Easier unit testing (mock repositories)
✅ Centralized data access logic

---

## 3. Where Repository Pattern Fits (Architecture View)

```
Controller / Service
        ↓
    Repository (Interface)
        ↓
    EF Core / Dapper / SQL
        ↓
      Database
```

In **Onion / Clean Architecture**, Repository lives in:

* **Domain layer** → interface
* **Infrastructure layer** → implementation

---

## 4. Core Components of Repository Pattern

### 1️⃣ Entity (Domain Model)

```csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

---

### 2️⃣ Repository Interface

> Defined in **Domain or Application layer**

```csharp
public interface IEmployeeRepository
{
    Employee GetById(int id);
    IEnumerable<Employee> GetAll();
    void Add(Employee employee);
    void Update(Employee employee);
    void Delete(int id);
}
```

---

### 3️⃣ Repository Implementation

> Defined in **Infrastructure layer**

```csharp
public class EmployeeRepository : IEmployeeRepository
{
    private readonly AppDbContext _context;

    public EmployeeRepository(AppDbContext context)
    {
        _context = context;
    }

    public Employee GetById(int id)
    {
        return _context.Employees.Find(id);
    }

    public IEnumerable<Employee> GetAll()
    {
        return _context.Employees.ToList();
    }

    public void Add(Employee employee)
    {
        _context.Employees.Add(employee);
        _context.SaveChanges();
    }

    public void Update(Employee employee)
    {
        _context.Employees.Update(employee);
        _context.SaveChanges();
    }

    public void Delete(int id)
    {
        var emp = _context.Employees.Find(id);
        if (emp != null)
        {
            _context.Employees.Remove(emp);
            _context.SaveChanges();
        }
    }
}
```

---

### 4️⃣ Usage in Service / Controller

```csharp
public class EmployeeService
{
    private readonly IEmployeeRepository _repository;

    public EmployeeService(IEmployeeRepository repository)
    {
        _repository = repository;
    }

    public Employee GetEmployee(int id)
    {
        return _repository.GetById(id);
    }
}
```

---

### 5️⃣ Dependency Injection

```csharp
services.AddScoped<IEmployeeRepository, EmployeeRepository>();
```

---

## 5. Generic Repository (Common in Interviews)

### Generic Interface

```csharp
public interface IRepository<T> where T : class
{
    T GetById(int id);
    IEnumerable<T> GetAll();
    void Add(T entity);
    void Update(T entity);
    void Delete(T entity);
}
```

### Generic Implementation

```csharp
public class Repository<T> : IRepository<T> where T : class
{
    protected readonly DbContext _context;
    protected DbSet<T> _dbSet;

    public Repository(DbContext context)
    {
        _context = context;
        _dbSet = context.Set<T>();
    }

    public T GetById(int id) => _dbSet.Find(id);
    public IEnumerable<T> GetAll() => _dbSet.ToList();
    public void Add(T entity) => _dbSet.Add(entity);
    public void Update(T entity) => _dbSet.Update(entity);
    public void Delete(T entity) => _dbSet.Remove(entity);
}
```

---

## 6. Repository + Unit of Work (Best Practice)

**Problem:** Multiple repositories calling `SaveChanges()`
**Solution:** Unit of Work

```csharp
public interface IUnitOfWork
{
    IEmployeeRepository Employees { get; }
    int Save();
}
```

```csharp
public class UnitOfWork : IUnitOfWork
{
    private readonly AppDbContext _context;
    public IEmployeeRepository Employees { get; }

    public UnitOfWork(AppDbContext context)
    {
        _context = context;
        Employees = new EmployeeRepository(context);
    }

    public int Save() => _context.SaveChanges();
}
```

---

## 7. Common Interview Questions & Smart Answers

### ❓ Is Repository Pattern required with EF Core?

✅ **Not mandatory**, EF Core already implements Repository + Unit of Work
👉 But **custom repositories** are useful for:

* Complex queries
* Domain-specific methods
* Better testability

---

### ❓ Repository vs Service Layer?

* **Repository** → Data access
* **Service** → Business rules & orchestration

---

### ❓ How it relates to Onion Architecture?

* Repository **interfaces** → Domain layer
* Repository **implementation** → Infrastructure layer
* Domain does **not depend on EF or DB**

---

## 8. One-Line Interview Definition ⭐

> **“Repository Pattern abstracts data access logic and provides a clean, testable way to interact with the persistence layer without exposing database details to the business logic.”**



* **How interviewers trap candidates on Repository questions**
* **Repository in Microservices**

Just tell me 👍
