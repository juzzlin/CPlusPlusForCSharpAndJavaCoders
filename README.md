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

In C++ we don't need to always explicitly test against whatever falsy value.

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
