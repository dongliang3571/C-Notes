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

**Iteration**

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

### Range based loop

```c++
// This is a typical range based loop
vector<int> v(5, 1);
for (int i : v) {
    cout << i << ' ';
}

// output: 1 1 1 1 1
```

In range based loop we can decide the way of getting each element from sequences. That is, copy of a element, or an original element.

```c++
vector<int> v(5, 1);

// i here is a copy of elements in v
for (int i : v) {
    i = 4;
}

for (int i : v) {
    cout << i << ' ';
}

// output will still be: 1 1 1 1 1
```

```c++
vector<int> v(5, 1);

// i here is the original elements in v
for (int &i : v) {
    i = 4;
}

for (int i : v) {
    cout << i << ' ';
}

// output will be: 4 4 4 4 4, because the original argument was updated in last loop.
```

```c++
// Don't want copy but dont want to modify the original, we can use combination of const and &
// This way original element is used but not allow to be modified
for (int const& i : v) {
    i = 4; // error!
}

// const can be before data type, it's the same
for (const int& i : v) {
    i = 4; // error!
}
```
