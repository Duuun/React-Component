安装各种插件等等
**直接运行ts**

1. 装包：npm install -g ts-node
2. 运行：ts-node you.ts 

**将ts转为js**
tsc hello.ts

**JQury**
装包：npm install --save @types/jquery

## TypeScript
命令行中将ts转为js：tsc hello.ts
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1710121529709-7f62bbc6-c07e-4dfd-8523-9342295312a3.png#averageHue=%23242931&clientId=u60e49fe2-df7a-4&from=paste&height=144&id=hQXd8&originHeight=194&originWidth=425&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11126&status=done&style=none&taskId=u79d195c6-7ed9-46ff-a983-2701df7acbf&title=&width=315)![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1710121545107-ce8c6bba-67a5-47b6-8356-af14fdf9fe20.png#averageHue=%23242931&clientId=u60e49fe2-df7a-4&from=paste&height=145&id=rY7f3&originHeight=193&originWidth=394&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=10935&status=done&style=none&taskId=ub9b2d9aa-7263-40cf-9c94-d87b038d763&title=&width=295.20001220703125)

## ts数据类型
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1710123004070-c9a3e88b-55d7-4ec8-9cea-beb8f0a3b250.png#averageHue=%23453959&clientId=u60e49fe2-df7a-4&from=paste&height=26&id=P5zAl&originHeight=33&originWidth=693&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=5910&status=done&style=none&taskId=u9984eba3-e586-4585-86c4-47f732117de&title=&width=554.4)
Tips：

1. **使用联合类型：**let num: number | undefined = undefined
2. 关闭严格空值检查
```javascript
{
  "compilerOptions": {
    "strictNullChecks": false
  }
}

```

### 原始数据类型
```typescript
// 1. 确定的基本数据类型
let isDone: boolean = false

let age: number = 20
let binaryNumber = 0b111   //0b开头二进制

let firstName: string = 'Dun'
let msg: string = `my name is ${firstName} , age is ${age}`

let u: undefined = undefined
let n: null = null

let num: number | undefined = undefined

// 2.不确定类型的，设为any，他调用方法的返回值类型也是any
let notSure: any = 4
notSure = 'this is notSure'

notSure.getName()


// 3.联合类型：可以指定想要的那几个类型

let numOrString: number | string = 23
numOrString = 'abc'
// numOrString = false 报错
```

### 数组
```typescript
// 4. 数组
let arrayOfNumber: number[] = [1, 2, 3]
arrayOfNumber.push(4)
// arrayOfNumber.push('3')  报错

// 类数组
function test() {
    console.log(arguments)
    arguments.length
    arguments[2]
    // arguments.foreach  报错，不能调用数组方法
}

// 元祖：合并不同数组类型的数组 
// 且项要对应，只能有两项
let user: [string, number] = ['Dunn', 123]
user = ['abc', 6666]

```

### interface对象
```typescript
// interface：对对象的契约/规定，描述对象长什么样
// 1. readonly ：只读与const的区别：const修饰变量，readonly修饰类中的属性
// 2. 定义的属性必须跟接口中的一致，不能多也不能少
// 3. 对可选的加?即 age？: number;
// 接口中要用;来分割

interface Person {
   readonly id:number;
   name: string;
    age？: number;
}

let dunn: Person = {
    id:1,
    name: 'dunn',
    age: 18
}

// dunn.id = 2   报错，不能修改只读属性
```

### function
注意声明式和函数式的写法
**函数声明式写法**
```typescript
// 1. 传的参数必须跟定义完全一致
// 2. 可选的参数也加?，且必须定义在最后
// 3. 设置参数的默认值,传了就用，没传使用默认值
// 4. 如果想使用默认值，又想传可选参数，则默认值处写undefined

function add(x: number, y: number, j: number = 10, z?: number): number {
    // return x + y
    if (typeof z === 'number') {
        return x + y + j + z
    } else {
        return x + y + j
    }
}

let result1 = add(1, 2)
let result2 = add(1, 2, 3)
let result3 = add(1, 2, undefined, 4)  //17
let result4 = add(1, 2, 3, 4)

console.log(result3)

```

**函数表达式写法**
```typescript
// 函数表达式写法
// 函数本身也有类型了
const add = function (x: number, y: number, j: number = 10, z?: number): number {
    // return x + y
    if (typeof z === 'number') {
        return x + y + j + z
    } else {
        return x + y + j
    }
}

// const add2:string = add  报错
// 要把每个参数类型以及返回说清楚，不能设置默认值
const add2: (x: number, y: number, j: number, z?: number) => number = add
```
### 类
Tips：

1. 注意类的写法，继承和重写
2. 在重写时，子类constructor的参数也要加类型

```typescript
// 类
class Animal {
    name: string;
    constructor(name: string) {
        this.name = name
    }
    run() {
        return `${this.name} is running`
    }
}

const snake = new Animal('Dunn')
console.log(snake.run())

// 继承
class Dog extends Animal {
    bark() {
        return `${this.name} is barking`
    }
}

const xiaogou = new Dog('xiaogou')

console.log(xiaogou.run())
console.log(xiaogou.bark())


// 重写父类方法
class Cat extends Animal {
    constructor(name: string) {   //这里要加类型string，不然会报错
        super(name)
        console.log(this.name)
    }
    run() {
        return 'meow,' + super.run()
    }
}

const miaomiao = new Cat('miaomiao')
console.log(miaomiao.run())
```

**修饰符**
1.public：公共修饰符，表示成员可以被任何地方访问，默认的修饰符。
2.private：私有修饰符，表示成员只能在类的内部被访问。
3.protected：保护修饰符，表示成员可以在类的内部及子类中访问,但不能在外部访问.
4.readonly:只读修饰符，表示成员只能在声明时或者构造函数中初始化，之后不能修改。
5.static：静态修饰符，表示成员属于类本身而不是实例，通过类名直接访问。
6.abstract:抽象修饰符，用于声明抽象方法和抽象类，抽象类不可以被实例化，只能作为其他类的基类，抽象方法必须要在派生类中实现

```typescript
class Animal {
    readonly name: string;
    static categoies: string[] = ['mammal', 'bird']
    static isAnimal(a: any): a is Animal {     //这个位置
        return a instanceof Animal
    }

    constructor(name: string) {
        this.name = name
    }
    run() {
        return `${this.name} is running`
    }
}


const snake = new Animal('Dunn')
console.log(snake.run())

console.log(Animal.categoies)
console.log(Animal.isAnimal(snake))

// [ 'mammal', 'bird' ]
// true
```
**Tip：**
**关于这个    **
```typescript
static isAnimal(a: any): a is Animal {  //这个位置
        return a instanceof Animal
    }
// 可以替换成
static isAnimal(a: any): boolean {  //这个位置
        return a instanceof Animal
    }
```


**a is Animal 表示函数返回的是一个布尔值，所以也可以直接用boolean来代替**

### interface接口
```typescript
interface Radio {
    switchRadio(): void;
}

interface Battery {
    checkBatteryState()
}

interface RadioWithBattery extends Radio {
    checkBatteryState()
}

class Car implements Radio {
    switchRadio()
}

class CellPhone implements RadioWithBattery {
    checkBatteryState()
}


// 或
// class CellPhone implements Radio,Battery {
//     switchRadio()
//     checkBatteryState()
// }
```

### 枚举
```typescript
enum Direction {
    Up,
    Down,
    Left,
    Right
}

console.log(Direction.Up)  // 0
console.log(Direction[0])  // Up
```
还可以给他赋初值：后面的项也会跟着变，如果是给中间项赋值，那么前面的还是默认值
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1710148177015-9441a16c-411d-49bb-ace8-25cf4d7105bd.png#averageHue=%23242931&clientId=ue825ee0e-b756-4&from=paste&height=119&id=xI5DL&originHeight=141&originWidth=429&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=7723&status=done&style=none&taskId=u862cb452-f5fd-4090-96b3-d8fb45ca334&title=&width=363.20001220703125)![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1710148203507-45d441f5-18ea-412c-a6f8-35b3bda46253.png#averageHue=%23242931&clientId=ue825ee0e-b756-4&from=paste&height=116&id=Gchyw&originHeight=158&originWidth=371&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=8074&status=done&style=none&taskId=u1a412ab8-f740-41ec-a0f0-9f530424da3&title=&width=272.8000183105469)
还可以赋字符串等
```typescript
enum Direction {
    Up = 'Up',
    Down = 'Down',
    Left = 'Left',
    Right = 'Right'
}

const value = 'Up'
if (value === Direction.Up) {
    console.log('go up!')
}

//go up!
```

### 泛型
动态的来确定值的类型
#### 1.函数泛型
```typescript
// 此时arg为any，传入为number，但结果为string，为了规范化使用泛型
function echo(arg) {
    return arg
}
const result: string = echo(123)

// 这样输入输出的值就一致
function echo1<T>(arg: T): T {
    return arg
}
const result1 = echo1(123)

// 多种类型
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]]
}

const result2 = swap(['string', 123])   //传入一个数组
console.log(result2)   //[ 123, 'string' ]
```

#### 2.约束泛型

- 约束泛型是对泛型进行限制，使得类型参数必须满足某些特定条件。
- 通过约束泛型，可以限制泛型类型参数的范围，例如只允许指定某种类型或其子类。
- 约束泛型可以在编译时提供更多的类型检查，避免一些潜在的运行时错误。

想解决这类问题，要使得arr上有相应的属性(length)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1710151890847-47e8202d-3b7e-40ac-9154-4a2962c750ad.png#averageHue=%23232830&clientId=ue825ee0e-b756-4&from=paste&height=98&id=sHswV&originHeight=123&originWidth=567&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=9984&status=done&style=none&taskId=u4341de75-0ca9-4cda-a136-9ceb3e98a0d&title=&width=453.6)
**方法：**

1. 将T的类型规定为数组，数组上有length
```typescript
function echoWithArr<T>(arg: T[]): T[] {
    console.log(arg.length)
    return arg

}
```

2. 用接口实现约束泛型，接口里规定length属性

类型继承接口
```typescript
// 接口实现
interface IWithLength {
    length: number    //设置了length属性
}

// 调用这个函数的必须要有IWithLength接口中规定了的length属性
function echoWithLength<T extends IWithLength>(arr: T): T {
    console.log(arr.length)
    return arr
}

const result6 = echoWithLength('str')  //字符串也有length
const result7 = echoWithLength([1, 2, 3])
const result8 = echoWithLength({ length: 6, name: 'jack' })
// const result9 = echoWithLength(123) 报错，123身上没有length属性
```

之前是在函数中，函数的参数和返回值
#### 3.类的泛型
模拟一个队列的弹入弹出：想要加入的和弹出的类型是同一种
```typescript
// 模拟一个队列
class Queue{
    private data = [];
    push(item){
        return this.data.push(item)
    }
    pop(item){
        return this.data.shift()

    }
}

const queue = new Queue()
queue.push(1)
queue.push('123')
queue.pop().toFixed()
queue.pop().toFixed()
//这样会使得队列中1和'123'都能加入队列，且str错误的使用了number的方法
```

想要加入的和弹出的类型是同一种，采用**类的泛型**
```typescript
// 3. 类的泛型

// 模拟一个队列
class Queue<T> {
    private data: T[] = [];   //这个位置要给data一个初始值[]
    push(item: T) {
        return this.data.push(item)
    }
    pop(): T | undefined {
        return this.data.shift()

    }
}

const queue = new Queue<number>()
queue.push(1)
console.log(queue.pop()?.toFixed())

const queue2 = new Queue<string>()
queue2.push('abc')
console.log(queue2.pop()?.length)
```
Tips：
 **   private data: T[] = [];  **
这个位置要给data一个初始值[]，不能直接写成**private data: T[];** 而没有给它一个初始值，那么 **data** 将被默认初始化为 **undefined**。这会导致在尝试对 **data** 进行操作时（如 **push** 或 **pop**），由于 **data** 是 **undefined**，会遇到 **TypeError** 或类似的错误。


**过程版**
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1710156276301-054fe91b-a506-43f2-b17a-00e541481668.png#averageHue=%2323282f&clientId=ue825ee0e-b756-4&from=paste&height=355&id=SSZIO&originHeight=444&originWidth=1227&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=39382&status=done&style=none&taskId=u49adc9fa-4dfa-4256-a2c1-c2e48199bad&title=&width=981.6)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/42577230/1710157540971-05b3b301-ceb9-474f-8dfc-0066c81010f7.png#averageHue=%233b3148&clientId=ue825ee0e-b756-4&from=paste&height=20&id=ypk1d&originHeight=25&originWidth=476&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=9288&status=done&style=none&taskId=u512597f5-7be0-4160-a2ee-7e7383525ba&title=&width=380.8)

1. 类型'T'的参数不能赋给类型“never”的参数

问题：**private data = [];** 中的 **data** 变量的类型会根据等号右侧的初始值来推断。在这种情况下，初始值是一个空数组 **[]**，TypeScript 会根据这个初始值推断出 data 的类型为 **any[]**，也就是任意类型的数组。
解决办法：显式指定 **data** 变量的类型为 **T[]**，即一个泛型类型数组

2. 不能将undefined赋给T

问题：此时值可能为undefined
解决办法： T | undefined

3. queue.pop()对象可能未定义

问题：pop() 方法返回的值的类型是 **T | undefined**，因为队列可能为空，从而返回 **undefined**。因此，调用 **toFixed()** 方法可能会在运行时导致 **TypeError**，因为 **undefined** 没有 **toFixed()** 方法。
解决办法：使用可选链操作符**?.**来控制，如果 **pop()** 方法返回 **undefined**，**?.** 操作符会导致表达式直接返回 **undefined**，而不会继续执行 **toFixed()** 方法。

#### 4.接口泛型
```typescript
// 4. 接口泛型
interface KeyPair<T, U> {
    key: T;
    value: U
}

let kp1: KeyPair<number, string> = { key: 123, value: 'abc' }
let kp2: KeyPair<boolean, number> = { key: true, value: 123 }

// 接口内部属性可以多个
interface a<T, U> {
    key: T;
    value: U
    item: T
}

let kp3: a<number, string> = { key: 123, value: 'abc', item: 456 }


补充
// 数组封装的接口
let arr: number[] = [1, 2, 3]
let arr1: Array<number> = [1, 2, 3]
```

#### 5.函数接口泛型
```typescript

// 普通
 interface IPlus {
    (a: number, b: number): number
}

function plus(a: number, b: number): number {
    return a + b
}

const a : IPlus = plus 


// 泛型
interface IPlus<T> {
    (a: T, b: T): T
}

function plus(a: number, b: number): number {
    return a + b
}

function connect(a: string, b: string) {
    return a + b
}

const a: IPlus<number> = plus
const b: IPlus<string> = connect
```

### 类型别名和断言
#### 1.类型别名
常用场景：联合类型中
```typescript
// type aliases

type PlusType = (x: number, y: number) => number

function sum(x: number, y: number) {
    return x + y
}

// const sum2: (x: number, y: number) => number = sum
const sum2: PlusType = sum

// 常用在联合类型中
// 如果一个函数可能有的类型是string和function

type NameResolver = () => string
type NameOrResolver = string | NameResolver

function getName(x: NameOrResolver): string {
    if (typeof x === 'string') {  //注意大小写
        return x
    }
    else {
        return x()
    }

}
```

#### 2.断言
input as String
<String>input
不是类型转换，也不能断言成其他类型
不清楚这个函数在干吗
```typescript
// type assertion
// 当在还不确定类型时就要访问
function get(input: number | string): number {
  // 也不能断言成其他类型
  // return <boolean>input 错
  
    // input.length 想直接访问不行，要人为的告诉计算机我们的是string，要用大写
    /* 1.    
        const str = input as String
        if (str.length) {
            return str.length
        } else {
            const number = input as Number
            return number.toString().length
        } */
  
    // 2.简写
    if ((<String>input).length) {
        return (<String>input).length
    } else {
        return input.toString().length
    }
}
```





