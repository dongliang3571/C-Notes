# C++ Notes
C++ Notes

### Some combination and permuation formulas

[resource](https://www.mathsisfun.com/combinatorics/combinations-permutations.html)

[special combination case](http://mathforum.org/library/drmath/view/56197.html)

`0!` is define as `1`

**Permutation**

with repetition:

```
n^r
```

where n is the number of things to choose from, and we choose r of them

without repetition

```
n! / (n-r)!  
```

where n is the number of things to choose from, and we choose r of them

**Combination**

with repetition:

```
(r+n-1)! / r!(n-1)!
```

where n is the number of things to choose from, and we choose r of them

without repetition

```
n! / (n-r)!r!  
```

where n is the number of things to choose from, and we choose r of them


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

**NOTICE: ** `delete` does not set point to `NULL` or `nullptr`. 

[resource1](https://stackoverflow.com/questions/704466/why-doesnt-delete-set-the-pointer-to-null)

[resource2](http://www.stroustrup.com/bs_faq2.html#delete-zero)

Consider following case

```c++
struct ListNode {
     int val;
     ListNode *next;
     ListNode(int x) : val(x), next(NULL) {}
};

ListNode* root = new ListNode(2);

delete root;

if (root == nullptr) std::cout << "empty" << '\n';
else std::cout << "NOT empty" << '\n';

// output is "NOT empty"
// because pointer still has value, but the object that pointer points to has been deleted
```

### `std::map`, `#include <map>`

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

### `std::bitset`, `#include <bitset>`

A bitset stores bits (elements with only two possible values: 0 or 1, true or false, ...).

```c++
// bitset::count
#include <iostream>       // std::cout
#include <string>         // std::string
#include <bitset>         // std::bitset

int main ()
{
  std::bitset<8> foo (std::string("10110011"));

  std::cout << foo << " has ";
  std::cout << foo.count() << " ones and ";
  std::cout << (foo.size()-foo.count()) << " zeros.\n";

  return 0;
}

// output: 10110011 has 5 ones and 3 zeros.
```

### `istringstream`, `ostringstream`, `stringstring`, `#include <sstring>`

```c++
string str = "word hello";

istringstream iss(str);

str tmp;

while (iss >> tmp) { 
    cout << tmp << '\n'\
}

// output will be:
//  word
//  hello
```

**Note** that this syntax `iss >> tmp`, refer to http://www.cs.technion.ac.il/users/yechiel/c++-faq/istream-and-while.html

```c++

ostringstream oss;

oss << "word" << "hello";

cout << oss.str();
// output will be: wordhello
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

### Declaration and initialization

`vector<int> V[]` vs. `vector< vector<int> > V`

`vector<int> V[]` is an array of vectors.

`vector<vector<int>> V` is a vector of vectors.

Using arrays are **C-style** coding, using vectors are **C++-style** coding.

Quoting cplusplus.com ,

Vectors are sequence containers representing arrays that can change in size.

Just like arrays, vectors use contiguous storage locations for their elements, which means that their elements can also be accessed using offsets on regular pointers to its elements, and just as efficiently as in arrays. But unlike arrays, their size can change dynamically, with their storage being handled automatically by the container.

When you want to work with a fixed number of std::vector elements, you can use vector <int> V[].

When you want to work with a dynamic array of std::vector, you can use  vector< vector<int> > V.
