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

### Templates

**Function templates**

Function templates are special functions that can operate with generic types. This allows us to create a function template whose functionality can be adapted to more than one type or class without repeating the entire code for each type.

For example, to create a template function that returns the greater one of two objects we could use: 

```c++
template<class myType>
myType GetMax(myType a, myType b) {
   return (a>b?a:b);
}
```

To use this function template we use the following format for the function call:

```c++
function_name<type> (parameters);
```

For example, to call GetMax to compare two integer values of type int we can write:

```c++
int x,y;
GetMax <int> (x,y);
```

When the compiler encounters this call to a template function, it uses the template to automatically generate a function replacing each appearance of myType by the type passed as the actual template parameter (int in this case) and then calls it. This process is automatically performed by the compiler and is invisible to the programmer.

Here is the entire example:

```c++
// function template
#include <iostream>
using namespace std;

template <class T>
T GetMax (T a, T b) {
  T result;
  result = (a>b)? a : b;
  return (result);
}

int main () {
  int i=5, j=6, k;
  long l=10, m=5, n;
  k=GetMax<int>(i,j);
  n=GetMax<long>(l,m);
  cout << k << endl;
  cout << n << endl;
  return 0;
}

// output:
// 6
// 10

```
**Note:** In this specific case where the generic type `T` is used as a parameter for `GetMax` the compiler can find out automatically which data type has to instantiate without having to explicitly specify it within angle brackets (like we have done before specifying `<int>` and `<long>`). So we could have written instead:

```c++
int i,j;
GetMax (i,j);
```


Since both i and j are of type int, and the compiler can automatically find out that the template parameter can only be int. This implicit method produces exactly the same result:

```
// function template II
#include <iostream>
using namespace std;

template <class T>
T GetMax (T a, T b) {
  return (a>b?a:b);
}

int main () {
  int i=5, j=6, k;
  long l=10, m=5, n;
  k=GetMax(i,j);
  n=GetMax(l,m);
  cout << k << endl;
  cout << n << endl;
  return 0;
}

// output:
// 6
// 10
```

However, we cannot do this,

```c++
int i;
long l;
k = GetMax (i,l);
```

we cannot call our function template with two objects of different types as arguments.

### `std::priority_queue`

The template parameter should be the type of the comparison function. The function is then either default-constructed or you pass a function in the constructor of priority_queue. So try either

```c++
std::priority_queue<int, std::vector<int>, decltype(&compare)> pq(&compare);
```

or don't use function pointers but instead a functor from the standard library which then can be default-constructed, eliminating the need of passing an instance in the constructor:

```c++
std::priority_queue<int, std::vector<int>, std::less<int> > pq;
```

If your comparison function can't be expressed using standard library functors (in case you use custom classes in the priority queue), I recommend writing a custom functor class, or use a lambda.

**lambda **

First define the lambda:

```c++
auto compareFunc = [](int a, int b) { return a > b; };
```

Then use decltype:

```c++
typedef priority_queue<int, vector<int>, decltype(compareFunc)> q2;
```

Now when you use `q2`, pass in the function:

```c++
q2 myQueue(compareFunc);
```

Basically, priority_queue takes the type of a function as it's 3rd template argument, while the constructor takes a pointer to that function itself.

### `decltype`

Inspects the declared type of an entity or the type and value category of an expression

```c++
#include <iostream>
 
struct A { double x; };
const A* a = new A{0};
 
decltype(a->x) y;       // type of y is double (declared type)
decltype((a->x)) z = y; // type of z is const double& (lvalue expression)
 
template<typename T, typename U>
auto add(T t, U u) -> decltype(t + u) // return type depends on template parameters
{
    return t+u;
}
 
int main() 
{
    int i = 33;
    decltype(i) j = i * 2;
 
    std::cout << "i = " << i << ", "
              << "j = " << j << '\n';
 
    auto f = [](int a, int b) -> int
    {
        return a * b;
    };
 
    decltype(f) g = f; // the type of a lambda function is unique and unnamed
    i = f(2, 2);
    j = g(3, 3);
 
    std::cout << "i = " << i << ", "
              << "j = " << j << '\n';
}

//output:
//    i = 33, j = 66
//    i = 4, j = 9
```

### Initialization

Suppose I have a class with private memebers ptr, name, pname, rname, crname and age. What happens if I don't initialize them myself? Here is an example:

```c++
class Example {
    private:
        int *ptr;
        string name;
        string *pname;
        string &rname;
        const string &crname;
        int age;

    public:
        Example() {}
};
```

And then do

```c++
int main() {
    Example ex;
}
```

**Question:** 

How are the members initialized in ex? What happens with pointers? Do string and int get 0-intialized with default constructors string() and int()? What about the reference member? Also what about const references?

**Answer:**

In lieu of explicit initialization, initialization of members in classes works identically to initialization of local variables in functions.

For objects, their default constructor is called. For example, for std::string, the default constructor sets it to an empty string. If the object's class does not have a default constructor, it will be a compile error if you do not explicitly initialize it.

For primitive types (pointers, ints, etc), they are not initialized -- they contain whatever arbitrary junk happened to be at that memory location previously.

For references (e.g. std::string&), it is illegal not to initialize them, and your compiler will complain and refuse to compile such code. References must always be initialized.

So, in your specific case, if they are not explicitly initialized:

```c++
int *ptr;  // Contains junk
string name;  // Empty string
string *pname;  // Contains junk
string &rname;  // Compile error
const string &crname;  // Compile error
int age;  // Contains junk
```
