# 我是从11.13开始学的

## 11.15开始记录。

1、学习了Macro这个变态的语法

```rust
macro_rules! my_macro{
    ($a:expr)=>{
        "Hello ".to_string() + $a
    }
}
```

2、还有就是变量的使用权的转移

```rust
let x=5;
let y=x;//这里就转移了
```

3、最后变量与常量没有理解，第二天再搞