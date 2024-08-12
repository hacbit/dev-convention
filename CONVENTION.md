>   ⚠本文档只是基于**代码可读性**提出的一些约定，请酌情考虑使用本约定，你可以只使用其中的部分约定甚至可以无视本约定，一切取决于你或者你的团队



## Menu:

-   [标点符号](#标点符号)
-   [布尔变量的命名&使用](#布尔变量的命名使用)
-   [函数签名](#函数签名)
-   [函数要表达【是否，有】等类似语义](#函数要表达是否有等类似语义)
-   [函数的返回具有【可空】语义](#函数的返回具有可空语义)
-   [文档注释](#文档注释)



## 标点符号

`,` , `;`, `:` 等分隔符，**后面必须**接空格或者换行

`=`, `>=` , `==` 等二元操作符，**前后必须**留空格或者换行

**examples:**

```Cpp
// Cpp

// Bad
int a=5;
for(int i=0;i<a;i++) cout<<i<<'\n';

// Good
int a = 5;
for (int i = 0; i < a; i++) cout << i << '\n';
```



[Back Menu](#Menu)



## 布尔变量的命名&使用

布尔变量，顾名思义是值得类型为 `bool` 的变量。

### 命名

命名必须以 `is` 开头。

**examples:**

```rust
// Rust
let a = true; // bad
let is_true = true; // good
```



```csharp
// C#
public struct MyStruct
{
    public bool IsTarget; // good
}
```

### 使用

如果要在表达式中表示**非**的逻辑，对于使用 `!` 来表示**非**的语言，不应该使用 `!` ，而应该写成 `== false` 的形式

如果要表达 **true** ，也仍然推荐使用 `== true` 

**examples:**

```rust
// Rust

if is_win == true {
    // do something
}
if is_exit == false {
    // do something
}

// this is not recommended
if is_win {}
// the following is a bad!!!
if !is_exit {}
```

>   对于 Python 这种使用 `not` 关键字的，那么可以不用遵守本约定

**examples:**

```python
# Python

# the following is recommended
if not is_exit:
    pass
# this is recommended, too
if is_exit == False:
    pass
```



[Back Menu](#Menu)



## 函数签名

>   为了避免有人不清楚【函数签名】是什么，这里简单说一下：其实就是定义了函数/方法的输入，返回等信息，比如对于C语言，`int add(int left, int right)` 这就可以称为一个函数签名；比如Rust语言，那么可以是 `pub fn registry(&mut self, name: &str) -> &mut Self` 

如果整个函数写在一行太长了（标准可以自己定，比如80个字符左右？）

那么需要把每个参数换行

**examples:**

```cpp
// Cpp

// Bad
void foo(std::string str, std::vector<uint8_t> vec, int offset, int arg, std::variant<char, int> arg2) {
    
}

// Good
void foo(
	std::string str,
    std::vector<uint8_t> vec,
    int offset,
    int arg,
    std::variant<char, int> arg2
) {
    
}
```



```rust
// Rust

// Bad
pub fn foobar(query: &HashMap<String, String>, name: Cow<'_ str>, flags: usize, offset: usize) -> Option<String> {
    
}

// Good
pub fn foobar(
	query: &HashMap<String, String>,
    name: Cow<'_ str>,
    flags: usize,
    offset: usize,
) -> Option<String> {
    
}
```

>   另外，对于Cpp这种函数**返回类型在前面**的，**如果返回类型太长，返回类型也应该换行**
>

**examples:**

```cpp
// Cpp

// Bad
THIS_IS_A_VERY_VERY_VERY_LONG_RETRUN_TYPE function_name(int arg1, int arg2, int arg3, int arg4) {
    
}

// Good
THIS_IS_A_VERY_VERY_VERY_LONG_RETRUN_TYPE
function_name(int arg1, int arg2, int arg3, int arg4) {
    
}
```



[Back Menu](#Menu)



## 函数要表达【是否，有】等类似语义

### 命名

通常使用 `is` 或者 `has` 开头来表达【是否】和 【有，存在】

### 返回类型

**返回类型**必须为 `bool` 

**examples:**

```c
// C

bool is_reachable() {
    return true;
}
```



```c#
// C#

public static bool HasCookie()
{
	return true;
}
```



[Back Menu](#Menu)



## 函数的返回具有【可空】语义

### 命名

函数命名应该以 `try` 开头

### 返回类型

对于类似 C# 这种可以用 `out` 定义出参，那么用户需要的类型应该在 `out` 定义，函数返回类型应为 `bool` 类型

否则应该返回类似 Rust 的 `Option<T>` 这种可以表示**可能存在可能不存在**语义的类型；

**examples:**

```c#
// C#

public bool TryGetSingleton(out Entity entity)
{
    entity = null;
    return false;
}

// usage
if (TryGetSingleton(out var entity) == false)
{
    // Create a new entity
    return;
}
// use entity do something

```



```rust
// Rust

pub struct MyStruct {
    table: Vec<usize>,
}

impl MyStruct {
    #[inline]
    pub fn try_get(&self, offset: usize) -> Option<usize> {
        if offset < table.len() {
            Some(table[offset])
        } else {
            None
        }
    }
}

// usage
if let Some(val) = my_struct.offset(1) {
    // do something
}
```



[Back Menu](#Menu)



## 文档注释

**每一个公开的**类型，函数，方法，全局变量等，**都必须有文档注释**

私有的字段，全局变量，类型，函数等，可以没有文档注释

如果不知道写什么可以先写个 `Usage: ` 占位，以后再补充

**examples:**

以下是展示Rust语言写文档注释

可以简单概况一下功能，然后对于一些不是特别直观的，建议补充用例

```rust
// Rust

/// Wrapper of type [`Vec<T>`]
#[derive(PartialEq, Debug)]
pub struct VecWrapper<T> {
    /// Raw vec
    /// 
    /// Private fields can have no documentation comments
    vec: Vec<T>,
}

impl<T> VecWrapper<T> {
    /// Convert a raw [`Vec`] to [`VecWrapper`]
    pub fn new(vec: Vec<T>) -> Self {
        Self { vec }
    }
    
    /// Try to get mutable reference of element by index
    /// 
    /// # Example
    /// ```ignore
    /// let mut vw = VecWrapper::new(vec![1_usize]);
    /// if let Some(first) = vw.try_get_mut(0) {
    ///     *first = 2;
    /// }
    /// assert_eq!(vw, VecWrapper::new(vec![2]));
    /// ```
    pub fn try_get_mut(&mut self, index: usize) -> Option<&mut T> {
        self.vec.get_mut(index)
    }
}
```



[Back Menu](#Menu)
