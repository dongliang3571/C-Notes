# C++ Notes
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

### `std::map`

In general

```c++
map<string, int>::iterator it;

for (it = symbolTable.begin(); it != symbolTable.end(); it++) {
    std::cout << it->first  // string (key)
              << ':'
              << it->second   // string's value 
              << std::endl ;
}
```

with C++11

```c++
for (auto const& x : symbolTable) {
    std::cout << x.first  // string (key)
              << ':' 
              << x.second // string's value 
              << std::endl ;
}
```

with C++17

```c++
for(auto const& [key, val] : symbolTable) {
    std::cout << key         // string (key)
              << ':'  
              << val        // string's value
              << std::endl ;
}
```
