# move入门之基础语法(一) ：模块，注释，常量，结构体
🧑‍💻作者：gracecampo

> 说明：本章将介绍在move中如何去新建模块，代码注释，声明常量，声明结构体语法的使用。

## 新建模块
> 模块的定义： 模块是move的基本构成组件，利用模块特性可以优化代码结构以及功能解耦，模块中定义的所有成员，例如常量，结构体，函数方法都是模块私有的。

模块语法：
```sui move
module package::module_name {
    // module body
} 
```
从以上代码，我们可以看出，当我们想要声明一个模块是，我们需要用 module 关键字后跟 包名：模块名，在{}中添加模块代码。
模块名规范： 
1. 模块名必须包内唯一
2. 模块名应采用snake_case形式的命名格式，即模块名小写，并以下划线分割

通常我们使用 `sui move new 项目名` 命令 即可创建出一个项目模板，其中模块源码写在source文件加下。

## 代码注释的使用
> 代码注释：代码注释在程序开发中非常重要，它用于为相关关键代码或者方法做解释说明，或者用于标识忽略部分代码，提高与其他开发者的沟通效率，方便他人理解代码。
>> 特点： 注释信息不参与编译,提升代码阅读性
### 行注释
> 语法：使用 // 来标识,被双斜杠标记后的行代码不参与编译
>> 通常我们用行注释，来描述变量说明；或注释掉对应代码行，使其不参与编译
```sui move
module package::module_name {
    // 常量说明
    const ENOT_IMPLEMENTED: u64 = 0; //常量说明也可以放在这里
    //该常量被注释，不参与编译，move编译器将或略此行代码
    //const AN_ADDRESS: address = @0x0;
    
} 
```
### 块注释
> 语法：使用/* 开始，以 */ 结束,来标识被包括的代码语句,代码块不参与编译
> > 通常我们用块注释，注释掉对应代码块，在/* 和 */之间的代码，将不参与编译
```sui move
module package::module_name {
    /*可以放在这里*/
    fun /*可以放在这里*/ add_method() : u64 {
        /*可以放在这里*/
        let a:u64 = 10;
        let b:u64 = 10;
        a + b
    }
}
```
### 文档注释
> 语法：使用 /// 开始，为相关代码生成文档说明
> > 通常我们用文档注释，来描述模块，方法，常量，结构体，函数的用途或处理逻辑，基于约定，需放在对应的代码上
```sui move
///模块功能说明
module package::module_name {
    ///方法功能说明
    fun  add_method() : u64 {
        ///变量说明
        let a:u64 = 10;
        let b:u64 = 10;
        a + b
    }
}
```
**文档注释和行注释类似，只不过对应的注释信息约定需要放于需要描述的代码上一行**

## 如何声明常量
> 常量的定义：常量是在模块中，属于模块私有的的成员，一旦声明，值便不可再修改;
> 我们通常使用const关键字来声明一个常量，常量必须以大写字母开头，编译器强制要求

> 常量必须以大写字母开头，编译器强制要求，我们通常约定为全大写，以下划线分割
>> 语法： const CONST_NAME: type = value;
```sui move
module package::module_name {
    // 此为一个常量
    const ENOT_IMPLEMENTED: u64 = 0;

} 
```
> 应用：因为常量是模块私有的，如果我们需要在外部引用它，可以通过声明一个函数，供外部访问常量

```sui move
module package::module_name {
    // 此为一个常量
    const ENOT_IMPLEMENTED: u64 = 0;
    public fun enot_implemented(): u64 { ENOT_IMPLEMENTED }
} 
```

## 如何声明结构体
>结构体：在move语言中，根据需求自定义对象结构类型，可以定义其能力以及包含的字段，是在日常开发中必须掌握的一个知识点

限制：

1.结构体默认为私有，其字段也是私有的，

2.结构体不支持递归引用，即不能包含自身类型的字段
```sui move
module package::module_name {
    // 结构体
    public struct Person has copy, drop{
        age: u8
    }
} 
```
在上述示例中，我们定义了一个Person的结构体，它拥有copy, drop能力，声明了age字段。
如果要开放外部可见，结构体必须加必须加public关键字，并提供一个公共构造函数，访问结构体字段提供对应的公共函数来访问

```sui move
    // 提供一个公共构造函数
    public fun new_person(age: u8): Person {
        Person { age }
    }
    // 提供一个公共函数来访问结构体的字段
    public fun get_age(person: &Person):u8{
        person.age
    }
```

创建和使用实例,并返回age字段值
```sui move
    ///创建和使用实例
    public  fun use_person():u8{
        let person = Person {age:16};
        //访问字段
        person.age
    }
```

解构结构体,并返回age字段值
```sui move
    ///解构结构体
    public  fun unpack_person():u8{
        let person = Person {age:16};
        let Person { age } = person;
        age
    }
```
💧  [HOH水分子公众号](https://mp.weixin.qq.com/s/d0brr-ao6cZ5t8Z5OO1Mog)

🌊  [HOH水分子X账号](https://x.com/0xHOH)

📹  [课程B站账号](https://space.bilibili.com/3493269495352098)

💻  Github仓库 https://github.com/move-cn/letsmove


