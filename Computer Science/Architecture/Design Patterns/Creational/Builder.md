# Intro

We’ll begin our discussion of design patterns with the Builder — our first creational design pattern.

> [!info]
> The Builder Pattern is used when the construction of an object involves multiple steps or requires clarity and flexibility during object creation.

Some objects are simple — for example, a Point object with `x` and `y` coordinates — and can be created easily with a single constructor call.

However, other objects are complex and require multiple steps or different configurations before being fully constructed.

For instance, when constructing a long string from many smaller pieces, you wouldn’t concatenate with `+` repeatedly. Instead, you would use a string builder, which efficiently constructs content piece by piece.

> [!warning]
> Having constructors with too many parameters is error-prone and reduces readability.

The Builder Pattern solves this by allowing step-by-step construction using a clear and expressive API.

In short:
	•	Builders provide piecewise construction.
	•	They return the final, ready-to-use object only when complete.
	•	They make construction logic more readable and maintainable.

# Life Without Builders

Before implementing the [[Builder]] pattern, let’s consider a world without it.

Imagine building HTML content in a web server backend manually:

```cpp
std::string output;
std::string text = "hello";

output += "<p>";
output += text;
output += "</p>";

std::cout << output;
```

This simple approach works fine for tiny snippets of static content.
Now imagine constructing a list dynamically:

```cpp
std::vector<std::string> words = {"hello", "world"};
std::ostringstream os;

os << "<ul>\n";
for (auto& w : words)
  os << "  <li>" << w << "</li>\n";
os << "</ul>\n";

std::cout << os.str();
```

Again, this works — but it’s just managing strings, not actual HTML structure.

> [!warning]
> This approach doesn’t enforce rules about tag nesting or hierarchy and doesn’t model HTML in an object-oriented way.

## The Problem

There is no structure, no constraints on valid combinations, and no meaningful abstraction.

The [[Builder]] Pattern fixes this by defining object-oriented abstractions that represent each part of the structure, allowing recursive composition and consistent construction logic.

# The Builder

To make the example of the `HTML` construction scalable, let’s introduce an object-oriented model.

## HTML Element Structure

```cpp
struct HtmlElement {
    std::string name;
    std::string text;
    std::vector<HtmlElement> elements;
    const size_t indent_size = 2;

    HtmlElement() = default;
    HtmlElement(const std::string& name, const std::string& text)
        : name(name), text(text) {}

    std::string str(int indent = 0) const; // recursive string builder
};
```

Each `HtmlElement`:
- Represents a tag (like `<p>` or `<li>`).
- Can contain other elements recursively.
- Provides `str()` for string output with indentation.

### The Builder to the rescue

Next, we define a helper class to build `HTML` elements fluently.

```cpp
struct HtmlBuilder {
    HtmlElement root;

    HtmlBuilder(std::string root_name) { root.name = root_name; }

    void add_child(const std::string& child_name, const std::string& child_text) {
        HtmlElement e{child_name, child_text};
        root.elements.emplace_back(e);
    }

    std::string str() const { return root.str(); }
};
```

And to use it:

```cpp
HtmlBuilder builder("ul");
builder.add_child("li", "hello");
builder.add_child("li", "world");

std::cout << builder.str();
```

> [!success]
> The HTML Builder separates structure building (object composition) from rendering (string conversion).
>
>The result is structured, type-safe, and extendable.

# The fluent Builder

We can evolve our builder to support a fluent interface — method chaining — for expressive, compact code.
## Fluent Additions

Instead of returning `void` from `add_child`, return a reference to the builder:

```cpp
HtmlBuilder& add_child(const std::string& child_name, const std::string& child_text) {
    root.elements.emplace_back(HtmlElement{child_name, child_text});
    return *this;
}
```

Usage:

```cpp
HtmlBuilder builder("ul");
builder.add_child("li", "hello")
       .add_child("li", "world");
```

## Static Factory and Conversion Operators

To make the API self-explanatory:

```cpp
struct HtmlElement {
    static HtmlBuilder build(const std::string& root_name);
};
```

So now, instead of directly instantiating the builder:

```cpp
auto builder = HtmlElement::build("ul")
                   .add_child("li", "hello")
                   .add_child("li", "world");
```

Add an implicit conversion operator for convenience:

```cpp
operator HtmlElement() const { return root; }
```

This allows seamless transformation between builder results and elements.

# Optional — Making the Builder the Only Entry Point

To enforce builder usage, restrict direct construction and declare the builder as a friend class:

```cpp
class HtmlElement {
    friend class HtmlBuilder;

    HtmlElement() = default;
    HtmlElement(const std::string& name, const std::string& text);
public:
    static HtmlBuilder create(const std::string& root_name);
};
```

Now all construction goes through `HtmlElement::create()`.

The `build()` function then finalizes and returns the object.

```cpp
auto elem = HtmlElement::create("ul")
                .add_child("li", "hello")
                .add_child("li", "world")
                .build();
```

> [!tip]
> Private constructors + a friend builder enforce consistent creation workflows — preventing misuse and encouraging clarity.

# Key Takeaways
	•	The Builder Pattern separates construction from representation.
	•	It allows step-by-step, fluent, and safe object creation.
	•	Ideal when:
	•	An object requires many initialization steps.
	•	Constructor arguments become unwieldy.
	•	You want to enforce a specific creation sequence.

# Groovy-Style Builder

> [!info]
> You can mimic HTML or DSL-like syntaxes in C++ by using uniform initialization and class composition, even though C++ doesn’t natively support inline HTML.

==Suppose you want to write HTML==-like constructs directly in your `C++` code.
While `C++` isn’t as flexible as some dynamic languages, you can approach this by modeling `HTML` elements as classes and leveraging `C++` uniform initialization and initializer lists.

## Tag Structure

```cpp
struct Tag {
    std::string name, text;
    std::vector<Tag> children;
    std::vector<std::pair<std::string, std::string>> attributes;

    // Stream output operator -- prints the tag, attributes, children, and text nicely
    friend std::ostream& operator<<(std::ostream& os, const Tag& tag);
protected:
    Tag(const std::string& name, const std::string& text);
    Tag(const std::string& name, const std::vector<Tag>& children);
};
```

You then build concrete tags as subclasses. For example:

```cpp
struct P : Tag {
    P(const std::string& text) : Tag("p", text) {}
    P(const std::initializer_list<Tag>& children) : Tag("p", children) {}
};

struct Img : Tag {
    Img(const std::string& url) : Tag("img", "") {
        attributes.emplace_back("src", url);
    }
};
```

## HTML-Like Construction

With this structure, you can “write” `HTML` in ==C++==:

```cpp
std::cout << P{
    Img{"http://pokemon.com/Pikachu.png"}
} << std::endl;
```

This prints a paragraph with an image element inside — resembling how you would write 
HTML markup, but using valid ==C++==.

> [!tip]
> Using initializers and custom constructors, you can restrict what types of elements and children are allowed, ensuring type safety and valid HTML structure.

This approach effectively creates a domain-specific language (DSL) for building HTML (or similar structures) in `C++`.

# Builder Facets

> [!info]
> For complex objects, consider splitting the builder into multiple specialized sub-builders, each handling a different aspect (“facet”).

Suppose you have a `Person` class with multiple aspects — address and employment info:

```cpp
class Person {
    std::string street_address, post_code, city;
    std::string company_name, position;
    int annual_income = 0;

    friend class PersonBuilder;
    friend class PersonAddressBuilder;
    friend class PersonJobBuilder;
public:
    static PersonBuilder create();
};
```

## The Builder Facade Structure

- `PersonBuilder`: Facade and entry point for building Person objects.
- `PersonBuilderBase`: Stores a reference to the Person being built, shared by sub-builders.
- `PersonAddressBuilder`: Fluent API to set address properties.
- `PersonJobBuilder`: Fluent API to set employment properties.

This enables chaining:

```cpp
Person p = Person::create()
    .lives().at("123 London Road").with_postcode("SW1 1AB").in("London")
    .works().at("Acme Corp").as_a("Engineer").earning(120000);
```

- Entry point is always `Person::create()`.
- The builder returns sub-builders (`lives`, `works`) to transition between facets.
- Each sub-builder returns itself, supporting fluent chaining.

> [!success]
> By separating aspects, you keep the API clear and expressive while avoiding duplication or confusion.

An implicit conversion operator turns the final builder chain into a `Person` instance:

```cpp
operator Person() const { return std::move(person); }
```

This technique combines [[Builder]] and [[Facade]] patterns — ideal for flexible construction of objects with multiple logically related facets.

# Summary

The Builder pattern provides a separate, dedicated component for step-by-step construction of complex objects, improving readability and correctness.

Fluent builders allow chainable and efficient API usage.

## Key takeaways

- You can enforce builder usage via private constructors and static creation methods.
- Splitting builder logic by aspect and sharing a common base enables switching and combines expressive construction with clear separation of concerns.
- Groovy-style builders and custom DSLs can be mimicked in C++ using initializer lists and smart constructor/hierarchy design.

> [!info]
> Use builders to create flexible, maintainable, and robust APIs for objects whose construction is too complicated for simple constructors.
