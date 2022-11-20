# 我是从11.13开始学的

## 11.14开始记录。

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

## 11.15 星期二

1、if 结构语法

2、简单的函数

3、所有权和借用，没有理解号，费头发的地方来了😭

## 11.16 星期三

1、所有权和借用，理解了一点

```rust
// Should not take ownership
fn get_char(data1: &String) -> char {//借用&
    data1.chars().last().unwrap()
}		//data1没有失效，&只是暂时借用；相当于使用完毕，物归原主，有借【有】还
fn get_return(data:String) ->String{//拥有所有权
    println!("{}", data);
    data
}		//通过返回值将所有权归还，有借【有】还
// Should take ownership
fn string_uppercase(mut data: String) {//拥有所有权
    data = data.to_uppercase();

    println!("{}", data);
}		//data失效了，有借【无】还
```

2、现在是第二天0.15，要睡了，就看了这一个知识点

## 11-18 星期五

1、数组切片：x..y 表示 [x, y) 的数学含义。 ..  两边可以没有运算数。

2、[用模式匹配解构元组](https://course.rs/basic/compound-type/tuple.html#用模式匹配解构元组)

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
//The value of y is: 6.4
```

3、结构体，和 JavaScript 一样的。

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
let user=User{
    active:true,
    username:String::from("John"),
    email:String::from("123@qq.com"),
    sign_in_count:123;
}
```

## 11-20：星期天

1、利用已有Struct创建新Struct

2、Struct's implement：https://www.modb.pro/db/248301，如果创建失败：`panic!("error");`

3、match ==> c/c++ switch

```rust
fn process(&mut self, message: Message) {
        // TODO: create a match expression to process the different message variants
        match message {
            Message::Echo(str) => self.echo(str),
            Message::Move(p) => self.move_position(p),
            Message::Quit => self.quit(),
            Message::ChangeColor(color) => self.change_color(color),
        }
    }
```



