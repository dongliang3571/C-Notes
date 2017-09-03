# C-Notes
C++ Notes

### Memeory leaking

Objects allocated with `new` operator must eventually be freed with `delete` operator, or there will be leaks. Even the `new` takes place in function's actual arguments. That is,

```c++

class SomeClass {
    public:
        SomeClass() {};
};

void func(SomeClass obj) {
  // do something with obj
}

// we can call func like below, but there will be leaks
func(new SomeClass());

// good way
SomeClass* sc = new SomeClass();
func(sc);

// or you can `delete` that obj in `func`, so the func should be like
void func(SomeClass obj) {
  // do something with obj
  delete obj;
}

// But this is not recommanded
```
