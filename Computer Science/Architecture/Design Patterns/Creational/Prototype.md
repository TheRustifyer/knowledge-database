# **🧬 5.1 Prototype Design Pattern — Overview**

> [!idea] **Concept Summary**
> The **Prototype Design Pattern** is all about **object copying** — specifically **deep cloning** of complex objects.

Instead of creating objects from scratch, we **replicate** existing ones and modify them as needed.

---

## **🎯 Motivation**

In real life, **complex designs** such as cars, smartphones, or machinery are **not created from scratch** every time.

When a manufacturer wants to make a new model of a car, they **don’t start with a blank sheet** — they **take an existing design** and **refine or modify** it:

- improving performance
- changing aesthetics
- or adding new features
  
==The Prototype pattern applies this same principle to software engineering.==

We start with a **pre-built object** (the “prototype”), and instead of rebuilding it step by step, we **clone** it to obtain a new instance that can then be customized.

---

## **🧩 Core Idea**

A **Prototype** is a **fully or partially initialized object** that serves as a **template** for new instances.

When we need a new variation:

1. We **copy** the prototype.
2. We **customize** the copy.

This allows for:
- Efficient object creation (especially when construction is expensive).
- Easy creation of similar but slightly different objects.

---

## **📖 Deep Copy vs. Shallow Copy**  

> ⚙️ **Key Detail: Deep Copy**
When copying an object, we must decide how references (pointers, sub-objects) are handled.

- **Shallow Copy** → copies only the top-level object, but references still point to the same memory.
- **Deep Copy** → recursively copies **all referenced objects**, producing an entirely independent duplicate.

## **🏗️ Implementation Goal**

To make cloning **convenient and safe**, we expose a clear API — often through:

- a **clone() method** inside the class itself, or
- a **factory** that handles the duplication of prototypes.

---

## **🧾 Definition**

> [!note] 🧱 **Prototype Pattern Definition**
> The **Prototype** pattern is a **creational design pattern** that allows you to **create new objects by copying existing ones** (prototypes), instead of instantiating new objects from scratch.

---

✅ **In summary:**

- A **prototype** is a **ready-made object** you can copy and modify.
- **Deep copy** ensures independence between clones.
- It is often used when **object creation is costly** or **requires complex setup**.
- **Factories** or **clone methods** make copying convenient.

---

# **🧾 5.2 Record Keeping — Motivation for the Prototype Pattern**

> [!intro]
> Before implementing the **Prototype Design Pattern**, let’s explore **why** we might need it in the first place — through a **realistic record-keeping example** involving employees and addresses.

## **🧱 The Problem Setup**

Imagine we have a system that keeps **records of employees** — each with:

- a **name**, and
- an **address** (which itself includes street, city, and suite information).

We can model this in C++ as follows:

```cpp
#include <iostream>
#include <string>

struct Address {
    std::string street;
    std::string city;
    int suite;

    Address(const std::string& street, const std::string& city, int suite)
        : street{street}, city{city}, suite{suite} {}
};

struct Contact {
    std::string name;
    Address address;

    Contact(const std::string& name, const Address& address)
        : name{name}, address{address} {}
};

// Pretty-print for debugging
std::ostream& operator<<(std::ostream& os, const Address& a) {
    return os << a.street << ", " << a.city << " [Suite " << a.suite << "]";
}

std::ostream& operator<<(std::ostream& os, const Contact& c) {
    return os << c.name << " lives at " << c.address;
}

int main() {
    Contact john{"John Doe", {"123 East Drive", "London", 123}};
    Contact jane = john; // Copy existing contact
    jane.name = "Jane Smith";
    jane.address.suite = 103; // Different office, same building

    std::cout << john << std::endl;
    std::cout << jane << std::endl;
}
```

### **✅ Output:**

```bash
John Doe lives at 123 East Drive, London [Suite 123]
Jane Smith lives at 123 East Drive, London [Suite 103]
```

Everything looks perfect — both Contact objects are independent, and modifying one doesn’t affect the other.

---

## **⚙️ Why It Works**

This example works **only because** Address is stored **by value** inside Contact.

Each Contact owns its own copy of the Address, so copying Contact creates a separate Address.

However, in **real-world systems**, we often store objects via **pointers** or **references** — especially when:

- objects are large,
- shared between many owners,
- or created dynamically (e.g., from a database).

Let’s see what happens then.

---

## **🧨 The Pointer Problem**

If we change Contact so that it stores a **pointer** to Address:

```cpp
struct Contact {
    std::string name;
    Address* address; // ⚠️ Pointer now!

    Contact(const std::string& name, Address* address)
        : name{name}, address{address} {}
};

std::ostream& operator<<(std::ostream& os, const Contact& c) {
    return os << c.name << " lives at " << *c.address;
}

int main() {
    Contact john{"John Doe", new Address{"123 East Drive", "London", 123}};
    Contact jane = john; // Copy constructor does shallow copy!
    jane.name = "Jane Smith";
    jane.address->suite = 103;

    std::cout << john << std::endl;
    std::cout << jane << std::endl;
}
```

### **⚠️ Output:**

```bash
John Doe lives at 123 East Drive, London [Suite 103]
Jane Smith lives at 123 East Drive, London [Suite 103]
```

==Both John and Jane ended up in the same suite!== 😱

That’s because `Contact jane = john;` only **copied the pointer**, not the underlying object.

---

## **🧩 Why This Matters**

When you use pointers or shared references:

- A **shallow copy** duplicates the pointer itself (both objects refer to the same memory).
- A **deep copy** duplicates the entire object structure (each gets its own independent data).

Without deep copying:

- Changes in one clone affect the original.
- Ownership and deletion become ambiguous.
- Memory leaks or double-deletes can occur.

---

## **🧠 Lesson Learned**

This is exactly the **motivation** for the **Prototype Pattern**.

> [!summary] ✅ **We need a clean, reliable way to clone complex objects**
> so that each copy gets its own deep structure — without rewriting all the constructor logic manually.

---

# **🧬 5.3 Prototype — Implementing Deep Copy**

> [!💡 **Recap:**]
> In the previous lesson, we saw that **shallow copying** (via operator=) doesn’t work properly when objects contain **pointers** — both copies end up sharing the same memory.

> [!definition]
> The **Prototype Pattern** fixes this by performing a **deep copy**: creating new instances of all referenced objects, not just copying the pointer values.

---

## **🎯 The Goal**

We want to:

- Copy an object (e.g., a Contact).    
- Ensure it gets its own independent copy of the data (especially its Address).
- Keep the process clean and reusable.

This makes the original object act as a **prototype**, from which we can clone new variations.

---

## **🧩 Implementing Deep Copy Using Copy Constructors**

Let’s start with the same setup, but now with pointers to emphasize deep copying.

```cpp
#include <iostream>
#include <string>

struct Address {
    std::string street;
    std::string city;
    int suite;

    Address(const std::string& street, const std::string& city, int suite)
        : street{street}, city{city}, suite{suite} {}

    // 🧬 Copy Constructor (deep copy)
    Address(const Address& other)
        : street{other.street}, city{other.city}, suite{other.suite} {}
};

struct Contact {
    std::string name;
    Address* address;

    Contact(const std::string& name, Address* address)
        : name{name}, address{address} {}

    // 🧬 Copy Constructor (deep copy)
    Contact(const Contact& other)
        : name{other.name}, address{new Address{*other.address}} {} // Deep copy the address

    ~Contact() {
        delete address; // Prevent memory leak
    }
};

// Stream output for debugging
std::ostream& operator<<(std::ostream& os, const Address& a) {
    return os << a.street << ", " << a.city << " [Suite " << a.suite << "]";
}

std::ostream& operator<<(std::ostream& os, const Contact& c) {
    return os << c.name << " lives at " << *c.address;
}

int main() {
    Contact john{"John Doe", new Address{"123 East Drive", "London", 123}};
    Contact jane{john}; // ✅ Deep copy using copy constructor

    jane.name = "Jane Smith";
    jane.address->suite = 103; // Modify Jane’s address independently

    std::cout << john << std::endl;
    std::cout << jane << std::endl;
}
```

### **✅ Output:**

```bash
John Doe lives at 123 East Drive, London [Suite 123]
Jane Smith lives at 123 East Drive, London [Suite 103]
```

---

## **🧠 How It Works**

The **copy constructor** for Contact explicitly:

1. Copies the name.
2. Allocates a **new Address** using new Address{*other.address}.
3. Uses the **copy constructor** of `Address** to recursively copy its fields.

==This ensures each Contact has its own independent copy of the address.==

>[!note]
Changing one no longer affects the other.

---

## **🧬 The Prototype in Action**

> [!tip] 🧱 **Conceptual Summary**
> - The object john acts as a **prototype**.
> - We create a new object (jane) by **cloning** john using the copy constructor.
> - We then **customize** the clone’s attributes (name, suite number, etc.).

==Both share the same structure but remain completely independent in memory.==

## **🧩 Summary**

✅ **Shallow Copy:** Copies only pointer values (leads to shared state).
✅ **Deep Copy:** Creates independent copies of all nested data.
✅ **Prototype Pattern:** Lets you create new objects by cloning existing, fully-initialized ones.

---

## **🧠 Key Takeaways**

- Use **copy constructors** or a **clone()** method to enable deep copying.
- The original instance serves as a **prototype** for creating similar objects.
- This is the essence of the **Prototype Design Pattern**.

# **🏭 5.4 Prototype Factory**


> [!idea] **Recap:**
> Previously, we had a variable like John acting as our **prototype**, and we manually cloned it to create Jane.

While this works, it’s **not convenient** for API consumers — especially when you want to:
- Provide predefined prototypes (e.g., _main office_, _branch office_ employees).
- Restrict object creation so that it only happens via cloning existing prototypes.

To solve this elegantly, we’ll introduce a **Prototype Factory**.

---

## **🎯 The Problem**  

If you simply define a global variable as a prototype:

```cpp
Contact main{"", new Address{"123 East Drive", "London", 0}};
```

You can make copies using the ==copy constructor== or `operator=`, but:

- It’s not **explicit** that users should clone it.
- It’s easy to misuse (e.g., directly constructing new instances).
- It doesn’t **scale** if you have multiple prototypes (like _main_ and _auxiliary_ offices).  

==We need a structured and reusable way to manage prototypes.==

---

## **🏗️ Solution: The Prototype Factory**

We’ll create an **EmployeeFactory** that:

- Holds predefined prototypes.
- Offers helper methods to create new employees by cloning those prototypes.
- Customizes each clone with the desired name and suite number.

### **🧩 Full Example**

```cpp
#include <iostream>
#include <string>
#include <memory>

// --------------------------------------------------
// Address structure
// --------------------------------------------------
struct Address {
    std::string street;
    std::string city;
    int suite;

    Address(const std::string& street, const std::string& city, int suite)
        : street{street}, city{city}, suite{suite} {}

    Address(const Address& other)
        : street{other.street}, city{other.city}, suite{other.suite} {}
};

// --------------------------------------------------
// Contact structure
// --------------------------------------------------
struct Contact {
    std::string name;
    std::unique_ptr<Address> address;

    Contact(const std::string& name, std::unique_ptr<Address> address)
        : name{name}, address{std::move(address)} {}

    // 🧬 Deep copy constructor
    Contact(const Contact& other)
        : name{other.name}, address{std::make_unique<Address>(*other.address)} {}

    // 🧬 Deep copy assignment operator
    Contact& operator=(const Contact& other) {
        if (this == &other)
            return *this;
        name = other.name;
        address = std::make_unique<Address>(*other.address);
        return *this;
    }

    // 📤 Stream output
    friend std::ostream& operator<<(std::ostream& os, const Contact& c) {
        return os << c.name << " lives at "
                  << c.address->street << ", " << c.address->city
                  << " [Suite " << c.address->suite << "]";
    }
};

// --------------------------------------------------
// Prototype Factory
// --------------------------------------------------
struct EmployeeFactory {
    // ⚙️ Generic factory utility
    static std::unique_ptr<Contact> NewEmployee(
        const std::string& name, int suite, const Contact& prototype
    ) {
        auto result = std::make_unique<Contact>(prototype);  // Clone prototype
        result->name = name;
        result->address->suite = suite;
        return result;
    }

    // 🏢 Main Office Prototype
    static std::unique_ptr<Contact> NewMainOfficeEmployee(
        const std::string& name, const int suite
    ) {
        static Contact prototype{
            "", std::make_unique<Address>("123 East Drive", "London", 0)
        };
        return NewEmployee(name, suite, prototype);
    }

    // 🏬 Auxiliary Office Prototype
    static std::unique_ptr<Contact> NewAuxOfficeEmployee(
        const std::string& name, const int suite
    ) {
        static Contact prototype{
            "", std::make_unique<Address>("66 West Drive", "Manchester", 0)
        };
        return NewEmployee(name, suite, prototype);
    }
};

// --------------------------------------------------
// Demonstration
// --------------------------------------------------
int main() {
    auto john = EmployeeFactory::NewMainOfficeEmployee("John Doe", 123);
    auto jane = EmployeeFactory::NewAuxOfficeEmployee("Jane Smith", 200);

    std::cout << *john << std::endl;
    std::cout << *jane << std::endl;
}
```

### **✅ Example Output**

```bash
John Doe lives at 123 East Drive, London [Suite 123]
Jane Smith lives at 66 West Drive, Manchester [Suite 200]
```

## **🗣️ Discussion**

This design provides:

- **Encapsulation:** All prototype details (e.g., addresses) are hidden inside the factory.    
- **Safety:** Only the factory creates employees; constructors can be made private if desired.
- **Reusability:** Each factory method reuses a **static prototype**, avoiding redundant object setup.    

---

## **🧬 5.5 [EXTRA] Prototype via Serialization**

When implementing the **Prototype Design Pattern**, one of the main challenges is performing a _deep copy_ of an object graph — particularly when the object contains pointers. In C++, this typically means writing boilerplate code like:

- Copy constructors
- Copy assignment operators
- Or explicit clone() methods

For simple value types, this is fine. But once pointers or dynamically allocated objects are involved, you must manually manage deep copies, which is error-prone and repetitive.

### **The Serialization Approach**  

A more elegant approach is to use **serialization and deserialization** to implement the prototype behavior automatically.

The reasoning is simple:

- Serialization captures _all_ aspects of an object    
- Deserialization restores them perfectly

> [!quote] So if we can serialize and deserialize an object, we can also _clone_ it.

In this case, we’ll use **Boost.Serialization**, a powerful and type-safe C++ library.

### **⚙️ Step 1 – Include the Serialization Headers**
  
We’ll include Boost’s serialization utilities and text archives (there are also binary and XML options):

```cpp
#include <boost/serialization/serialization.hpp>
#include <boost/archive/text_oarchive.hpp>
#include <boost/archive/text_iarchive.hpp>
#include <sstream>
```

We can also shorten the syntax:

```cpp
using namespace boost;
```

### **⚙️ Step 2 – Add Serialization Support to** 

### **Contact**

We define a serialize() method that Boost can use internally.
Because Boost needs access to private members, we declare it as a friend.

```cpp
struct Contact {
    std::string name;
    Address* address = nullptr;

private:
    friend class boost::serialization::access;

    template<class Archive>
    void serialize(Archive& ar, const unsigned version) {
        ar & name;
        ar & address; // Boost automatically dereferences and handles pointers
    }

public:
    Contact() = default; // Required for serialization
    Contact(std::string name, Address* addr)
        : name(std::move(name)), address(addr) {}
};
```

### **⚙️ Step 3 – Add Serialization to** 

### **Address**

Every serializable member type must also implement the same interface:

```cpp
struct Address {
    std::string street;
    std::string city;
    int suite = 0;

private:
    friend class boost::serialization::access;

    template<class Archive>
    void serialize(Archive& ar, const unsigned version) {
        ar & street;
        ar & city;
        ar & suite;
    }

public:
    Address() = default; // Needed by Boost
    Address(std::string street, std::string city, int suite)
        : street(std::move(street)), city(std::move(city)), suite(suite) {}
};
```

### **⚙️ Step 4 – Implement the** **clone() function**

Now we can define a lambda or function that performs deep cloning through serialization:

```cpp
auto clone = [](const Contact& c) {
    std::ostringstream oss;
    {
        archive::text_oarchive oa(oss);
        oa << c; // serialize the object
    }

    // Optional: view serialized text
    std::string serialized = oss.str();
    std::cout << serialized << std::endl;

    std::istringstream iss(serialized);
    Contact result;
    {
        archive::text_iarchive ia(iss);
        ia >> result; // deserialize the object
    }

    return result;
};
```

This approach:

- Serializes the object to an in-memory string.
- Deserializes it into a brand-new object.
- Returns a **deep copy** with no shared pointers.

### **⚙️ Step 5 – Try It Out**

We can combine this with the **Prototype Factory** from before:

```cpp
auto john = EmployeeFactory::NewMainOfficeEmployee("John", 123);
auto jane = clone(*john);

jane.name = "Jane";

std::cout << *john << std::endl;
std::cout << jane << std::endl;
```

Output:

```bash
Contact: John, 123 East Dr, London, Suite 123
Contact: Jane, 123 East Dr, London, Suite 123
```

The serialization step will also print an internal representation, e.g.:

```bash
22 serialization::archive 18 0 0 John 0 123 East Dr London 123
```

### **🧩 Summary**

The **serialization-based prototype** technique gives us:

- A **universal cloning mechanism**,
- Cleaner code (no manual pointer management),
- Built-in **validation** for missing serialization code,
- And **future-proof extensibility** (persistence, networking, etc.).    

It’s an elegant and safe way to implement **deep copy semantics** in C++.

---
## **5.6 🧩 Final Summary**

Let’s summarize what we’ve learned about the **Prototype Design Pattern**.

To implement a prototype, you first construct an object — either partially or fully — and store it somewhere so it can be **replicated** later.

The key question is: _how do you clone the prototype?_

There are two main approaches:

1. **Manual deep copy:**
    
    You can implement deep copy functionality yourself, for example by creating copy constructors or defining a clone() method in a Prototype interface that all cloneable classes implement.
    
    However, this approach can become complex and error-prone, especially for large object hierarchies.
    
2. **Serialization-based cloning:**
    
    A simpler approach is to **serialize and then deserialize** the object. Serialization automatically traverses the entire object graph, effectively producing a deep copy with minimal effort.
    

Once the object has been cloned, all that remains is to **customize the new instance** and start using it in your code.

In essence, the Prototype pattern provides a convenient way to create new objects by copying existing ones, especially when object creation is expensive or complex.
