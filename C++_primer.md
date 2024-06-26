# C++ Primer

## Chapter 2. Variables and Basic Types

### 2.1.1 Arithmetic Type

#### C++ Arithmetic Type

| Type        | Meaning                           | Minimum Size          |
| :---------- | --------------------------------- | --------------------- |
| bool        | Boolean                           | NA                    |
| char        | character                         | 8 bits                |
| wchar_t     | wide character                    | 16 bits               |
| char16_t    | Unicode character                 | 16 bits               |
| char32_t    | Unicode character                 | 32 bits               |
| short       | short integer                     | 16 bits               |
| int         | integer                           | 16 bits               |
| long        | long integer                      | 32 bits               |
| long long   | long integer                      | 64 bits               |
| float       | single-precision floating-point   | 6 significant digits  |
| double      | double-precision floating-point   | 10 significant digits |
| long double | extended-precision floating-point | 10 significant digits |



#### Signed and Unsigned Type

For char, there are three distinct basic character types: `char`, `signed char`, and `unsigned char`, but there're only two representations, that is either signed or unsigned. The char type used depends on the compliers. `char` is signed on some machines and unsigned on others. Therefore, if it is required to use `char`, make sure it is specified explicitly either `signed char` or `unsigned char`.

`int, short, long and long long` are all signed. By adding `unsigned` to the type, we could obtain the corresponding unsigned type.



### 2.1.2 Type Conversions

As we assign a value of one type to an object of another type, following things would happen:

- Assign non-bool to bool, the result is false if value is 0 and true otherwise.
- Assign a floating-point value to an object of integral type, the value stored is the part before the decimal point.
- Assign an out-of-range value to an object of unsigned type, the result is the remainder of the value assigned modulo the maximum number of value this type can hold, e.g. 
  `unsigned char a = 300; // a = 300 % 256 = 44`
- Assign an out-of-range value to an object of signed type, the result is **undefined**.

#### Expressions involving both signed value and unsigned value

As we both use unsigned and signed int values in an arithmetic expression, the signed int value would ordinarily be converted into the unsigned value, following the same process as we assign an out-of-range value to an unsigned type. 

```c++
unsigned int u = 10;
int i = -42;
unsigned int u2 = -32; 

cout << u + i << endl;   // 4294967264
cout << u2 % 4294967295 << endl; // 4294967264
```

#### Unsigned integer in loop

It is wrong to use an unsigned integer in the for loop, since if the unsigned number could never become less than 0, leading to the unterminated condition. 

```c++
for (unsigned int i = 10; i >= 0; i--)  // Don't allow
	cout << i << endl;

for (unsigned int i = 10; i > 0; i--)   // Allow
	cout << i << endl;
```

In conclusion, **don't mix signed and unsigned in arithmetic expression** since the result of the expression depends on how many bit an int has on that particular machine. 

### 2.1.3 Literals

A value is known as a **literal**, such as 42. 

#### Escape sequences

Some characters are nonprintable, others have special meaning in our languages, therefore, we can't use them directly. Instead, we use the Escape sequences to represent those characters. E.g. \n, \t, \v, and so on.



### 2.2.1 Variables

A variable definition consists of a **type specifier** with one or a list of variable names.

```c++
int book_count;z
```

#### Initializers

An object that is **initialized** gets the specified value at the moment it is created.  (Value + first created)

> **Initialization** isn't assignment, it happens when a variable is given a value when it is created. Assignment obliterates an object's current value and replaces that value with a new one.



#### List Initialization

Four different ways could be used to initialize a list shown below:

```c++
int units_sold = 0;
int units_sold = {0};
int units_sold{0};
int units_sold(0);
```

One important property of list initialization is that the compiler will not let us to list initialize a variables of built-in type if the initializer might lead to the loss of information.

编译器会报错：Error: Narrowing conversion required 需要收缩转换



#### Default Initialization

As we define a variable without an initializer, the variable is **default initialized.** The default value of **built-in** type depends on where the variable is defined:

- Defined outside the function body are initialized to zero.
- Variables of built-in types defined inside the function body are **uninitialized**. The value of them is **undefined**, which is an error when try to use or copy.

For objects of class type we do not explicitly initialize have a value that is defined by the class.



### 2.2.2 Variable Declarations and Definitions

A declaration makes name known to the program. 

*A file that wants to use a name defined elsewhere includes a declaration for that name.*

A definition creates the associated entity, which **allocates storage** and may **provide the variable with an initial value.**  (内存分配 + 数据初始化)

A variable declaration specifies the name and type of a variable, which is a variable definition. **Variable declaration = Variable definition**. 

To use the same variable in multiple files, we must **separate the definition and declaration of variables**.

```c++
extern int i;   // declares but does not define i
int j;          // declares and defines j
```

**Any declaration  that includes an explicit initializer is a definition.**

对于上面的例子而言，如果为`extern`变量提供initializer，那么就会override `extern`. `extern`关键字不能被用在函数体里面。

#### Declarator

It refers to the one that comes after the base type of the variable in declaration.

> 为了保证程序不会导致undefined error，需要initialize every object of built-in type.

一个变量可以被declare很多次，但只能被define一次。



### 2.2.3 Identifier

An identifier must begin with either a letter or an underscore.



### 2.2.4 Scope of a Name

#### Nested Scopes

The contained scope is referred to as an **inner scope**. The containing scope is referred to as the **outer scope**.

A name that is declared in a scope, which could be used by nested scopes. Name in the outer scope could be redefined in an inner scope. 注意此处并不是重写，而是在另一个内存空间定义了一个变量，并把该地址associate到这个名字上去。也因此redefined name in inner scope has a different memory address with the name defined in the outer scope.



### 2.3.1 References

A compound type is a type that is defined in terms of another type. 

> Reference一般说的都是 "lvalue reference".

A reference defines an alternative name for an object. We define it by writing the declarator in the form of `&d`. It is important to notice that as we declare a reference, it need to be initialized, otherwise it will lead to errors.

```c++
int ival = 2024;
int& refVal = ival;
int& refVal2; // Error
```



#### Reference Definitions

A reference may be bound only to an object, not to a literal or to the result of a more general expression:

```c++
int &refVal4 = 10;  //  error: initializer must be an object
double dval = 3.14;
int &refVal5 = dval; // error: initializer must be an int object
```



#### Comparison with ordinary variable declaration

Compared to the ordinary variable declaration, we actually bind the reference to the initializer. For ordinary variable, the value of the initializers is copied into the object we're creating.  "="是在做initializing，右边传递的是initializer.

- As we assign to a reference, we're assigning to the object to which the reference is bound.
- As we fetch the value of a reference, we're fetching the value of the object to which the reference is bound. 

注意，Once initialized, a reference can't be rebound to a different object.

```c++
int i = 0;
int j = 40;
int &refval = i;
refval = j; // not rebinding, but assigning the data of j to i, leading to i = 40
```



### 2.3.2 Pointer

A pointer is a compound type that points to another type. Unlike the reference, it is an actual objects, and **doesn't need to be initialized**. 



A pointer holds the address of another object. We use **(address-of operator) &** operator to get the address of the object.

```C++
int ival = 42;
int *p = &ival;
```

> Reference is not an object, thus, we can't define a pointer to reference.



#### Pointer Value

The values of a pointer can be in one of the four states:

- Point to an object
- Point to the location just immediately past the end of an object.
- Null pointer
- Invalid (other the preceding three above)

#### Dereferencing a pointer

We can dereferencing a pointer by using dereference operator *.

```C++
int ival = 42;
int *p = &ival;
cout << *p << endl; // yields the object to which pointer p points

*p = 0;
cout << *p << endl; // output: 0
```

> Dereference only works with the valid pointer.



#### Other pointer operations

We can use pointer in a condition. As long as it is not a `NULL` or `nullptr`, then it will return true, otherwise false.

#### void* Pointers

The type void* pointers can hold the address of any object, but its type is unknown.



#### Reference to Pointers

Since Pointers is an object, we can use reference to refer a pointer.

```C++
int* p = &ival;
int*& r = p;
cout << r << endl;
cout << *r << endl;
```



### 2.3.3 Compound Type Declarations

注意，* modifies 变量名的类型，而并不是前面的数据类型，例如:

```c++
int* p; // legal but might be misleading
int* p1, p2;  // p1 is a pointer; p2 is an int
int *p1, *p2; // both p1 and p2 are pointers to int
```

所有的变量名都会share共同的base type！



### 2.4 const qualifier

We can make a variable unchangeable by defining the variable's type as const:

#### How does it work?

```C++
const int bufsize = 512; // input buffer size
```

The const variable is local to a file. At compiling stage, the compiler would automatically convert all const variable to its data, that is to substitute `bufsize` to literal 512. Therefore, a variable defined as a const is essentially an ***rvalue***.



#### Initialization and const

We could use all operations that cannot change an object. Whenever we use operations that don't change the value of the object, it doesn't matter whether either or both of the objects are const as we use an object to initialize another object.

```c++
int i = 42;
const int ci = i; // the value of i is copied into ci, therefore the new object has no further access to the original object.
int j = ci;
```



#### Split into multiple file

Sometimes we want a const variable be shared in multiple files without declaring it for multiple times. To achieve this, we define the const using the keywork extern in one file, and declare it in another file. 

```c++
// file1.cpp
extern const int bufSize = fcn();
// file1.h
extern const int bufSize;
```

In this case, file1.cpp defines and initializes `bufSize`. The extern signifies that `bufSize` is not local to this file.



### 2.4.1 References to const

Technically speaking, it is references to const, not const reference. 

A reference to const cannot be used to change the object to which the reference is bound.



#### Initialization and references to const

There're two exception to the rules for the const:

1. It is possible to bind a reference to const to non-const object, a literal, or a more general  expression:
2. This would be covered later.

```c++
int i = 42;
const int &r1 = i;      // we can bind a const int& to a plain int object
const int &r2 = 42;
const int &r3 = r1 * 2;
int &r4 = i * 2;        // error: r4 is a plain, non-const reference
```

Another scenario is that

```c++
double dval = 3.14;
const int &ri = dval;
```

The reason why this would work is that the compiler would transform the code into something like:

```c++
const int temp = deval;
const int &ri = temp;
```

The compiler would automatically create a temporary object, thus allowing a reference to a const be able to accept the non-const object, literal or a more general expression.

总结来说，使用reference to a const会有两种情况：

1. Initializer是一个non-const object (lvalue)，直接绑定，这里的const相当于是在绑定以后，我不能通过这个reference对这个object进行修改，但是在对object修改的时候，reference的值会被修改. 其指向的对象不一定非得是const.
2. Initializer是其他类型，也就是rvalue (右值)，包括literal, general expression和其他类型的Initializer，此时，编译器会创建一个临时的const对象，然后将reference绑定到这个对象上.





### 2.4.2 Pointers and const

#### Pointer to const

We could store the address of a const object only in a pointer to the const. 我们可以修改指针指向的地址，但是不能修改指针所指向的对象. 

```c++
const double pi = 3.14f;
double *ptr = &pi;          // error: ptr is a plain pointer
const double *cptr = &pi;   // ok
*cptr = 442;                // error: cannot assign to *cptr
```

同样，如Reference to a const，指向的对象不一定非要是const，将他定义为const只是会影响我们能够对它做什么:

```c++
double dval = 3.14;
cptr = &dval;
```



#### const Pointers

We indicate that a pointer is const by putting the const after the *. Notice that it should be put after the declaration of type. 

```C++
int errNum = 2;
int* const curErr = &errNum;
const double pi2 = 3.14;
const double *const pip = &pi2; // const double is the base type of the pointer

if (*curErr) {
	ErrorHandler();
    *curErr = 0;
}
```

We can't change the address that a const pointers point to, but we can change the value that it point to. (不能改变指针，但是可以修改指针所值的对象的值，让修改变得更加安全). 指针指向这里就永远不变了。 

**const pointers must be initialized.**



### 2.4.3 Top-Level const

We use term `top-level const` to indicate the pointer(object) itself is a const. When a pointer can point to a const object, we refer to that const as a `low-level const`.

More generally, `top-level const` indicates that an object itself it const

(Top-level 和 low-level const的差别在于 low-level const 代表的是base type，所以它重要，但top-level并不是，仅仅是const pointer，所以在copy操作时，要关注low-level const，如果二者有不同的low-level，则无法进行copy操作).

如果不是指针类型，那么前面包含const的话，则改表达式前的const为top-level const.

```c++
const int ci = 42; // const here is a top-level const
const int *ciptr = &ci; // const here is a low-level const
```

在进行copy的时候，both objects must have the same low-level const qualification or there must be a conversion between the type of the two objects.

程序允许右边从non-const变成const，但是不允许左边从non-const变成const，例如：

```c++
int i = 0;
const int* ptr = &i;
int* p2 = ptr;  // error: ptr has a low-level const, but p2 doesn't
```



### 2.4.4 constexpr and Constant expression

A constant expression is an expression whose value cannot change and that can be **evaluated at compile time**.

```C++
const int max_files = 20;
const int limit = max_files + 1;  // it is a constant expression
const int sz = get_size(); // sz is not a constant expression
```

The reason why `sz` is not a constant expression is that we can only know the value of `get_size()` at runtime even though it is constant.

#### constexpr Variables

We could ask a complier to verify that a variable is a constant expression by declaring the variable in a `constexpr` declaration. Variables declared as `constexpr` are implicitly const and must by **initialized by constant expressions**. 



#### Literal Types

A literal type are type that could be used in a `constexpr`. The arithmetic, reference and pointer types are literal types. 



#### Pointers and constexpr

It is important to notice that when we define a constexpr, the constexpr specifier applies to the pointer instead of the type to which the pointer points.

```c++
const int *p = nullptr;   // p is a pointer to a const int.
constexpr int *q = nullptr; // q is a const pointer to int
```

`constexpr` imposes a **top-level const** on the object it defines. 

A `constexpr` can also point to a const or a non-const type:

```c++
const int i = 10;  // It must be defined out of any function
int j = 12;

constexpr const int *q = &i; // q is a const pointer points to the const int i
constexpr int *p = &j; // p is a const pointer points to int j
```

注意，由于variables defined inside a function are not stored at fixed address，所以我们无法用constexpr的pointer来指向这样的变量。而通过使用static关键字则可以实现定义的variable拥有fixed address.



### 2.5.1 Type Aliases

A type alias is a name that is a synonym for another type, which allows us to simply complicated type definitions. Traditionally, we use a `typedef` to define a type alias. 	

```c++
typedef double wages; // wages is a synonym for double
```

C++ 11 introduces a new way to define a type alias, via an alias declaration:

```
using ll = long long;
```

#### Pointers, const and Type Aliases

As declaration use type aliases to represent the compound type, the result would be surprising:

```c++
typedef int* ipt;
const ipt cipt = 0; // cipt is a constant pointer to int. 
```

In this case, **the const means it is a constant pointer not a pointer to const int.** Therefore, it would be incorrect substituting the aliases with its corresponding type.

### 2.5.2 The auto Type specifier

auto tells the complier to deduce the type from the initializer, and therefore as we declare a variable with auto specifier, we must have an initializer. 

```c++
auto item = val1 + val2;
```

#### Compound Types, const and auto

As using reference as the initializer, the compiler would then use the object's type for auto's type deduction.

```c++
int i = 0, &r = i;
auto a = r; // a is an int, not a reference.
```

The initialization would automatically drop the top-level const.

```c++
const int ci = i, &cr = ci;
auto b = ci; // b is an int (since the top-level const is dropped)
auto c = cr; 
auto d = &i; // d is an int*
auto e = &ci; // e is const int* (low-level) since the object itself is a pointer
```

If we want the deduced type to have a top-level const, we must say so explicitly:

```c++
const auto f = ci;
```

However, when it comes to a reference to an auto-deduced type, ***top-level const of the initializers*** are never ignored. 而他们在被绑定到reference以后，就不是top-level了.

```c++
auto k = ci, &l = i; // k is int, l is int&
auto &m = ci, *p = &ci;  // m is a const int&, p is a pointer to a const int.
```



### 2.5.3 The decltype Type specifier

To define a variable with a type that the complier deduces from an expression, but don't want to use that type to define the variable, we could use a type specifier, `decltype`, which returns the type of its operand(运算对象).

```c++
decltype(f()) sum = x; // sum has the type of what decltype return.
```

#### decltype and references

暂缓。要介绍左值 (P.135) 参考后面内容



## Chapter 3. Strings, Vectors, and Arrays

### 3.1 Namespace using Declarations

#### Headers Should not include using Declarations

Code inside headers ordinarily should not use using declarations since all code in the header file would be copied into the including program's text. As a result, if there's a using declarations in headers, then every program that includes those headers would gets those  using declarations leading to some name conflicts.



### 3.2.1 Defining and Initializing strings

Common ways to initialize strings include:

```c++
string s1;
string s2 = s1;
string s3 = "hello";
string s4(10, 'c')   // s4 is cccccccccc
```

#### Direct and Copy Form of initializations

Different forms of initializations above differ from each other.  Here're two ways: Direct and copy initialize.

- While using `=` to initialize, we're asking compilers to **copy initialize** the object by copying the initializer on the right hand side.
- **Direct initialization** is to initialize in the way of what `s4` does.

If we initialize a variable from more than one value, we must use the direct form of initialization.



### 3.2.2 Operations on strings

#### Reading and Unknown Number of strings

```c++
int main() 
{
	string word;
	while (cin >> word) 
		cout << word << endl;
	return 0;
}
```

#### Using getline to Read an Entire Line

```c++
int main()
{
	string line;
	while (getline(cin , line)) {
		cout << line << endl;
	}
}
```

What the `getline` method return is `istream`. For condition, if the input line is valid, then return true. If it is invalid, then return false and end the loop.

#### The string::size_type Type

One member method of string is called `size()`. It might be logical to expect its return type is `int`. However, in this case, the return type is `size_type`.

Like other libraries, string class would define several companion types, which makes it possible to use those types in a machine-independent manner. The `size_type` is an `unsigned` type. We can get access to it by using `string::size_type`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  

> While using size(), avoid use int together to avoid the conversion of signed and unsigned int.



#### Comparing strings

String comparison following two cases if they're different:

1. Two strings of different lengths and every character in the shorter string is equal to the corresponding character of the longer string, then the shorter string is less than longer string.
2. If two strings differ at corresponding characters, then the result is the comparison of the first character at which two strings differ. 

```c++
string str1 = "Hello World";
string str2 = "Hiya" // str2 > str1
```

#### Adding Literals and strings

When we mix strings and strings or character literals, at least one operand to each +operator must be of string type.

```c++
string s2 = s1 + "Hello";  // ok
string s3 = "Hello" + ",";  // Error: no string operand
string s4 = s1 + "," + "Hello"; // ok, each + has a string operand
string s5 = "hello" + "," + s4;  // error: can't add string literal
```

注意这里的“hello”不是字符串，是字符数组.



### 3.2.3 Dealing with the Characters in a string

#### Processing every character

To do something with every character in the string, we can use a statement in the new standard: the range-for statement.

```c++
string str("hello world");
for (auto c : str) 
    cout << c << endl;
```

Here're some of the functions in `cctype` header that do operation to each character in a string:

- `issalnum(c)`
- `isalpha(c)`
- `iscntrl(c)`
- `isdigit(c)`
- ...



> In C++, every C library of C++ version in the namespace of standard library will begin with letter c. For example, in this case, the original C library is called ctype.h, while the C++ version is called cctype.



#### Using a Range for to Change the characters in a string

Here's the example code to convert the whole string to upper letter using reference.

```c++
string s("Hello World");
for (auto &c : s) 
	c = toupper(c); // since c is a reference, so the assignment changes the char in s.
cout << s << endl;
```





#### Processing only some characters

There're two ways to access each individual characters in a string: subscript or an iterator.

The subscript operator [] takes a `string::size_type` value. This operator would return a **reference** to the character at the given position.



#### Using a Subscript for Iteration

```c++
for (decltype(str.size()) index = 0; index != str.size() && !isspace(str[index]); ++index)
	str[index] = toupper(str[index]);
```



### 3.3 Library vector Type

A vector is a class template. The process that the compiler uses to create classes or functions from templates is called **instantiation**. 

> Vector is a template, not a type. Type generated from vector must include the element type, for example, `vector<int>`.

Since reference is not an object, we cannot have a vector of references.



#### Defining and initializing vectors

We can default initialize a vector, which creates an empty vector of the specified type:

```c++
vector<string> svec;
```

We can also supply initial values by copying elements from another vector:

```c++
vector<int> ivec;
vector<int> ivec2(ivec);    // copy
vector<int> ivec3 = ivec;   // copy
vector<string> svec(ivec2); // error: svec holds strings, not int
```



#### List Initializing a vector

A vector could be list initialized by a list of zero or more initial element values enclosed in **curly braces**:

```c++
vector<string> articles = {"a", "an", "the"};
```

We cannot use **parentheses** to initialize the vector. 

```c++
vector<string> v1{"a", "an", "the"};
```



#### Creating a Specified Number of Elements

Two parameter for this way of initialization required: count of element and element value.

```c++
vector<int> ivec(10, -1);
vector<string> svec(10, "hi!");
```



#### Value Initialization

It is possible to supply only a size while omitting the value. The library creates a value-initialized element initializer for us, which is used to initialize each element in the container.

```c++
vector<int> ivec(10);
vector<string> svec(10);
```



#### List Initializer or Element Count?

1. 只有在用curly braces的时候，才会用list initializer.
2. 如果里面的参数和vector的element type不一样，那么就不会考虑list initializer，而是转向element count.

```c++
vector<string> v1{10};        // vector with 10 empty string
vector<string> v2{10, "hi"};  // vector with 10 "hi"
vector<string> v3{"hi"};      // vector with 1 element of "hi"
```



#### 3.3.2 Adding elements to a vector

```c++
vector<string> text;
while (cin >> word) {
	text.push_back(word);
}
```



#### 3.3.3 Other vector Operations

The `size_type` of vector could only be accessed using following:

```c++
vector<int>::size_type // ok
vector::size_type      // error
```

For the relational operators, if the size of two vector is different, then the vector with fewer elements is less than the one with more elements. If the elements have differing values, then the relationship is determined by the relationship between the first elements that differ.

*We can compare two vectors only if we can compare the elements in those vectors.*



### 3.4 Introducing Iterators

All containers have iterators, but not all of them support the subscript operation.

#### 3.4.1 Using Iterators

Containers have members named `begin` and `end`, which return iterator.

```c++
auto b = v.begin(), e = v.end();
```

The begin member denotes the first element in the container. The end member denotes the one past the end of the container. If the container is empty, then `begin` return the same iterators as the one returned by `end`.

##### Iterator Operations

1. *iter ➡️ Returns a reference to the element denoted by the iterator iter.
2. iter->mem ➡️ Dereferences iter and fetches the member named mem from the underlying element. Equivalent to (*iter).mem.
3. ++iter ➡️ Increments iter to refer to the next element in the container.
4. --iter ➡️ Decrements iter to refer to the previous element in the container.
5. iter1 == iter2 ➡️ Compares two iterators for equality.
6. iter1 != iter2 ➡️ If they're not the same element or if the container is not empty.

```c++
string s("some string");
if (s.begin() != s.end()) {
    auto it = s.begin();
    *it = toupper(*it);
}
```



##### Moving Iterators from one element to another

Iterators use the increment (++) operator to move from one element to the next.

```c++
for (auto it = s.begin(); it != s.end() && isspace(*it); ++it) {
    *it = toupper(*it);
}
```



##### Iterator Type

There's generally two type of iterator, one is `iterator`, the other is `const_iterator`. The latter behaves like a const pointer, **only allowing read** the element from the containers. While the former one allows **both read and write**. 

```c++
vector<int>::iterator it;
string::iterator it2;
vector<int>::const_iterator it3;
string::const_iterator it4;
```

The new standard introduced two new functions named `cbegin` and `cend` to let use `const_iterator` type.

```c++
auto it3 = v.cbegin();  // it3 has type vector<int>::const_iterator
```

> For now, it is important to know that loops that use iterators should not add elements to the container to which the iterators refer.



#### 3.4.2 Iterator Arithmetic

All the library containers have iterators that support arithmetic operation with integer:

1. iter + n ➡️ returns an iterator that the `iter` moves n elements forward within the container. 
2. iter1 - iter2  ➡️ returns the number that when added to the right-hand iterator yields left-hand iterator.

To compute the middle of a vector, we can use following operation.

```c++
auto mid = vi.begin() + vi.size() / 2;
```

 

### 3.5 Arrays

#### 3.5.1 Defining and Initializing Built-in Arrays

Arrays are a compound type, of which the declarator has the form `a[d]`. d is the dimension of the array, which must be known at compile time, which means the **dimension must be a constant expression**. 常量类型的变量初始化发生在runtime时，因此无法实现。

```c++
unsigned cnt = 42;
constexpr unsigned sz = 42;

int arr[10];
int *parr[sz];
string bad[cnt]j;
string strs[get_size()];
```

> A default-initialized array of built-in type that is defined inside a function will have a undefined values.

 

##### Explicitly initializing array elements

We can list initialize the elements in the array. It is possible to omit the dimension, then the compiler would infer the dimension from the number of initializers. If we then specify a dimension as well, the number of initializers must not exceed the specified size. If the dimension is greater, then the initializers are used for the first elements and any remaining elements are value default-initialized.

```c++
int arr1[] = {0, 1, 2};
int arr2[10] = {0, 1, 2};
```



##### Character Arrays

Character arrays have an additional form of initialization. It is possible to initialize arrays from a string literal. 使用它的时候，数组的长度必须要多一个位，用于放null character.

```c++
const char a1[6] = "Daniel"; // error: no space for the null
const char a2[7] = "Daniel"; // ok
```



##### No copy or Assignment

We cannot initialize an array as a copy of another array, nor it is legal to assign one array to another.

```c++
int a[] = {1, 2, 3};
int a2[] = a;
a2 = a; 
```

> Some compilers allow array assignment as a compiler extension. It is usually a good idea to avoid such non-standard feature to be able to work across different compiler.



##### Other Complicated Array Declarations

We could define an array of pointers. Since array is an object, it is also possible to define both pointers and references to array.

```c++
int *ptrs[10];               // ptrs is an array of ten pointers to int
int &refs[10];               // error: no arrays of references (ref is not an object)
int (*Parray)[10] = &arr;	 // Parray points to an array of 10 ints
int (&arrRef)[10] = arr; 	 // arrRef refers to an array of 10 ints
```

因为Array的Declarator是`a[d]`的形式，所以从内部往外看可以让其更容易理解。例如对于Parray而言，在parentheses里面是一个pointer，右边是size of ten，所以是一个pointer指向an array of size 10. 向左看，array的每一个element是int.

注意，dimension一定要加上.

 

#### 3.5.2 Accessing the Elements of an Array

When we use a variable to subscript an array, we normally should define a variable to have a type `size_t`. It is a machine-specific unsigned type that is guaranteed to be large enough to hold the size of any object in memory.

The difference between the subscript used in array and vector is that the subscript operator in array is defined as part of the language itself, while the subscript of vector is defined in the library.



#### 3.5.3 Pointers and Arrays

Pointers and arrays are closely intertwined.

```c++
string nums[] = {"one", "two", "three"};
string *p = &nums[0];
```

Now, we could get access to the pointer to the first element in the `nums` array. However, arrays have a special property - in most places, the compiler would substitutes a pointer to the first element:

```c++
string *p2 = nums;  // equivalent to p2 = &nums[0];
```

One implications of this usage is that when we use an array as an initializer for a variable defined using auto, the deduced type is a pointer, not an array.

```c++
int ia[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
auto ia2(ia);
ia2 = 42;   // error: ia2 is a pointer, we can't assign an int to a pointer.
```

The compiler would treat the initializer we wrote as follows:

```c++
auto ia2(&ia[0]);
```

However, this conversion does not happen when we use decltype. 

```c++
decltype(ia) ia3 = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
ia3 = p;        // Error: can't assign int * to an array
ia3[4] = 1;            
```



##### Pointers are Iterators

Pointers to array elements support the same operations as iterators on vectors or strings. 

Increment Operators:

```c++
int arr[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
int *p = arr;
++p;
```

Traverse the elements in an Array:

```c++
int *e = &arr[10];  // Obtaining the last nonexistent element in the array
for (int *b = arr; b != e; ++b)
    cout << *b << endl;
```



##### The Library begin and end Functions

Since computing an off-the-end pointer is error-prone, the new library includes two functions, named `begin` and `end`. Those two functions are not member functions. 

```c++
int ia[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
int *beg = begin(ia);   // pointer to the first element in the array
int *last = end(ia);    // pointer to the last element in the array
```

Those two functions are defined in the `iterator` header.

```c++
int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
int* pbeg = begin(arr), * pend = end(arr);

for (; pbeg < pend; ++pbeg) {
	cout << *pbeg << endl;
}
```



##### Pointer Arithmetic

Pointers that address the array elements can use all the iterator operations. 

```c++
constexpr size_t sz = 5;
int arr[sz] = { 1, 2, 3, 4, 5 };
int* ip = arr;
int* ip2 = ip + 4;   // ip2 points to arr[4], the last element in arr
```

When we add `sz` to `arr`, the compiler converts `arr` to a pointer to the first element in `arr`. When we add `sz` to that pointer, we get a pointer that points `sz` positions past the first one.

```c++
int *p = arr + sz; // do not dereference -> undefined value.
```

Subtracting two pointers gives us the distance between pointers. The pointers must point to elements in the same array:

```c++
auto n = end(arr) - begin(arr);  // the type is called ptrdiff_t type in cstddef header
```



##### Subscripts and Pointers

When we uses the name of an array, we are really using a pointer to the first element in that array. One place where the compiler does this transformation is when we subscript an array:

```c++
int ia[] = {0, 2, 4, 6, 8};
int i = ia[2];
// Equivalent to the following code:
int *p = ia;
i = *(p + 2);
```

It is also possible to use a negative value.

```c++
int *p = &ia[2];
int j = p[1];

int k = p[-2];
```



#### 3.5.4 C-Style Character Strings

C-Style string are not a type. Instead, they're the convention of how we represent and use character strings. String following this convention are stored in character arrays and are **null terminated**, which means the last character in the string is followed by a null character (`'\0'`).

```c++
#include <cstring>
#include <iostream>
using namespace std;

int main() {
    char ca[] = {'C', '+', '+'};
   	cout << strlen(ca) << endl;  // ca isn't null terminated, producing undefined value.
    char ca_correct[] = {'C', '+', '+', '\0'};
    cout << strlen(ca_correct) << endl;  // print 3.
}
```



##### Comparing Strings

When we compare two library strings, we use the normal relational or equality operators. But in C-style strings, it would lead to undefined error.

```c++
string s1 = "Hello";
string s2 = "World!";
if (s1 < s2) {
    cout << "Great" << endl;
}

// C-Style String
const char ca1[] = "A string Example";
const char ca2[] = "A different String";
if (ca1 < ca2)  // undefined: compares two unrelated addresses
    
if (strcmp(ca1, ca2) < 0)
```

因为使用array就代表着使用指针，因此这里的条件实际是比较两个const char* values. 所以无法进行大小比较.



##### Caller is Responsible for Size of a Destination String

Concatenating or copying C-style String is also different from the same operation on library strings.

```c++
string largeStr = s1 + "" + s2;

// In C-Style String
// Disastrous if we miscalculated the size of largeStr
strcpy(largeStr, ca1);
strcat(largeStr, " ");
strcat(largeStr, ca2);
```



#### 3.5.5 Interfacing to Older Code

As using the library string type in the program, sometimes it has to interface to programs written in C. For example, there's no way to initialize a character pointer from a string. There is a member function  named `c_str` that can accomplish this.

```c++
string s("Hello World");
char *str = s;   // error: Can't initialize a char* from a string
const char *str = s.c_str();
```

由于`c_str()`返回的是一个`const char*`，因此如果要继续使用，则需要复制一份数据，否则会影响到`s`的内容。

 

### 3.6 Multidimensional Arrays

#### Pointers and Multidimensional Arrays

> When you define a pointer to a multidimensional array, remember that a multidimensional array is really an array of arrays.

The pointer type to which the array converts is a pointer to the first inner array:

```c++
int ia[3][4];
int (*p)[4] = ia;
p = &ia[2];
```

注意，这里的p是指向inner array的指针，inner array的size是4，因此定义的时候其大小为4.

在C++ 11标准下，我们可以使用auto关键字来实现遍历的过程。

```c++
for (auto p = ia; p != ia + 3; ++p) {
	for (auto q = *p; q != *p + 4; ++q)
		cout << *q << ' ';
	cout << endl;
}
```



## Chapter 4. Expressions

### 4.1 Fundamentals

#### 4.1.1 Basic Concepts

In C++, there're both **unary operators**(一元运算符) and **binary operators**(二元运算符). 

##### Lvalues and Rvalues

Every expression is C++ is either an rvalue or an lvalue. When we use an object as an rvalue, we use the object's value. When we use an object as an lvalue, we use the object's identity (its location in memory).

We can use an lvalue when rvalue required, but cannot use an rvalue when lvalue (location) required. 

For example:

1. Assignment需要lvalue在它左边，而生成的结果也是一个lvalue，在左边。
2. The address-of operator requires an lvalue operand and returns a pointer to its operand as an rvalue.
3. The built-in dereference and subscript operators and the iterator dereference all yield lvalues.

当使用decltype的时候，二者不同。When we apply decltype to an expression that yields lvalue(注意这里不能是variable), the result is a reference type. 例如:

```
decltype(*p) -> int& // p is a pointer to integer.
```

另外，如果decltype里面是一个rvalue, 比如取地址符，那么decltype产生的则是pointer to pointer.

```
decltype(&p) -> int** // p是指针，p的地址需要另一个指针来存储
```



### 4.2 Arithmetic Operators





### 4.5 Increment and Decrement Operators

There are two forms of these operators: Prefix and postfix. 

The prefix form increments its operand and yields the changed object as a result.

The postfix form increment the operand but yields a copy of the original, unchanged value as its result.

```c++
int i = 0, j;
j = ++i;  // i=1, j=1  Prefix form
j = i++   // i=2, j=1  Postfix form
```



##### Combining Dereference and Increment in a single Expression

```c++
auto pbeg = v.begin();
while (pbeg != v.end() && *beg >= 0)
	cout << *pbeg++ << endl;
```

注意，此处使用了postfix form of increment operator，也就是说`*pbeg++`的意思是去对iterator的数据+1，但其返回的内容是original data. 因此相当于打印当前迭代到的数据，并进行下一次迭代。由于Increment operator的precedence要比dereference operator要高，因此该语句相当于 `*(pbeg++)`.



#### The Operands can be evaluated in any order

由于标准库并未定义operand执行的顺序，

因此某些情况下会出现错误

To be continued...



### 4.8 The Bitwise Operators

由于对signed value的操作是machine dependent的，因此在使用bitwise operators的时候最好使用unsigned类型。

##### Bitwise shift Operators

`>>` 将bit向右移动一定数量的单位

`<<` 将bit向左一定一定数量的单位

The behavior of the right-shift operator depends on the type of the left-hand operand. 如果其类型是unsigned，则保持不变，而如果类型是signed，其实现是由编译器所决定的，要么和unsigned类型的操作一样，要么会保留原始的符号位。



##### Bitwise Not Operator

The bitwise NOT operator generates a new value with the bits of its operand inverted. 每个1变成0，每个0变成1.





## Chapter 6. Functions

### 6.1 Function Basics

A function definition consists of a return type, a name, a list of zero or more parameters. The action that the function performs are specified in a statement block, referred to as the **function body**. 

When execute function through the **call operator**, which is a pair of parentheses. Inside the parentheses is a comma-separated list of **arguments**. 

##### Calling a function

A function call does two things: It initialize the function's parameters from the corresponding arguments, and it transfers control that function. Execution of the calling function is suspended and execution of the called function begins. 

##### Parameters and Arguments

Arguments are the initializers for a function's parameters. Every arguments initializes each corresponding parameters.

##### Function Parameter List

We can define a function with no parameters either implicitly and explicitly:

```c++
void f1() { }
void f2(void) { }
```

> **Outermost scope指代的是函数体**



#### 6.1.1 Local Objects

Every objects have lifetimes and every names have scope.

- The scope of a name is party of the program's text in which that name is visible.
- The lifetime of an object is the time during the program's execution that the object exists.

Parameters and variables defined inside a function body are referred to as **local variables**. Their declaration are hide in an outer scope. **The lifetime of a local variable depends on how it is defined**.



##### Automatic Objects

对于一个local variables而言，当control到它的定义时，the object corresponding to the local variables are created. 而当control passes through the end of the block，local variables就被销毁了.

Objects that exist only while a block is executing are known as **automatic objects**. After the execution of that block, the values of the automatic objects created is **undefined**.

Automatic objects corresponding to local variables are initialized if their definition contains an initializer. Otherwise, they are **default initialized**. (注意，除去built-in type的值是uninitialized) (P.43)



##### Local `static` Object

We obtain the objects correspond to a local variable whose lifetime continues across calls to the function by defining a local variable as `static`.

*Each local static object is initialized before the first time execution passes through the object's definition. Local statics are not destroyed when a function ends. They are destroyed when the program terminates.* local static object只初始化一次，例：

```c++
size_t get_count() {
    static size_t count	= 0;
    return count++;
}

int main() {
    get_count();  // 初始化count为0
    get_count();  // 不会再次初始化count
    
    return 0;
}

```



#### 6.1.2 Function Declaration

The name of a function must be declared before we can use it. A function may be defined only once but may be declared multiple times. 
Declaration没有函数体，但是definition有函数体. Function declarations are also known as the **function prototype.**



##### Function Declarations Go in Header Files

Functions should be declared in header files and defined in source files. The source file that defines the function should include the header file. That way the compiler will verify that the definition and declaration are consistent.



#### 6.1.3 Separate Compilation

C++ support what is commonly known as separate compilation. Separate compilation let us split our programs into several files, each of which can be compiled independently.

The compiler lets us ***link*** object files together to form an executable.



### 6.2 Argument Passing

> Parameter Initialization works the same way as variable initialization.

#### 6.2.1 Passing arguments by Value

When we initialize a nonreference type variable, the value of the initializer is copied. Changes made to the variable have no effect on the initializer:

##### Pointer Parameters

Pointers behave like any other nonreference type. If we simply pass another pointer to the function, then the value of the pointer is copied. After the copy, the two pointers are distinct. 

```c++
int n = 0, i = 42;
int *p = &n, *q = &i;
void reset(int *ip) {
	int i = 0;
	ip = &i;
}

reset(p); // p would still stay the same
```

However, when changing the value of the object to which p points, the value to which the pointer passed points to would be changed:

```c++
void reset(int *ip) {
	*ip = 0;
}
```

> C程序员经常使用pointers作为函数的parameters，而在C++中，则经常使用reference parameters.



#### 6.2.2 Passing Arguments by Reference

A reference parameter is bound directly to the object from which it is initialized. For example:

```c++
void reset(int &i) {
	i = 0;
}
```

when we call the reset, i will be bound to whatever int object we pass. Changes made to the i are also made to which i refers.

##### Using reference to Avoid Copies

Using the reference could avoid the copies.

> Reference parameters that are not changed inside a function should be references to const.

##### Using Reference Parameters to Return Additional Information

Reference parameters let us effectively return multiple results. For example, 如果我们要程序找到字符在字符串中第一个出现的索引，则只需要返回位置。而如果我们又想要程序同时返回字符在字符串中出现了多少次，则可以使用reference parameters，作为第二个返回值.

```c++
string::size_type find_char(const string &s, char c, string::size_type &occurs) {
	auto ret = s.size();
    occurs = 0;
    for (decltype(ret) i = 0; i != s.size(); ++i) {
        if (s[i] == c) {
            if (ret == s.size())
                ret = i;
        	++occurs;
        }
    }
    return ret;
}
```



#### 6.2.3 const Parameters and Arguments

When we use parameters that are const, it is important to notice that they are top-level const, which means for initialization, the **top-level consts are ignored**. Therefore, it is possible to pass either a const or a non const object to a parameter that a top-level const.

It also has one implication:

函数可以重载，但是对于两个相同名称，返回值类型的函数而言，如果二者的参数一个为non-const，另一个为const，则无法进行重载。

```c++
void fcn(const int i) { }
void fcn(int i) { }   // error: redefines fcn(int)
```



##### Use Reference to const when possible

好处有两个：

1. 使用reference to const可以避免argument被修改
2. 使用reference to const可以让caller传入一个literal



#### 6.2.4 Array Parameters

 Array has two properties: we cannot copy an array, and when we use an array, it is usually converted to a pointer.

Therefore, when we pass an array to a function, we are actually passing a pointer to the array's first element.

Considering three types of declaration:

```c++
void print(const int*);
void print(const int[]);
void print(const int[10]);
```

Those three declarations are equivalent, each declares a functio with a single parameter of type `const int *`

当我们将数组传入到函数里后，其会被自动转换为pointer to the first element in the array, 而数组的长度是无关紧要的。

由于函数并不知道数组的长度是多少，因此额外的信息必须被提供。



##### Using a Marker to Specify the Extent of an Array

Just like the C-style character string, we could add a null character at the last of a string. The function that deal with C-style strings stop processing the array when they see a null character.

```c++
void print(const char *cp) {
	if (cp) {   // whether cp is nullptr
        while (*cp) {   // whether the character that cp points to is a null character
            cout << *cp++ << endl;
        }
    }
}
```



##### Using the Standard Library Conventions

A second approach used to manage array arguments is to pass pointers to the first and one past the last element in the array:

```c++
void print(const int *beg, const int *end) {
    while (beg != end) {
        cout << *beg++ << endl;
    }
}
```



##### Array Parameters and const

注意前几个代码实例都用pointer to const来表示数组，因为此处只需要读取数组，而不是写入数组。前面对reference to const的讨论同样应用于此处。只有当需要对数组元素进行修改的时候，才会使用plain pointer to a nonconst type.



##### Array Reference Parameters

We can define a parameter that is a reference to an array:

```c++
void print(int (&arr)[10]) {
	for (auto elem : arr)
		cout << elem << endl;
}
```

但由于在数组的尺寸也在declarator的一部分，因此该函数实现的功能有限，P(654)会有详细的说明关于传入一个任何大小的数组实现。



#### 6.2.6 Functions with Varying Parameters

If we do not know in advance how many arguments we need to pass to a function. We could pass a library type named `initializer_list`. If the argument types varies, we can write a special kind of function, known as a variadic template. (p.699)

Another special type, `ellipsis`, that can be used to pass a varying number of arguments. 然而该函数通常被用于C函数的接口。



##### `initializer_list` Parameters

It is a library type that represents an array of values of the specified type. It is defined in the `initializer_list` header. 

For the copy operation, copying or assigning an initializer_list does not copy the element in the list. After the copy, the original and the copy share the elements.

```c++
// Copy Operation
lst2(lst);
lst2 = lst;
```

Unlike vector, the elements in an `initializer_list` are always const values. There's no way to change the value of the element in an `initializer_list`.

注意，该容器只支持以下几个函数：

1. size()
2. begin()
3. end()

且并不支持subscript的运算符重载

We can write a function to produce error messages from a varying number of arguments as follows:

```c++
void error_msg(initializer_list<string> il) {
	for (auto beg = il.begin(); il != il.end(); ++beg)
        cout << *beg << " ";
    cout << endl;
}

// Invocation 
error_msg({"functionX", expected, actual});
```



##### Ellipsis Parameters

Ellipsis parameters are in C++ to allow programs to interface to C code that uses a C library facility named `varargs`. 

> Objects of most class types are not copied properly when passed to an ellipsis parameter.

It takes either of two forms:

```c++
void foo(para_list, ...);
void foo(...);
```

  

### 6.3 Return Types and the return Statement

#### 6.3.2 Functions that return a vlaue

> Failing to provide a return after a loop that contains a return is an error



##### How Values Are Returned

Values are returned in exactly the same way as variables and parameters are initialized. The return value are used to initialize a temporary at the call site. After returning a value, the value is copied to the call site.

注意，正常情况下，所有的返回值都是通过**copy**来传递到外层block中去的。



##### Never Returns a Reference or Pointer to a Local Object

由于当函数结束以后，在这个scope里面的所有数据全会被清楚，因此返回对某个local objects的引用是invalid的。

```c++
const string *manip() {  // Disaster: this functio nreturns a reference to a local object
	string ret;
	if (!ret.empty()) {
		return ret;  // Wrong: returning a reference to a local object!
	}
	else {
		return "Empty";
	}
}
```

  

##### Reference Returns Are Lvalues

Calls to functions that return references are lvalues; other return types yield rvalues. Therefore, we can assign to the result of a function that returns a reference to nonconst;

```c++
char &get_val(string &str, string::size_type ix) {
	return str[ix];
}

int main() {
    string s("a value");
    cout << s << endl;
    get_val(s, 0) = 'A';
    cout << s << endl;  // prints: A value
    return 0;
}
```

由于正常的return都是通过copy进行的，因此只有通过reference才能得到正在的结果.



##### List initializing the Return Value

Under the new standard, functions can return a braced list of values:

```c++
vector<string> process() {
	// ...
	if (expected.empty())
		return {};
	else if (expected == actual)
		return {"functionX", "okay"};
	else
		return {"functionX", expected, actual};
}
```



#### 6.3.3 Returning a Pointer to an Array

Since the syntax used to define functions that return pointers or references to array can be intimidating. But a simplified way could be adopted:

```c++
typedef int arrT[10];
using arrT = int[10];
arrT* func(int i);  // func returns a pointer to an array of ten ints
```



##### Declaring a Function that return a pointer to an array

The declaration for a function that returns a pointer to an array has following format:

```c++
Type *(function(parameter_list)) [dimension];
int (*func(int i)) [10];
```



##### Using a Trailing Return Type

Trailing return type follows the parameter list and is preceded by ->. It is most commonly used for function that has complicated return types.

```c++
auto function(parameter_list) -> return_type {
	// function body
}

auto func(int i) -> int(*)[10];
```



### 6.4 Overloaded Functions

Functions that have the same name but different parameter lists and that appear **in the same scope** are **overloaded**.

Overloaded functions must differ in the number or the type of their parameters.

注意：It is an error for two functions to differ only in terms of their return types. If the parameter lists of two functions match but the return types differ, then the **second declaration** is an error. 

如果两个函数在参数列表上相互匹配，但如果其中一个函数的返回值类型不同，那么编译器就会报错。



##### Overloading and const Parameters

The top-level const has no effect on the objects that can be passed to the function. ***Therefore, a parameter that has a top-level const is indistinguishable from one without a top-level const.***



##### `const_cast` and Overloading

`const_cast` are most useful in the context of overloaded functions.

```c++
const string &shorterString(const string &s1, const string &s2) {
	return s1.size() <= s2.size() ? s1 : s2;
}

// To have a function that passes nonconst arguments and yield a plain reference, we can rewrite as follow:
string &shorterString(string &s1, string &s2) {
    auto &r = shorterString(const_cast<const string&>(s1),
                           	const_cast<const string&>(s2));
    return const_cast<string&>(r);
}
```



### 6.5 Features for specialized Uses

#### 6.5.1 Default Arguments

Some functions have parameters that are given a particular value mostly. Therefore, we can declare that common value as a **default argument** for the function.

##### Calling Functions with Default Argument

注意，在调用函数的时候，我们可以省略default argument的传参，但是省略最右边的参数 (omit the right-most parameter ***(trailing arguments)***).



#### 6.5.2 Inline and constexpr Functions

##### inline Functions Avoid Function Call Overhead

对于有些函数而言，如果只是去返回一个表达式，会造成内存不必要的开销，因为函数会被保存在寄存器当中，而argument会被复制，使用inline则可以避免这一问题。

对于上面代码部分`shorterString`函数，使用inline可以让函数的调用变为：

```c++
cout << shorterString(s1, s2) << endl;
cout << (s1.size() < s2.size() ? s1 : s2) << endl;
```

这样，run-time overhead就被remove掉了

> The inline specification is only a request to the compiler. The compiler may choose to ignore this request.

We define `shorterString` as an inline function by putting the keyword inline before the function's return type.



##### constexpr Functions

A constexpr function is a function that can be used in a constant expression. *The return type and the type of each parameter must be a literal type, and the function body must contain exactly one return statement.* 这样可以确保相应的值可以在compile time被evaluated.

```c++
constexpr int new_sz() { return 42; } 
constexpr int foo = new_sz(); // ok: foo is a constant expression
```

The compiler will replace a call to a `constexpr` function with its resulting value. They are implicitly `inline`.

A constexpr function is permitted to return a value that is not a constant as long as they generate no actions at run time:

```c++
constexpr size_t scale(size_t cnt) { return new_sz() * cnt; }
```

> A constexpr function is not required to return a constant expression.



##### Put inline and constexpr Functions in Header Files.

由于函数的definition代码可能会被重复定义，且需要与头文件相匹配，因此我们通常将inline和constexpr函数放在头文件当中。无论它被定义in class or out of class.



#### 6.5.3 Aids for Debugging

##### The assert Preprocessor Macro

assert is a preprocessor macro. A preprocessor macro is a preprocessor variable that acts somewhat like an inline function:

```c++
#include <cassert>
assert(expr);
```

if the expression is false (i.e. zero), then assert writes a message and terminates the program.



##### The NDEBUG Preprocessor Variable

The behavior of assert depends on the status of a preprocessor variable named `NDEBUG`. If `NDEBUG` is defined, `assert` does nothing. Since, by default, NDEBUG is not defined, thus, assert performs a run-time check by default.

我们通过控制行可以设置NDEBUG：

```
g++ -DNDEBUG -o test_release main.cpp
./test_release
```

在Visual studio中，如果将其变为debug模式，所有assert语句都会执行，若变为release模式，那么编译器不再执行这些语句。

除此之外，我们还可以通过`#ifndef`和`#endif`语句来设置是否执行代码。

```c++
void print(const int ia[], size_t size) {
#ifndef NDEBUG
   cerr << __func__ << ": array size is " << size << endl;
#endif
}
```

Here we use the variable named `__func__`to print the name of the function we are debugging. The compiler defines `__func__` in every function. It is a local static array of `const char` that holds the name of the function.

除此之外，C++ compiler还定义了别的preprocessor：

1. `__FILE__ `文件名字
2. `__LINE__`integer literal containing the current line number
3. `__TIME__`string literal containing the time the file was compiled
4. `__DATE__`string literal containing the date the file was compiled

For example:

```c++
int main()
{
	string word = "foo";
	int threshold = 5;
	if (word.size() < threshold) {
		cerr << "Error: " << __FILE__
			<< " : in function " << __func__
			<< " at line " << __LINE__ << endl
			<< "       Compiled on " << __DATE__
			<< " at " << __TIME__ << endl
			<< "       Word read was \"" << word
			<< "\":  Length too short" << endl;
	}
}
```



### 6.6 Function Matching



### 6.7 Pointers to Functions

A function pointer is just that a pointer that denotes a function rather than an object. A function's type is determined by its return type and the types of its parameters. For example:

```c++
bool lengthCompare(const string&, const string &);
```

To declare a pointer that can point to this function:

```c++
bool (*pf) (const string &, const string &); // uninitialized.
```

Looking to the right, pf points to a function. Looking left, we find that the type the function returns is bool.

> The parentheses around *pf are necessary. If we omit then, then the declaration would becomes a function that returns a pointer to bool:
>
> `bool *pf(const string &, const string &);`



##### Using Function Pointer

When we use the name of a function as a value, the function is automatically converted to a pointer. For example, we can assign the address of `lengthCompare` to pf as follows:

```c++
int add(const int& a, const int& b) {
	return a + b;
}

int main()
{
	int (*pf) (const int& a, const int& b); // uninitialized
	pf = add;
	pf = &add;
}
```

We could use the pointer to call the function to which the pointer points:

```c++
int res = pf(1, 2);     // calls add
int res2 = (*pf)(1, 2); // equivalent call
int res3 = add(1, 2);   // equivalent call
```

We could assign pf to zero to let pf points to no function:

```c++
pf = 0;
```



##### Pointers to Overloaded Functions

The compiler uses the type of the pointers to determine which overloaded function to use.



##### Function Pointer Parameters

We cannot define parameters of function type, but we can have a parameter that is a pointer to a function. 相当于C#中的委托。We can write a parameter that looks like a function type, **but it will be treated as a pointer. ** 当我们将function当作为argument时，*其会自动转换为pointer*. 

```c++
void useBigger(const string &s1, const string &s2, bool pf(const string&, const string&));

void useBigger(const string &s1, const string &s2, bool (*pf)(const string &, const string &));

useBigger(s1, s2, lengthCompare);
```

To make it simpler, we could use type alias.

```c++
// Func and Func2 have function type
typedef bool Func(const string &, const string &);
typedef decltype(lengthCompare) Func2;

// FuncP and FuncP2 havea pointer to function type
typedef bool(*FuncP) (cons6t string &, const string &);
typedef decltype(lengthCompare) *FuncP2;
```

注意，此处的automatic conversion for `Func` and `Func2` 并没有完成，它们是function type.

```c++
void UseBigger(const string&, const string&, Func);
void UseBigger(const string&, const string&, FuncP2);
```



##### Returning a Pointer to Function

The easiest way to return a pointer to function is by using a type alias:

```c++
using F = int(int*, int);      // F is a function type, not a pointer.
using PF = int(*)(int*, int);  // PF is a pointer type
```

Here we use the type alias declare function that return a pointer to function:

```c++
PF f1(int);  // ok, PF is the pointer to function
F f1(int);   // error: F is functio type, f1 can't return a function
F *f1(int);  // ok: explicitly specify that the return type is a pointer to function
```

Declaring directly would be like:

```c++
int (*f1(int))(int*, int);
```

Reading from the inside out, f1 has a parameter list (int). Reading left, f1 returns a pointer. The type of that pointer also has a parameter list, so the pointer points to a function. That function returns int.

```c++
int add(const int& a, const int& b) {
	return a + b;
}

int (*add_ptr(const int& a, const int& b))(const int& a, const int& b) {
	int (*pf)(const int&, const int&) = add;
	return pf;
}
```

We could also adopt the trailing return to simplify the declaration of functions:

```c++
auto f1(int) -> int (*)(int*, int);
```

Thus:

```c++
auto plus_ptr(const int& a, const int& b) -> int(*)(const int&, const int&) {
	int (*pf)(const int&, const int&) = add;
	return pf;
}
```



##### Using auto or decltype for Function Pointer Type

我们可以使用decltype来simplify writing a function pointer return type. 但是注意decltype返回的是函数类型，并不是函数指针类型。

```c++
string::size_type sumLength(const string&, const string&);
string::size_type largerLength(const string&, const string&);

decltype(sumLength) *genFcn(const string&);
```

一定需要explicitly添加指针.



## Chapter 7 Classes

Member functions must be declared inside the class. Member functions may be defined inside the class itself or outsid the class body.

> Functions defined in the class are implicitly `inline`.

```c++
struct Sales_data {
	std::string isbn() const { return bookNo; }
	Sales_data& combine(const Sales_data&);
	double avg_price() const;

	std::string bookNo;
	unsigned units_sold = 0;
	double revenue = 0.0;
};
```



##### Introducing `this`

成员函数通过一个implicit参数`this`来访问它们被调用的对象。当我们调用一个成员函数的时，`this` is initialized with the **address of the object** on which the function was invoked. 例如：

```c++
total.isbn();
```

当我们调用这个函数的时候，编译器会将total这个对象的地址传入implicit `this` parameter in isbn. 也就是this是成员函数的一个参数。因此编译器会重写为：

```c++
Sales_data::isbn(&total);
```

this是一个指向对象的指针

而在成员函数内部，我们不需要通过member access operator (.)来使用对象的成员。Any direct use of a member of the class is assumed to be an implicit reference through this. 也就是说在isbn函数使用bookNo的时候，他是在隐式使用this指向的成员: `this->bookNo`.

this是一个const pointer，我们无法改变this所承载的地址。



##### Introducing `const` Member Functions

The keyword `const` that follows the parameter list is to modify the type of the implicit this pointer. By default, the type of this is a const pointer to the nonconst version of the class type. A const following the parameter list indicates that `this` is a pointer to const. Member functions that use const in this way are **const member functions**.

Having a const member functions means that const member functions cannot change the object on which they are called.

Therefore, isbn may read but not write to the data members of the objects on which it is called.

只能读，不能写！



##### Member Functions

bookNo和isbn的定义先后顺序不重要，因为编译器在处理类的时候会分为两个部分，成员声明会被编译，然后再是成员函数的body。



##### Defining a Member Function outside the Class

```c++
double Sales_data::avg_price() const {
	if (units_sold)
		return revenue / units_sold;
	else
		return 0;
}
```

此处的函数名字使用了scope operator，意思是we are defining the function named `avg_price` that is declared in the scope of the `Sales_data` class.



##### Defining a Function to Return "This" Object

We want our function defined operates like a built-in operator. The built-in assignment operators return their left-hand operand as an lvalue. To return an lvalue, the combine function must **return a reference. ** lvalue意味着它们可以**被赋值**.

```c++
Sales_data& Sales_data::combine(const Sales_data& rhs) {
	units_sold += rhs.units_sold;
	revenue += rhs.revenue;
	return *this;
}
```

the return statement dereferences this to obtain the object on which the function is executing.



#### 7.1.3 Defining Nonmember Class-Related Functions

Ordinarily, nonmember functions that are part of the interface of a class should be declared in the same header as the class itself. 一般而言，会将非成员函数声明在与类被声明的头文件中，而定义则会被放在.cpp文件当中。



#### 7.1.4 Constructors

Each class defines how objects of its type can be initialized. Classes control object initialization by defining one or more special member functions known as constructors. 构造函数的作用是用来初始化类对象的成员，当这个类型被创建的时候，构造函数将会被执行。

构造函数不能被声明为const，而当我们创建了类类型的const对象时，该对象只有在构造函数完成对象初始化后才会具有常量属性，因此构造函数可以在其中修改常量对象。



##### The Synthesized Default Constructor

Classes control default initialization by defining a special constructor, known as the default constructor. The compiler will implicitly define the default constructor for us.

The compiler-generated constructor is known as the synthesized default constructor. It initializes each data member as follows:

1. 如果有一个in-class initializer, 那么用它看来初始化成员
2. 如果没有，则default-initialize the member.



##### Some Classes Cannot Rely on the Synthesized Default Constructor

The compiler generates the default synthesized default constructor for us ***only if we do not define any other constructors for the class***. If we define any constructors, the class will not have a default constructor unless we define that constructor ourselves.

这样做是因为如果一个类在某一个情况下需要初始化，那么这就意味着在所有情况下都要初始化以确保一致性。另外，对于非built-in type的成员变量，使用synthesized default constructor可能会导致错误，因为它们不一定会有默认值，这就导致了它们会有undefined value，前提是它们都有in-class initializer.



##### Defining default constructors

To define a default constructors, we could use syntax `class_name()  = default;`. Under the new standard, if we want the default behavior, we could ask the compiler to generate it by writing `=default`.

**If it appears inside the class body, the default constructor will be inlined; If it appears on the definition outside the class, the member will not be inlined by default.**



##### Constructor Initializer List

The constructor initializer list specifies initial values for one or more data members of the object being created:

```c++
Sales_data(const std::string &s) : bookNo(s) { }
Sales_data(const std::string& s, unsigned n, double p) : bookNo(s), units_sold(n), revenue(p* n) { }
```

When a member is omitted from the constructor initializer list, it is **implicitly** initialized using the same process as it used by the ***synthesized default constructor.***

It is usually best for a constructor to use an **in-class initializer** if it could gives the member the correct value.

However, it it doesn't or the compiler does not yet support in-class initializer, then the constructor should explicitly initialize every member of built-in type.

In-class initializer指的是在define成员变量的时候已经为其提供了initializer了。

> It would be more correct to say that the constructor initializer list is empty.



#### 7.1.5 Copy, Assignment, and Destruction

Copy, assignment and destruction are common operation for classes. If we don't define these operations, the compiler would synthesize them for us.

##### Some Classes Cannot Rely on the Synthesized Versions

For some classes, the default versions do not behave appropriately. For example, when it comes to classes that manage dynamic memory, it cannot rely on the synthesized versions.



### 7.2 Access Control and Encapsulation

In C++, we use access specifiers to enforce encapsulation.

- Members defined after a public specifier are accessible to all parts of the program. The `public` members define the interface to the class.
- Members defined after a private specifier are accessible to the member functions of the class but are not accessible to code that uses the class. The `private` sections encapsulate the implementation.



##### Using the class or struct Keyword

The difference between between `struct` and `class` is the default access level. The default access level for `struct` is **public**, but the members are **private** if we use `class`.



#### 7.2.1 Friends

A class can allow another class or function to access its nonpublic members by making that class or function a friend. 

对于有些非成员函数而言，它们需要获取到类中被封装的成员变量(private)，而通过定义为friend，则允许这些函数进行访问。

```c++
friend std::istream& read(std::istream& is, Sales_data& item);
```

> It is a good idea to group friend declarations together at the beginning or end of the class definition.



##### Declarations For Friends

A friend declaration only specifies access. It is not a general declaration of the function. If we want users of the class to be able to call a friend function **(To make a friend visible to users of the class)**, we must also declare the function separately from the friend declaration in the header file.

在类内部声明友元函数只是让编译器知道这个函数是该类的友元函数，但并不意味着这个函数在类的作用域之外是可见的，而为了能够让用户使用，则会在相同的头文件中再次定义该类。



### 7.3 Additional Class Features

#### 7.3.1 Class Members Revisited

##### Defining a Type Member

In addition to defining data and function members, a class can define its own local names for types. Type names defined by a class are subject to the same access controls as any other member.

```c++
class Screen {
public:
	typedef std::string::size_type pos;
private:
	pos cursor = 0;
	pos height = 0, width = 0;
	std::string contents;
};
```

There're two more points. 

Firstly, we can equivalently use a type alias:

```c++
class Screen {
public:
	using pos = std::string::size_type;
};
```

Secondly, members that define types must appear before they are used. Therefore, type members usually appear at the beginning of the class.



##### mutable Data Members

A mutable data member is never const, even when it is a member of a const object. 

A const member function may change a mutable member. 

```c++
class Screen {
public:
	void some_member() const;
private:
	mutable size_t access_ctr;
};

void Screen::some_member() const {
	++access_ctr; // ok
}
```



##### Initializers for Data Members of Class Type

在新标准中，若要specify the default value for a vector of a class, the best way is to use in-class initailizer:

```c++
class Window_mgr {
private:
    std::vector<Screen> screen { Screen(24, 80, ' ')};
};
```



#### 7.3.2 Functions That Return *this

在定义一系列operation的时候，注意它们的返回值都是reference类型，这是因为我们希望返回值是一个lvalue，而不是一个rvalue。除此之外函数在返回的时候，如果返回值不是reference类型，那么其返回的是一个复制的对象，而并非其自身，例如对于函数set而言：

```c++
inline Screen& Screen::set(char c) {
	contents[cursor] = c;
	return *this;
}
```

如果其返回值只是Screen类型，那么返回的结果是类对象自身的copy，如果返回值是reference，那么返回值则是类对象发本身。为了验证这一结论，可以通过以下实验完成：

此处的move函数返回值为Screen&类型：

```c++
myScreen.set('#').move(1, 0);
```

如果其中set的返回值是Screen，而move的返回值是Screen&。当我们去取返回值的地址时，我们会发现当set的返回值是Screen的时候，整体的返回值的地址和当初定义的`myScreen`变量的地址不同。

```c++
Screen myScreen(10, 20, '*');
cout << &myScreen << endl; // 000000C4BCCFF880
Screen *ptr = &(screen.set('#', 1).move(1, 2));
cout << ptr << endl;       // 000000C4BCCFFA60
Screen* ptr2 = &(screen.set('#').move(1, 2));
cout << ptr2 << endl;      // 000000C4BCCFF880
```

而这就导致，在对没有使用Screen&作为返回值的set函数而言，后面的move函数并未实现对myScreen的内容进行修改，反而是对set函数中返回的对myScreen对象的copy进行的修改。

```c++
int main() 
{
	Screen screen(10, 20, '*');

	cout << screen.get_cursor() << endl; // 初始值为0
	Screen *ptr = &(screen.set('#', 1).move(1, 2));
	cout << ptr << endl;

	cout << screen.get_cursor() << endl; // 在经过第一次修改以后还是为0

	Screen* ptr2 = &(screen.set('#').move(1, 2));
	cout << ptr2 << endl;

	cout << screen.get_cursor() << endl;
}
```



##### Returning *this from a const Member Function

如果一个函数定义为const，那么this则变为了a pointer to const，而*this则是一个const object.，也因此我们无法进行下一步的操作：

```c++
Screen myScreen;
myScreen.display(cout).set('*');
```

在这个例子中，display是一个const function，也因此其返回的值是a reference to const，这让后面的operation无法进行。We cannot call set on a const object.

> A const member function that return *this as a reference should have a return type that is a reference to const.



##### Overloading Based on const

The nonconst version will not be viable for const objects; We could only call const member functions on a const object. We can call either version on a nonconst object. Therefore, sometimes, we need to define specifically a const function for the const objects of the class.

考虑到const object只能调用它自身的const函数，因此我们有些时候需要去为一些函数进行重载，从而使得const object也同样可以调用这些函数。



#### 7.3.3 Class Types



#### 7.3.4 Friendship Revisited

Not only a class could define nonmember functions as friends, but also makes another class its friend or declare specific member functions of another class as friends.

##### Friendship between Classes

To let the member function of class B to get access to the private data of the class A, we need to declare a friend class inside the class A:

```c++
class A {
	friend class B;
private:
    int myData;
    vector<std::string> str_vec;
}
```

To allow this access, A can designate B as its friend;

As a result, the member functions of a friend class B can access all the members of A, including the nonpublic members.

```c++
class B {
public:
	void clearVec(A a);
}

void B::clearVec(A a) {
	a.str_vec.clear();
}
```

However, the friendship is not ***transitive***. That is, if class B has its own friend, then those friends have no special access to class A.



##### Making a Member Function a Friend

Apart from making the entire class as a friend, it is possible to specify only a certain member function is allowed access. To declare a member function to be a friend, we must specify the class of which that function is a member:

```c++
class Screen {
	friend void Window_mgr::clear(ScreenIndex);
};
```

In this case, the clear function from the class Window_mgr would like to get access the data from the Screen class. By defining clear member as a friend, only this member are able to access internal data from Screen.



##### Overloaded Functions and Friendship

每个重载函数都需要被定义为friend，毕竟它们本质上都是不同的函数。



##### Friend Declarations and Scope

NULL



### 7.4 Class Scope

We access type members from the class using the scope operator.

```c++
Screen::pos ht = 24, wd = 80;
```

```c++
class Screen {
public:
	typedef std::string::size_type pos;
};
```



##### Scope and Members Defined outside the Class

注意，在类里面定义的type类型，即使定义不在class的scope里面，但只要compiler看到了和这个类相关的scope operator，那么则可以直接使用类的内容。

```c++
void Window_mgr::clear(ScreenIndex i) {
	Screen &s = screens[i];
	s.contents = string(s.height * s.width, ' ');
}
```

此处的`ScreenIndex`是定义在`window_mgr`里面的类型。

而相反，返回值类型被定义在scope operator前面，这就意味着返回值的类型必须specify the class of which it is a member, 例如以下例子钟，`ScreenIndex`是定义在`Window_mgr`里的类型，因此使用它作为返回值必须要添加Scope Operator.

```c++
class Window_mgr {
public:
	ScreenIndex addScreen(const Screen&);
};

Window_mgr::ScreenIndex Window_mgr::addScreen(const Screen &s) {
	screens.push_back(s);
	return screens.size() - 1;
}
```



#### 7.4.1 Name Lookup and Class Scope

To be Written... P283



### 7.5 Constructors Revisited

If we do not explicitly initialize a member in the constructor initializer list, that member is default initialized before the constructor body starts executing.

```c++
Sales_data::Sales_data(const string &s, unsigned cnt, double price) {
	bookNo = s;
	units_sold = cnt;
	revenue = cnt * price;
}
```

在执行上面的函数体之前，所有变量都是会被default-initialized.



##### Constructor Initializers are sometimes required

Members that are const or references must be initialized. 

Members that are of a class type that does not define a default constructor also must be initialized. 

```c++
class ConstRef {
public:
    ConstRef(int i);
private:
    int i;
    const int ci;
    int &ri;
};
```

member `ci` and `ri` must be initialized, therefore omitting a constructor initializer for these members is an error;

> We must use the constructor initializer list to provide values for members that are const, reference, or of a class type that does not have a default constructor.



##### Order of the Constructor Initializer.

The order of the constructor initializer doesn't matter. But it is a good idea to write constructor initializer in the same order as the members are declared.



##### Default Argument and Constructors

> A constructor that supplies default arguments for all its parameters also defines the default constructor





#### 7.5.2 Delegating Constructors

A delegating constructor uses another constructor from its own class to perform its initialization.

For example：

```c++
class Sales_data {
public:
    // Nondelegating constructor.
    Sales_data(std::string s, unsigned cnt, double price) : bookNo(s),units_sold(cnt),revenue(cnt * price) { }
    // Remaining constructor will delegate to another constructor
    Sales_data() : Sales_data("", 0, 0) { }
    Sales_data(std::string s) : Sales_data(s, 0, 0) { }
};
```

两者的执行先后顺序是，先去执行被代理的函数，再去执行代理的函数。



#### 7.5.3 The Role of the Default Constructor

> In practice, it is almost always right to provide a default constructor if other constructors are being defined.

##### Using the Default Constructor

注意在调用Default constructor不需要带parentheses

```c++
Sales_data obj();  // ok: but defines a function, not an object
Sales_data obj2;   // ok: obj2 is an object.
```



#### 7.5.4 Implicit Class-Type Conversions

A constructor that can be called with a single argument defines an implicit conversion from the constructor's parameter type to the class type.

也就是对于只有单一参数的构造函数，它们同时也定义了implicit conversion，将其唯一的参数类型转换到了类的类型。

```c++
Sales_data item("999-123", 20, 3);
string null_book = "9-999-9999-9";
item.combine(null_book);
```

Sales_data的一个构造函数是Sales_data(const std::string&)，而在combine函数的参数中，该string被自动转换为了Sales_data类型。



#### Only One Class-Type Conversion is Allowed

The compiler could automatically apply only one class-type conversion. 例如：

```c++
item.combine("9-999-9999-9"); // error
```

这段代码试图将"9-999-9999-9"先转换为string，然后再将string转换为Sales_data。可见，"9-999-9999-9"是一个char*，而对于任何包含字符串作为参数的函数而言，其本质上就是在做implicit class-type conversion，将字符指针转换为了字符串。



##### Not always useful

```c++
string null_book = "9-999-9999-9";
item.combine(null_book);
```

对于该代码而言，一个问题在于我们无法access到由null_book所创建的Sale_data对象。This object is temporary.



##### Suppressing Implicit Conversions Defined by Constructors

我们可以通过explicit关键字来避免构造函数被用来做implicit conversions:

```c++
class Sales_data {
public:
	Sales_data() = default();
	Sales_data(const std::string &s, unsigned n, double p): bookNo(s), units_sold(n), revenue(n * p) { }
    explicit Sales_data(const std::string &s) : bookNo(s) { }
    explicit Sales_data(std::istream&);
};
```

在将构造函数定义为explicit以后，再使用implicit conversion，编译器则会报错

注意，explicit只对有单一参数的构造函数有用，因为implicit conversion only happens on constructors with only one argument. 因此对于多参数的构造函数，没有必要定义explicit。

此外，explicit keyword is used only on the constructor declaration inside the class. It is not repeated on a definition made outside the class body:

```c++
explicit Sales_data::Sales_data(istream& is) { // error
	read(is, *this);
}
```



#### 7.5.4 Aggregate Classes

An aggregate class gives users direct access to its members and has special initialization syntax. An aggregate class must meet the following requirements:

1. All of its data members are public
2. It does not define any constructors
3. It has no in-class initializers
4. It has no base classes or virtual functions

An aggregate class could be initialized by providing a braced list of member initializers:

```c++
struct Data {
	int ival;
	string s;
};

Data val1 = { 0, "Anna" };
```

If the list of initializers has fewer elements than the class has members, the trailing members are value initialized (3.5.1).



#### 7.5.6 Literal Classes

 An aggregate class whose data members are all of literal type is a literal class. A nonaggregate class, that meets the following restrictions, is also a literal class:

1. The data members all must have literal type
2. The class must have at least one constexpr constructor.
3. For data members who have in-class initializer, the initializer must be a constant expression. If the member has class type, the initializer must use the member's own `constexpr` constructor.



##### constexpr Constructors

A constexpr Constructor can't have anything in the function body. It must initialize every data member. The initializers must either use a constexpr constructor or be a constant expression.

It is used to generate objects that are constexpr and for parameters or return types in constexpr functions.

```c++
class Debug {
public:
	constexpr Debug(bool b = true) : hw(b), io(b), other(b) { }
	constexpr Debug(bool b, bool i, bool o) : hw(h), io(i), other(o) { }
	constexpr bool any() { return hw || io || other; }
	void set_io(bool b) { io = b; }
	void set_hw(bool b) { hw = b; }
	void set_other(bool b) { hw = b; }
private:
	bool hw;
	bool io;
	bool other;
};

constexpr Debug io_sub(false, true, false);
```



### 7.6 static class Members

类可能需要一些成员是和类有关系而并非和其生成的对象有关系。例如银行需要一个数据成员用来表示当前的利率，此时的该数据是和类有关系，并非和每个object有关系。

##### Declaring static Members

We say a member is associated with the class by adding the keyword static to its declaration.

**Important:**

The static members of a class exist outside any object. Objects do not contain data associated with static data members.

Simultaneously, the static member function does not bound to any object, therefore, they do not have `this` pointer. 因此也就不能被定义为const函数。



##### Using a Class static Member

We can access a static member through the scope operator.

```c++
double r;
r = Account::rate();
```

Even though static members are not part of the objects, we can use an object, reference, or pointer of the class type to access a static member.

```c++
Account ac1;
Account *ac2 = &ac1;

r = ac1.rate();
r = ac2->rate();
```

Member functions can use static member directly, without using the scope operator.

```c++
class Account {
public:
	void calculate() { account += amount * interestRate; }
private:
	static double interestRate;
}
```



##### Defining static Members

We can define a static member function either inside or outside of the class body. When we define a static member function outside of the class body, we do not repeat the static keyword, which only appears in the declaration inside the class body.

```c++
void Account::rate(double newRate) { // don't add the static keyword.
	interestRate = newRate;
}
```

由于static member并不是每个类对象的一部分，因此它们不能在构造函数中被初始化，相反，他们应该在class body外进行初始化。

就如同全局对象，static data members are defined outside any function. Therefore, they continue to exist until the program completes. How we define a class member is as following:

```
double Account::interestRate = initRate();
```

注意，此处一定要再次specify类型，因为在类里面只是进行了声明，但编译器并没有为其分配内存空间。如果不加，那么编译器会报错。

> 为了确保所有的对象只是被定义了一次，最好的方式是放到.cpp文件中，和那些定义为noninline function一起。



##### In-Class Initialization of static Data Members

只有对于是constexpr或const类型的static data member才可以使用in-class initialization.

即便他们可以被initialized in the class body，但最好还是将他们定义在类的定义以外。

一般来说，对于那些直接可以用值代替的变量而言，最好可以将其定义在class里面，otherwise，需要将其定义在外部。



## Chapter 8. The IO Library

### 8.1 The IO Classes

`iostream` defines the basic types used to read from and write to a stream.

`istream`, `wistream` reads from a stream.

`ostream`, `wostream` writes to a stream.

`iostream`, `wiostream` reads and writes a stream.



`fstream` defines the type used to read and write named files.

`ifstream`, `wifstream` reads from a file.

`ofstream`, `wofream` writes to a file.

`fstream`, `wfstream` reads and writes to a file.



`sstream` defines the types used to read and write in-memory `strings`.

`istringstream`, `wistringstream` reads from a string.

`ostringstream`, `wostringstream` writes to a string.

`stringstream`, `wstringstream` reads and writes a string.



为了支持语言中的wide characters，library还包含了各种用来操作wchar_t数据的类型和对象，例如`wcin`, `wcout`和`wcerr`，与`cin`,`cout`, `cerr`相互对应。



#### 8.1.2 Conditional States

If an IO operation fails, then the subsequent operation on that stream will fail. 

```c++
int ival;
cin >> ival; // If we enter Boo, then the read will fail.
```

Therefore, we should check whether a stream is okay before attempting to use by taking the object as a condition.



##### Interrogating the State of a Stream

The IO library defines a **machine-dependent** integral **type** named `iostate` to convey information about the state of a stream. 注意，这是一种类型。

The state has following four flags: (类型为`stream::iostate`)

`strm::badbit`

​	System-level failure, such as an unrecoverable read or write error.

`strm::failbit`

​	The IO operation failed, such as reading a character when numerical values are expected.

`strm::eofbit`

​	Reading the end of the file.

`strm::goodbit`

​	It has value 0, indicates no failures on the stream.

注意，每个flag在程序中都相当于1 bit，你可以turn on or turn off. 

与此同时，library还提供了一系列函数，用来检测每个标志位flags的状态。

```c++
stream s;
s.eof();   // true if eofbit is set
s.fail();  // true if failbit or badbit is set
s.bad();   // true if batbit is set
s.good();  // true if the stream is in a valid state
s.clear(); // Reset all condition values in the stream to a valid state.
```



##### Managing the Conditional State

The `rdstate` member returns an `iostate` value corresponding to the state of the stream.

Other operations:

```c++
std::iostream::iostate old_state = cin.rdstate();
cin.clear();
process_input(cin);
cin.setstate(old_state);
```

<img src="D:\Computer Science\C++ Primer\Imgs\IO_conditionState.png" alt="IO_conditionState" style="zoom:50%;" />



在上图中，每个bit代表了一位，因此我们可以通过bitwise operator进行修改：

```c++
cin.clear(cin.rdstate() & ~cin.failbit & ~cin.badbit);
```

上述代码将`badbit`和`failbit`关掉，但是将`eofbit`留下来了。



#### 8.1.3 Managing the Output Buffer

任何输出流都包含了一块buffer，也就是缓冲区，它用来保存程序读写的数据。使用buffer能够使得操作系统将多个操作合并为一个单一操作，由于每次进行读写都需要调用操作系统资源，这会耗费大量资源，使用buffer可以大幅增加性能。

以下为buffer会被flush(刷新)的条件 (刷新缓冲区意思是强制将缓冲区的数据写入到目标设备当中)：

1. 程序正常结束
2. buffer满了的时候
3. explicitly flush the buffer using manipulator such as `endl`.
4. 通过`unitbuf` manipulator 手动设置流的内部状态，使其在每次输出操作后都自动刷新缓冲区

5. 如果一个输出流绑定到另一个流上，每当被绑定的流有一些操作时，绑定的流的缓冲区则会被刷新，例如默认情况下cin都绑定到了cout，因此读写cin都会导致cout的缓冲区被刷新。



##### Flushing the output Buffer

1. ends :arrow_right: 写入数据，在末尾添加null character并刷新缓冲区
2. flush :arrow_right: 写入数据，不添加任何数据，直接刷新缓冲区



##### Setting the state of the stream using `unitbuf` Manipulator.

```c++
cout << unitbuf;   // 告诉编译器all writes will be flush immediately
cout << nounitbuf; // 按照默认的方式进行flush
```

> 注意，如果程序崩溃，那么buffer可能并没有被flush，而是在pending状态



##### Tying Input and Output Streams Together

One version takes no argument and returns a pointer to the output stream.

Another version takes a pointer to an `ostream` and ties itself to that `ostream`, while it returns the `ostream` pointer pointing to the previous `ostream` object.

```c++
cin.tie(&cout);
ostream *old_tie = cin.tie(nullptr);  // old_tie is &cout
cin.tie(&cerr);
cin.tie(old_tie);
```



### 8.2 File Input and Output

#### 8.2.1 Using File Stream Objects

每当我们要读取或写入一个文件的时候，我们会定义一个文件流对象，并将该对象与文件关联在一起。每个file stream class都会有一个成员函数叫做open，相当于执行操作系统的指令，将流对象与文件关联起来。

在创建该对象的时候，我们可以提供文件名字，当我们提供名字后，open函数会被自动调用。

在新标准之下，名字可以是string也可以是c-style array，但在之前的标准中，只能允许c-style array.

```c++
ifstream in(file_name);
ofstream out;
```



##### Using an `fstream` in Place of an iostream&

如果函数的参数是istream或者ostream的时候，我们可以传入ifstream以及ofstream对象。



##### The `open` and `close` Members

在定义完文件流对象以后，我们可以通过调用open函数，将文件与对象关联起来：

```c++
ifstream in(ifile);
ofstream out;
out.open(ifile + ".copy");
```

如果open函数fail了，那么failbit会被设置，因此我们最好在使用之前添加判断：

```c++
if (out) {
    // Processing
} else {
    cerr << "Couldn't open file";
}
```

另外，如果一个流所关联的文件并没有关闭，并且用户尝试使用该流去打开关联别的文件，那么会报错，因此我们需要在打开别的文件之前先把文件关掉。

```c++
in.close();
in.open(ifile + "2");
```



#### File Modes

Each stream has an associated file mode that represents how the file may be used.

in 		Open for input (`ifstream` or `fstream` object)

out 	     Open for output  (`ofstream` or `fstream` object)

app	      Seek to the end before every write (追加写入文件，只有在trunc没有被specified的时候才能使用)

ate               Seek to the end immediately after the open (追加读写)

trunc            Truncate the file (删减，如果文件已经存在，则在打开时清空所有内容)

binary          Do IO operations in binary mode

 

ifstream对象打开的文件是in mode, ofstream打开的是out mode, fstream是in and out mode.



##### Opening a File in out Mode Discards Existing Data

默认情况下，如果我们打开ofstream，文件的内容都会被丢弃，避免的方法则是specify app.

```c++
ofstream out("file1")  // out and trunc are implicit
ofstream out1("file1", ofstream::out) // trunc are implicit
ofstream out2("file1", ofstream::out | ofstream::app);
```



### 8.3 string Streams

The sstream header defines three types to support in-memory IO:

1. istringstream type reads a string
2. ostringstream writes a string
3. stringstream reads and writes the string.

They all inherits from the types we have used from the iostream header.

```c++
string line, word;
vector<PersonInfo> people;
while (getline(cin, line))  {
    PersonInfo info;
    istringstream record(line);
    record >> info.name;
    while (record >> word) {
        info.phones.push_back(word);
    }
    people.push_back(info);
}
cout << people[0].name << endl;
```



## Chapter 9. Sequential Containers







## Chapter 12 Dynamic Memory

**Static memory** is used for local static object, for class static data members, and for variables defined outside any function. **Stack memory** is used for non-static object defined inside functions. 

Object allocated in **static memory or stack memory** are created and destroyed automatically by the compiler.

**Heap memory or free store** refers to the one used by the programs for objects that they dynamically allocate at run time. 对象的生命周期由程序所控制。

### 12.1 Dynamic Memory and Smart Pointers

In C++, dynamic memory is managed by a pair of operators:

1. new: 分配并初始化动态内存，并返回一个指针
2. delete: Operand是指向动态对象的指针，它将对象删掉，并释放相关内存

为了使动态内存变得更简单，C++11提供了library，其中包含了两种smart pointers:

1. shared_ptr
2. unique_ptr



#### 12.1.1 The `shared_ptr` Class

Smart pointers are template, it is part of the memory head file.

```c++
#include <memory>
shared_ptr<string> p1;
```

A default-initialized smart pointers holds a null pointer. 使用它的方式和正常指针一样，通过dereference operator可以返回其指向的对象，当我们将其放入条件判断语句中，作用为判断指针是否为null.

```c++
// if p1 is not null
if (p1 && p1->empty()) {
	*p1 = "hi";
}
```



##### The `make_shared` Function

最安全的去分配并使用动态内存的方式是去调用library function: `make_shared`. 其会返回一个smart_ptr.

```c++
shared_ptr<int> p3 = make_shared<int>(42);  // 42
shared_ptr<string> p4 = make_shared<string>(10, '9');  // 99999999999
shared_ptr<int> p5 = make_shared<int>(); // 0
```

make_shared使用argument去construct an object of the given type.



##### Copying and Assigning shared_ptrs

当我们在为shared_ptr复制与赋值的时候，每个shared_ptr都会保留其他指向同一对象的指针数量。

```c++
auto p = make_shared<int>(42);
auto q(p);
```

每个shared_ptr都有一个引用次数 reference count，每当我们要使用该shared_ptr去初始化别的shared_ptr时，当其被放在right-hand operand的时，将其传入函数，或者作为返回值时，其ref count都会increment. 

如果我们为shared_ptr assign a new value，或者其本身被销毁，又或者其go out of the scope，counter会decrement：

```c++
auto r = make_shared<int>(42);
r = q; // increase the use count of q, but decrease the use count of the object to which r had pointed.
```

每个shared_ptr通过对应类的destructor来destroy their objects.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   

> The class keeps track of how many shared_ptrs point to the same object and automatically frees that object when appropriate.



> If you put shared_ptrs in a container, and you subsequently need to use some, but not all, of the elements, remember to erase the elements you no longer need.



##### Classes with Resources That Have Dynamic Lifetime

There're classes that allocate resources that exist only as long as the corresponding object. 

例如对于vector而言，如若另vector1复制了vector2，那么当vector2被销毁时，vector1不变，因为vector1存的是vector2元素的copy：

```c++
vector<string> v1;
{
	vector<string> v2 = {"a", "an", "the" };
	v1 = v2;
}
// v1 has three elements, which are copies of the ones originally in v2
```

While some classes allocate resources with a lifetime that is independent of the original object.

One example with this shown below:

```c++
Blob<string> b1;
{
	Blob<string> b2 = {"a", "the", "an"};
	b1 = b2;
}
// b2 is destroyed, but the elements in b2 must not be destroyed
// b1 points to the elements originally created in b2.
```

> One common reason to use dynamic memory is to allow multiple objects to share the same state.

The implementation of `StrBlob.h` is as follows:

```c++
#ifndef STRBLOB_H
#define STRBLOB_H

#include <vector>
#include <string>
#include <memory>

class StrBlob {
public:
    typedef std::vector<std::string>::size_type size_type;
    StrBlob();
    StrBlob(std::initializer_list<std::string> il);
    size_type size() const { return data->size(); }
    bool empty() const { return data->empty(); }
    void push_back(const std::string &t) { data->push_back(t); }
    void pop_back();

    std::string& front();
    std::string& back();

private:
    std::shared_ptr<std::vector<std::string>> data;
    void check(size_type i, const std::string &msg) const;
};

#endif 
```

StrBlob.cpp:

```c++
#include "StrBlob.h"

StrBlob::StrBlob(): data(std::make_shared<std::vector<std::string>>()) { }

StrBlob::StrBlob(std::initializer_list<std::string> il) : data(std::make_shared<std::vector<std::string>>(il)) { }

void StrBlob::pop_back() {
    check(0, "pop_back on empty StrBlob");
    data->pop_back();
}

std::string &StrBlob::front() {
    check(0, "front on empty StrBlob");
    return data->front();
}

std::string &StrBlob::back() {
    check(0, "back on empty StrBlob");
    return data->back();
}

void StrBlob::check(StrBlob::size_type i, const std::string &msg) const {
    if (i >= data->size())
        throw std::out_of_range(msg);
}
```

By using the `StrBlob` class created above, the following code would produce a different results compared to the one by the vector.

```c++
StrBlob b1;
{
    StrBlob b2 = {"a", "an", "the"};
    b1 = b2;
    b2.push_back("about");
}
cout << b1.size() << endl;  // produces 4 since b1 and b2 shared the same dynamic memory.
```



#### 12.1.2 Managing Memory directly.

##### Using new to Dynamically Allocate and Initialize Objects

Objects allocates on the free store are unnamed, while new offers no way to name the object that it allocates.

使用new关键字在分配对象时，分配的内存块并不会被直接命名，而其只会返回一个指针，指向分配的内存块。

```c++
int *pi = new int;
```

Dynamically allocated object are default initialized.

We can either use direct initialization or value initialize a dynamically allocated object:

```c++
string *ps1 = new string;  // default initialized to the empty string
string *ps = new string(); // value initialized to the empty string

int *pi = new int(1024);
```

When we provide an initializer inside parentheses, we can use auto to deduce the type of the object we want to allocate. 由于是需要编译器来推断，因此auto关键字后面只能有一个initializer.

```c++
auto p1 = new auto(obj); // ok p1 points to obj.
auto p2 = new auto{a, b, c}; // error: must use parentheses for the initializer.
```



##### Dynamically Allocated const Objects

```c++
const int *pci = new const int(1024);
const string *pcs = new const string;
```

由于是const，因此必须被initialized.

a const dynamic object of a class type that defines a default constructor may be initialized implicitly. 如果有一个默认的构造函数，那么可以被隐式初始化。

由于分配的对象是const，那么由new返回的则是a pointer to const.



##### Memory Exhaustion 

free store的内存可能会耗尽，一旦耗尽，那么new将无法分配内存，并抛出`bad_alloc`类型的异常。我们可以通过定义另一种new**（Placement new）**来避免抛出异常：

```c++
int *p1 = new int;
int *p2 = new (nothrow) int; // if allocation fails, new returns a null pointer.
```



##### Freeing Dynamic Memory

为了避免内存耗尽，我们需要手动释放动态分配的内存: through a delete expression.

```c++
delete p; // p must point to a dynamically allocated object or be null.
```

**It destroys the object to which its given pointer points and it frees the corresponding memory.**



##### Pointer Values and delete

Deleting a pointer to memory that was not allocated by new, or deleting the same pointer value more than once is undefined.

 

##### Dynamically Allocated Objects Exist until They are Freed

A dynamic object managed through a built-in pointer exists until it is explicitly deleted.

```c++
Foo* factory(T arg) {
	return new Foo(arg);
}

void use_factory(T arg) {
	Foo *p = factory(arg);
} // p goes out of scope, but the memory to which p points is not freed!
```

我们必须要手动释放内存，当use_factory函数结束以后，local variable p is destroyed，但是它所指向的内存块并没有被释放。

> Neglecting to delete dynamic memory is known as a memory leak.





##### Resetting the Value of a Pointer after a delete

After the delete, the pointer becomes invalid, which is referred to as a dangling pointer.

A dangling pointer have all of the problems of uninitialized pointers. We can simply delete the pointer just before the pointer goes out of scope. Or we can assign `nullptr` to the pointer.

```c++
int *p(new int(42));
delete p;
p = nullptr;
```



#### 12.1.3 Using shared_ptrs with new

We can initialize a smart pointer from a pointer returned by new.

```c++
shared_ptr<double> p1;
shared_ptr<int> p2(new int(42));
```

The smart pointer constructors that take pointers are explicit. We cannot implicitly convert a built-in pointer to a smart pointer. 因此我必须要使用Direct form of initialization.

```c++
shared_ptr<int> p1 = new int(1024); // error
shared_ptr<int> p2(new int(1024));
```

由于同样原因，我们也无法执行一下转换：

```c++
shared_ptr<int> clone(intp) {
	return new int(p);
}
```



> It is dangerous to use a built-in pointer to access an object owned by a smart pointer, because we may not know when that object is destroyed.



##### `get` member of Smart Pointer

The smart pointer define a function named get that returns a built-in pointer to the object that the smart pointer managed.

其作用是为了适配那些不能够使用smart pointer的代码，但是注意在使用get返回的pointer时，我们不能将其删除 (delete).

Moreover, we cannot bind a another smart pointer to the pointer returned by get:

```c++
shared_ptr<int> p(new int(42));
int*q = p.get();
{
	shared_ptr<int>(q); // undefined: two independent shared_ptrs point to the same memory.
}
int foo = *p;
```

我们不能让两个相互独立的smart pointer指向同一块内存，这就意味着如果一个指针生命周期结束了，那么那块内存将会被释放，而另一个指针就会变成了dangling pointer，它的值是undefined，除此之外，当这个指针被删除以后，那么就会导致内存块被二次删除。



##### Other shared_ptr Operations

Resetting and assign a new value to the shared_ptr:

```c++
p = new int(1024);       // error: cannot assign a pointer to a shared_ptr
p.reset(new int(1024));  // ok: p points to a new object
```

reset updates the reference counts and if appropriate, delete the object to which p points to.

更新引用次数，并删除原来指向的对象。

通常reset操作会和unique一起使用，用来确保该smart pointer是否有其他的user：

```
if (!p.unique()) 
	p.reset(new string(*p));
*p += newVal;
```

判断是否为unique是为了避免在修改该指针的时候会影响到其他指针，也就意味着reset函数在进行修改的时候，相当于仅仅修改自身指针，如果存在多个共享对象，则会让该指针指向另一块内存，从而不影响其他共享指针。



#### 12.1.4 Smart Pointers and Exceptions

使用smart pointers可以确保程序在出现异常时，内存被正确的释放：

```c++
void f() {
	shared_ptr<int> sp(new int(32));
	// code that throws an exception
} // sp would be freed automatically.
```

而如果直接去管理动态内存，使用plain pointer，碰到exception的时候，且在delete语句前，那么其内存不会被正常释放：

```c++
void f() {
	int *ip = new int(42);
	// code that throws an exception
	delete ip;
}
```

如果在中间抛出了异常，那么ip内存无法被释放。



##### Smart Pointers and Dumb Class

对于dumb class而言，由于其非常简单，没有析构函数destructor，因此其内存可能会泄露，而使用shared_ptr则可以完美解决这个问题，从而确保其内存被正确释放了。



##### Using our own deletion code

与此同时，我们还可以定义自己的删除代码：

```c++
void deleter(string* str) {
    cout << "Delete my String" << endl;
    delete str;
}

int main() {
    shared_ptr<string>(new string("Hello World"), deleter);
}
```



#### 12.1.5 unique_ptr

A unique_ptr owns the object to which it points. Only one unique_ptr at a time can point to a given object.

在C++11标准中没有类似make_shared的函数，但在C++14标准中引入了make_unique函数。

以下为两种初始化方式：

```c++
unique_ptr<double> p1 = make_unique<double>(1.3);
unique_ptr<int> p2(new int(42));
```

由于unique_ptr仅仅拥有其指向的对象，因此它不支持copy or assignment.

但我们可以**transfer ownership** from one **(nonconst)** unique_ptr to another calling `release` or `reset`:

```c++
// transfers ownership from p1 to p2
unique_ptr<string> p2(p1.release());  // release makes p1 nullptr

unique_ptr<string> p3(new string("Text"));
// transfers ownership from p3 to p2
p2.reset(p3.release()); // reset deletes the memory to which p2 had pointed.
```

1. Release: it returns the pointer currently stored in the unique_ptr and makes that unique_ptr null.

2. Reset: It repositions the unique_ptr to point to the given pointer. 

   如果unique_ptr本身是一个非空指针，那么它将会先删除自己原先指向的内存，然后再指向the memory to which the given pointer points.

注意，在使用release的时候，一定要接受参数，其返回值一般会被用于初始化别的smart pointer，总之，responsibility一定要被传递到其他的管理对象上，否则我们需要手动去删除指针：

```c++
auto p = p2.release()
delete p;
```



##### Passing and Returning unique_ptrs

Although we cannot copy a unique_ptr, we could copy or assign a unique_ptr that **is about to be destroyed**:

```c++
unique_ptr<int> clone(int p) {
	return unique_ptr<int>(new int(p));
}
```

Copy of a local object:

```c++
unique_ptr<int> clone(int p) {
	unique_ptr<int> ret = make_unique<int>(p);
	// ...
	return ret;
}
```

在这两种情况下，编译器都知道这个对象将要被销毁，在这种情况下，编译器会做一种特殊的copy (p. 535 13.6.2).



##### Passing a Deleter to unique_ptr.

We could override the default deleter in a unique_ptr. The way that unique_ptr manages a deleter is differ from the way shared_ptr does.

Overriding the deleter affects the unique_ptr type as well as how we construct or reset objects of that type. 

我们需要在尖括号中提供deleter的类型：

```c++
unique_ptr<objT, delT> p(new objT, fcn);
```

Therefore we could have the following code:

```c++
void deleter(string* str) {
    cout << "Delete my String" << endl;
    delete str;
}

int main() {
    unique_ptr<string, decltype(deleter)*> ptr(new string("Hello World"), deleter);
}
```

注意，我们只能通过构造函数传入deleter，不能通过make_unique的方法进行。



#### 12.1.5 weak_ptr

A weak_ptr is a smart pointer that does not control the lifetime of the object to which it points. Instead, is points to an object that is managed by a shared_ptr. Binding a weak_ptr to a shared_ptr does not **change the reference count.** 

只要shared_ptr指向的对象结束了，即便weak_ptr指向了它，该对象仍然会被删除。

而由于weak_ptr指向的对象可能被删了，因此我们无法直接访问指向的对象，为了访问该对象，我们需要使用lock函数，如果对象存在，那么将会返回shared_ptr.

而最安全的使用方法是在外面再套一层if语句，从而判定lock是否成功：

```c++
if (shared_ptr<int> np = wp.lock()) {
	// inside the if, np shares its object with p
}
```



1. weak_ptr<T> w(shared_ptr)

   it points to the same object as the shared_ptr shared_ptr. T must be convertible to the type to which shared_ptr points.

2. w = p

   p can be a shared_ptr or as weak_ptr. After the assignment w shares the ownership with p.

3. w.reset()

   Makes w null

4. w.use_count()

   The number of shared_ptrs that share ownership with w.

5. w.expired()

   return true if use_count() is zero, false otherwise

6. w.lock()

   if expired is true, then return null shared_ptr, otherwise returns a shared_ptr to the object to which p points.



### 12.2 Dynamic Arrays

> Most applications should use a library container rather than dynamically allocated arrays since using a container is easier, less likely to contain memory-management bugs, and is likely to give a better performance.



#### `new` and Arrays

We ask new to allocated an array of objects by specifying the number of objects to allocate in a pair of square brackets, which returns a pointer to the first one.

```c++
int *pia = new int[get_size()];
```

我们同样也可以使用type alias to represent an array type:

```c++
typedef int arrT[42];
int *p = new arrT;
```



##### Allocating an Array Yields a Pointer to the Element Type

注意，It is misleading to refer to memory allocated by new T[] as a dynamic array. 由于其返回的只是一个pointer.



##### Initializing an Array of Dynamically Allocated Objects

Objects allocated by new are default initialized. We could value initialize the elements with a pair of parenthesis:

```c++
int *pia = new int[10];      // uninitialized ints
int *pia2 = new int[10]();   // block of ten ints value initialized to 0

string *psa = new string[10];   // block of ten empty strings
string *psa2 = new string[10](); // block of ten empty strings
```

注意，built-in type如何没有initializer，那么它们就是uninitialized，例如int

在新标准中，我们也可以用花括号来进行初始化：

```c++
int *pia3 = new int[10]{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
```



##### It is Legal to Dynamically Allocate an Empty Array

We could use arbitrary expression to determine the number of objects to allocate:

```c++
size_t n = get_size();
int *p = new int[n];
for (int *q = p; q != p + n; ++q) 
	// process
```

如果`get_size()`返回的值是0，代码仍然可以正常运行，这是legal。

他就像off-the-end iterator一样，该指针不能被dereferenced.



##### Freeing Dynamic Arrays

```c++
delete p;    // p must point to a dynamically allocated object
delete[] pa; // pa must point to a dynamically allocated array
```



##### Smart Pointers and Dynamic Arrays

The library provides a version of unique_ptr that can manage arrays allocated by new:

```c++
unique_ptr<int[]> up(new int[10]);
up.release();
```

When up destroys the pointer it manages, it will automatically use delete[].

Its operation is different than those we used previously.

But we could use the subscript operator to access the elements in the array:

```c++
for (size_t i = 0; i != 10; ++i)
	up[i] = i;
```

然而shared_ptr并未给我们提供对动态数组的支持，因此我们需要自己提供deleter：

```c++
shared_ptr<int> sp(new int[10], [](int *p) { delete[] p; });
sp.reset();
```

与此同时，在进行迭代的时候，也会用不同的代码：

```c++
for (size_i i = 0; i != 10; ++i)
	*(sp.get() + i) = i;   // use get to get a built-in pointer.
```



#### 12.2.2 The allocator Class

One problem with the new is that it combines allocation memory with constructing objects int that memory. Similarly, delete combines destruction with deallocation. 

如果能够将allocation和construction分离，这就意味着只有在我们需要去创建对象的时候才去pay the overhead，这能够节省资源。

相反，使用默认的new会让我们至少两次的assign value. 因为它会执行default-initialization，而后由于我们需要去使用，再次会为其赋值。除此之外，没有默认构造函数的类无法被动态分配为数组。



##### The allocator Class

The library allocator class lets us separate allocation from construction. It provides type-aware allocation of raw, unconstructed memory. 

```c++
allocator<string> alloc;
auto const p = alloc.allocate(n); // allocate n unconstructed strings
```

| Members                | Explanations                                                 |
| ---------------------- | ------------------------------------------------------------ |
| `a.allocate(n)`        | Allocates raw, unconstructed memory to hold n objects of type T. |
| `a.deallocate(p)`      | Deallocates memory that held n objects of type T starting at the address in the T* pointer p, which must be the pointer returned by the allocate function. |
| `a.construct(p, args)` | p must be the pointer to type T that points to the raw memory; args is for the constructor. |
| `a.destroy(p)`         | Runs the destructor on the object pointed to by the T* pointer p. |



##### allocators Allocate Unconstructed Memory

The memory an allocator allocates is unconstructed. We use this memory by constructing objects in that memory.

注意，construct函数在C++20中被移除了。

```c++
auto q = p;
alloc.construct(q++, 10, 'c');  // *q is cccccccccc
alloc.construct(q++, "hi");     // *q is hi
```

> We must construct objects in order to use memory returned by allocate. Using unconstructed memory is undefined.

When we finished using the objects, we must destroy the elements we constructed:

```c++
while (q != p)
	alloc.destroy(--q);
```

> We may destroy only elements that are actually constructed.

Once the elements have been destroyed, we can either reuse the memory to hold other strings or return the memory to the system. We free the memory by calling deallocate:

```c++
alloc.deallocate(p, n); // n必须是和在allocate时使用的n相同
```

总结来说，整体流程为：

allocate :arrow_right: construct :arrow_right: destroy :arrow_right: deallocate.



##### Algorithms to Copy and Fill Uninitialized Memory

| allocator Algorithms             | Explanations                                                 |
| -------------------------------- | ------------------------------------------------------------ |
| `uninitialized_copy(b, e, b2)`   | Copies elements from the input range denoted by b(begin) and e(end) iterator into unconstructed, raw memory denoted by b2. |
| `uninitialized_copy_n(b, n, b2)` | Copies n elements from the iterator b into raw memory starting at b2. |
| `uninitialized_fill(b, e, t)`    | Constructs objects in the range of raw memory denoted by b and e with a copy of t. |
| `uninitialized_fill_n(b, e, t)`  | Constructs an unsigned number n objects starting at b with a copy of t. |

One example with this is that we allocate a bunk of memory, with the first half holding the copy of vector data and the other half filling with different data.

```c++
auto p = alloc.allocate(vi.size() * 2);
auto q = uninitialized_copy(vi.begin(), vi.end(), p);
uninitialized_fill_n(q, vi.size(), 42);
```



## Chapter 13. Copy Control

