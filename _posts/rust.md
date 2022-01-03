

## Rust 字符串拼贴

### 结论

使用方法

```
let mut s1 = "Hello,".to_string();
let s2 = "world".to_string();
s1 += &s2;

```
### 应用

在应用的时候遇到了其他的问题。实际的应用场景当中，我想在一个

```
let file_path = &options.src.to_string()
```
### 原理

[参考](https://zhuanlan.zhihu.com/p/24486743)

当我试图+= 两个&String

```
help: to_owned() can be used to create an owned `String` from a string reference. String concatenation appends the string on the right to the string on the left and may require reallocation. This requires ownership of the string on the left.
```

[doc](https://doc.rust-lang.org/std/borrow/trait.ToOwned.html)里面的to_owned相关内容
### Rust中的String和Str

[参考](https://www.dyike.com/2018/09/22/rust-string-vs-str/)

## Rust中将vec的变量类型更换成另外一种类型

[参考](https://stackoverflow.com/questions/48308759/how-do-i-convert-a-vect-to-a-vecu-without-copying-the-vector)


## 在generic中规定多个数据类型

### 背景
在一个使用了generic函数中，想要使用一个`checked\_add/checked\_sub` 函数。然而因为`checked\_add/checked\_sub` 只对部分的数据类型成立，所以需要指定T的数据类型。



#### 仍然使用generic的解决方法

编译器提示的修改方式：
![](https://note.youdao.com/yws/api/personal/file/WEB0f807225fe4745b9c510da5e5afcde68?method=download&shareKey=a0d50ed81657b6d31892391a42386353)

根据编译器提示的修改方式进行修改之后出现的新问题：

[与这个stackoverflow](https://stackoverflow.com/questions/69032551/in-rust-how-can-i-restrict-a-generic-t-to-allow-modulus)类似，最终我也采用了这个解决方法，convert the constant into `T` explicitly.

![](https://note.youdao.com/yws/api/personal/file/WEB1b99aa8ea7d588a86f28c553126ff49d?method=download&shareKey=f417b0db1542c11198a544bcbd6b7508)

想了解<> 里面+来+去的是什么东西，参考[doc](https://doc.rust-lang.org/book/ch10-02-traits.html)中的一章。

想了解std::ops::Add<Output ={integer}>是怎么一回事，参考[doc](https://doc.rust-lang.org/std/ops/trait.Add.html)中的另外一部分。

`Output`是一个`Associated Type`，介绍是`The resulting type after applying the + operator`

Method not found in T

#### 改成使用trait方法

[参考这篇Stackoverflow](https://stackoverflow.com/questions/40776020/is-there-any-way-to-restrict-a-generic-type-to-one-of-several-types)提供了另外一个思路

## 一个打印变量类型的函数

```
    fn print_type_of<T>(_: &T) {
        println!("{}", std::any::type_name::<T>())
    };

```

## Can a function return different types depending on conditional statements in the function?

[stackoverflow](https://stackoverflow.com/questions/33390395/can-a-function-return-different-types-depending-on-conditional-statements-in-the)



