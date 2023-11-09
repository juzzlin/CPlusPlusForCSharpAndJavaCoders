# C++ Do's and Don'ts for C# and Java coders

I have collected here some real-life examples on how to avoid annoying `C#` and `Java` idioms when writing `modern C++`.

In genaral the modern C++ can be very compact and we want to avoid unnecessary typing and stupidity.

This is not supposed to be a C++ tutorial.

## Testing variables

**Don't do**

```
    if (p != nullptr) {
        p->doSomething();
    }
```

**Do**

```
    if (p) {
        p->doSomething();
    }
```

In C++ we don't need to always explicitly test against whatever truth value.

**Don't do**

```
    if (b == true) {
        std::cout << "b was true" << std::endl;
    }
```

**Do**

```
    if (b) {
        std::cout << "b was true" << std::endl;
    }
```

Inverted case:

**Don't do**

```
    if (b == false) {
        std::cout << "b was false" << std::endl;
    }
```

**Do**

```
    if (!b) {
        std::cout << "b was false" << std::endl;
    }
```

## Instantiating variables: avoid using new

In C# and Java you have to instantiate almost everything with `new`. In C++ this is pretty much a legacy pattern.

In C++ we have these things called `stack` and `scope`:

**Don't do**

```
    void createString()
    {
        std::string * s = new std::string("foo");

        // Do something with s

        // You have to delete s yourself
        delete s;
    }
```

**Do**

```
    void createString()
    {
        std::string s("foo");

        // Do something with s

        // s gets automatically deleted here
    }
```

Or with smart pointers (`unique_ptr`, `shared_ptr`) that replace the legacy `new`:


```
    void createString()
    {
        const auto s = std::make_unique<std::string>("foo");

        // Do something with s

        // s gets automatically deleted here
    }
```

In modern C++ you almost **never** need to use `new` anymore (expect probably when working with frameworks like `Qt`).

## Instantiating variables: avoid needless typing

After we got `auto` things got amazing. Let's demonstrate this with the legacy `new` you usually don't need:

**Don't do**

```
    MyLongClassName * m = new MyLongClassName();
```

**Do**

```
    auto m = new MyLongClassName;
```

Note also, that `()` is not needed in C++. At least replace these with the universal `{}`:

```
    auto m = new MyLongClassName{};
```
## Instantiating variables: understand the existence of universal and brace-enclosed initializations

Often things are much easier in C++ than you first think.

**Don't do**

```
    Foo getEmptyObject()
    {
        foo Foo;
        return foo;
    }
```

**Don't do**

```
    Foo getEmptyObject()
    {
        return Foo();
    }
```

**Do**

```
    Foo getEmptyObject()
    {
        return {};
    }
```

How about this:

**Don't do**

```
    std::map<int, std::string> getMap()
    {
        std::map<int, std::string> m;
        m[1] = "one";
        m[2] = "two";
        return m;
    }
```

**Do**

```
    std::map<int, std::string> getMap()
    {
        return { {1, "one"}, {2, "two"} };
    }
```

Wow, that's neat!
