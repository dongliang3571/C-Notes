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

### lvalues and rvalues

https://msdn.microsoft.com/en-us/library/f90831hc.aspx -- old

https://docs.microsoft.com/en-us/cpp/cpp/lvalues-and-rvalues-visual-cpp -- new

Every C++ expression is either an lvalue or an rvalue. An lvalue refers to an object that persists beyond a single expression. You can think of an lvalue as an object that has a name. All variables, including nonmodifiable (const) variables, are lvalues. An rvalue is a temporary value that does not persist beyond the expression that uses it. To better understand the difference between lvalues and rvalues, consider the following example

```c++
#include <iostream>  
using namespace std;  
int main()  
{  
   int x = 3 + 4;  
   cout << x << endl;  
}  
```

In this example, `x` is an lvalue because it persists beyond the expression that defines it. The expression `3 + 4` is an rvalue because it evaluates to a temporary value that does not persist beyond the expression that defines it.

The following example demonstrates several correct and incorrect usages of lvalues and rvalues:

```c++
int main()  
{  
   int i, j, *p;  
  
   // Correct usage: the variable i is an lvalue.  
   i = 7;  
  
   // Incorrect usage: The left operand must be an lvalue (C2106).  
   7 = i; // C2106  
   j * 4 = 7; // C2106  
  
   // Correct usage: the dereferenced pointer is an lvalue.  
   *p = i;   
  
   const int ci = 7;  
   // Incorrect usage: the variable is a non-modifiable lvalue (C3892).  
   ci = 9; // C3892  
  
   // Correct usage: the conditional operator returns an lvalue.  
   ((i < 3) ? i : j) = 7;  
}  
```

### Pointer vs Reference

**Pointers:** A pointer is a variable that holds memory address of another variable. A pointer needs to be dereferenced with * operator to access the memory location it points to.

```c++
int x = 1;
int *p = &x; // assign x's address to pointer p
```

**References:** A reference variable is an alias, that is, another name for an already existing variable. A reference, like a pointer, is also implemented by storing the address of an object.

```c++
int x = 1;
int &r = x; // r point to whatever is in the address that x locates
int c = x; // simply copy the value of x to c, c has its own address

cout << r; // the value of x
cout << x; // the value of x
cout << &r; // the address of x
cout << &x; // the address of x
cout << &c; // the address of c
```

A reference can be thought of as a constant pointer (not to be confused with a pointer to a constant value!) with automatic indirection, i.e the compiler will apply the * operator for you.

```c++
int i = 3; 

// A pointer to variable i (or stores
// address of i)
int *ptr = &i; 

// A reference (or alias) for i.
int &ref = i; 
```

1. **Reassignment:** A pointer can be re-assigned. This property is useful for implementation of data structures like linked list, tree, etc. See the following examples:

```c++
int x = 5;
int y = 6;
int *p;
p =  &x;
p = &y;

cout << *p
```
On the other hand, a reference cannot be re-assigned, and must be assigned at initialization.

2. **Memory Address:** A pointer has its **own memory address** and size on the stack whereas a reference shares the same memory address (with the original variable) but also takes up some space on the stack. References may be passed to functions, stored in classes, etc. in a manner very similar to pointers.  Pointer is an independent variable and can be assigned NEW address values; whereas a reference, once assigned, can never refer to any new object until the variable goes out of scope.

```c++
int x = 5;
int *r = &x;

cout << &r; // address of the pointer
cout << &x; // address of the x
cout << *r; // the value of x
cout << &(*r); // address of the x
```

### array declaration and initialization

To create dynamically 3D array of integers, its better you understand 1D and 2D array first.

1D array: You can do this very easily by

```c++
const int MAX_SIZE=128;
int *arr1D = new int[MAX_SIZE];
```

Here, we are creating a int-pointer which will point to a chunk of memory where integers can be stored.

2D array: You may use solution of above 1D array to create 2D array. First crate a pointer which should point to a memory block where only other integeral pointers are hold which ultimately points to actual data. since our first pointer point to a array of pointer so this will be called as pointer-to-pointer (double pointer).

```c++
const int HEIGHT=20;
const int WIDTH=20;

int **arr2D = new int*[WIDTH];  //create an array of int pointers (int*), that will point to 
                                //data as described in 1D array.
for(int i = 0;i < WIDTH; i++){
      arr2D[i] = new int[HEIGHT]; 
}
```

3D Array: This is what you want to do. Here you may try both the scheme used in above two. Apply the same logic as 2D array. Diagram in question explains all. First array will be pointer-to-pointer-to-pointer (int*** - since it points to double pointers). Solution is as below:

```c++
const int X=20;
const int Y=20;
const int z=20;

int ***arr3D = new int**[X];
for(int i =0; i<X; i++){
   arr3D[i] = new int*[Y];
   for(int j =0; j<Y; j++){
       arr3D[i][j] = new int[Z];
       for(int k = 0; k<Z;k++){
          arr3D[i][j][k] = 0;
       }
   }
}
```

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

### `multiset`, `#include <set>`

```c++
template < class T,                        // multiset::key_type/value_type
           class Compare = less<T>,        // multiset::key_compare/value_compare
           class Alloc = allocator<T> >    // multiset::allocator_type
           > class multiset;
```

Multisets are containers that store elements following a specific order, and where multiple elements can have equivalent values.

In a multiset, the value of an element also identifies it (the value is itself the key, of type T). The value of the elements in a multiset cannot be modified once in the container (the elements are always const), but they can be inserted or removed from the container.

```c++
#include <iostream>
#include <set>

int main ()
{
  std::multiset<int> mymultiset;
  std::multiset<int>::iterator it;

  // set some initial values:
  for (int i=1; i<=5; i++) mymultiset.insert(i*10);  // 10 20 30 40 50

  it=mymultiset.insert(25);

  it=mymultiset.insert (it,27);    // max efficiency inserting
  it=mymultiset.insert (it,29);    // max efficiency inserting
  it=mymultiset.insert (it,24);    // no max efficiency inserting (24<29)

  int myints[]= {5,10,15};
  mymultiset.insert (myints,myints+3);

  std::cout << "mymultiset contains:";
  for (it=mymultiset.begin(); it!=mymultiset.end(); ++it)
    std::cout << ' ' << *it;
  std::cout << '\n';

  return 0;
}
//output: myset contains: 5 10 10 15 20 24 25 27 29 30 40 50
```

### `std::for_each`, `#include <algorithm>`

```c++
template <class InputIterator, class Function>
   Function for_each (InputIterator first, InputIterator last, Function fn)
```

Applies function fn to each of the elements in the range [first,last).

The behavior of this template function is equivalent to:

```c++
template<class InputIterator, class Function>
  Function for_each(InputIterator first, InputIterator last, Function fn)
{
  while (first!=last) {
    fn (*first);
    ++first;
  }
  return fn;      // or, since C++11: return move(fn);
}
```

Example:

```c++
// for_each example
#include <iostream>     // std::cout
#include <algorithm>    // std::for_each
#include <vector>       // std::vector

void myfunction (int i) {  // function:
  std::cout << ' ' << i;
}

struct myclass {           // function object type:
  void operator() (int i) {std::cout << ' ' << i;}
} myobject;

int main () {
  std::vector<int> myvector;
  myvector.push_back(10);
  myvector.push_back(20);
  myvector.push_back(30);

  std::cout << "myvector contains:";
  for_each (myvector.begin(), myvector.end(), myfunction);
  std::cout << '\n';

  // or:
  std::cout << "myvector contains:";
  for_each (myvector.begin(), myvector.end(), myobject);
  std::cout << '\n';

  return 0;
}
// output:
// myvector contains: 10 20 30
// myvector contains: 10 20 30
```

### `std::next`, `#include <iterator>`

```c++
template <class ForwardIterator>
  ForwardIterator next (ForwardIterator it,
       typename iterator_traits<ForwardIterator>::difference_type n = 1);
```

Returns an iterator pointing to the element that it would be pointing to if advanced n positions.

it is not modified.

```c++
// next example
#include <iostream>     // std::cout
#include <iterator>     // std::next
#include <list>         // std::list
#include <algorithm>    // std::for_each

int main () {
  std::list<int> mylist;
  for (int i=0; i<10; i++) mylist.push_back (i*10);

  std::cout << "mylist:";
  std::for_each (mylist.begin(),
                 std::next(mylist.begin(),5),
                 [](int x) {std::cout << ' ' << x;} );

  std::cout << '\n';

  return 0;
}
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
