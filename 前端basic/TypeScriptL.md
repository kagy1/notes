# TypeScript

## js的缺点

js是弱类型语言，可以随时变换类型

## js的类型标注

```javascript
/**
* @param 
* @return {string}
*/
function a() {
    return "hello world"
}
```



## 认识TypeScript

- js无法在代码编译期间发现错误

错误出现的越早越好，能在写代码的时候发现错误，就不要在代码编译时发现错误。（ide的优势就是在代码编写过程中帮助我们发现错误）

- 为了弥补JavaScript类型约束上的缺陷，增加类型约束

2014年，facebook推出了flow对js进行类型检查； vue2/react => flow

同年，Microsoft微软也推出了Typescript1.0；现在ts已经完全胜出。

- TypeScript是拥有类型的JavaScript超集

不仅仅增加了类型约束，而且包括一些语法的扩展，比如枚举类型(Enum)、元祖类型(Tuple)等;

## TypeScript转js

- bable

 Babel是一个 JavaScript 编译器(转换引擎),将高版本语法转换成低版本语法。

一般使用bable 把ts转js

- tsc

```node.js
tsc xxx.ts   // 把ts文件编译为js文件
```



## 类型推导

let进行类型推导，推导出通用类型

const进行类型推导，推导出来为字面量类型



`typeof` 操作符返回的类型信息是基于 JavaScript 的运行时类型，而不是 TypeScript 的静态类型

```typescript
const str = 'hello wolrd';
console.log(typeof str); // string
const arr = ['a', 'b', 'c']
console.log(typeof arr); // object
```

1. 对于 `str`，它是一个字符串字面量，在 JavaScript 中，字符串的运行时类型就是 `'string'`。
2. 对于 `arr`，它是一个数组，在 JavaScript 中，数组是一种特殊的对象，因此 `typeof arr` 返回 `'object'`。

```typescript
const str = 'hello world';
type StrType = typeof str; // StrType 的类型是 'hello world'

const arr = ['a', 'b', 'c'];
type ArrType = typeof arr; // ArrType 的类型是 ['a', 'b', 'c']
```



### 类型校验

```ts
// 参数为一个对象，含有length属性且为数值类型
function getLength(args:{length : number}){
    return args.length;
}


const info = {
   	length:100
}

getLength(info);
getLength("hello")
getLength(['a','b','c'])
```



## 类型

### 数组

有两种写法

```typescript
let names: string[] = ["aaa","bbb"]
let nums: Array<number> = [123,111]
```



### 对象

```typescript
type InfoType={
    name: string
    age: number
}
let info: InfoType = {
    name："why",
    age: 18
}
// 或者
interface InfoType{
  name: string;
  age: number;
}

let info: InfoType = {
  name: 'Tom',
  age: 20
}
```

object对象类型可以用于描述一个对象，但是从myinfo中不能获取和设置数据

```typescript
const myInfo:object = {
    name:"li",
    age: 16
}

myInfo["name"]= "zhang"  // 元素隐式具有 "any" 类型，因为类型为 ""name"" 的表达式不能用于索引类型 "{}"。类型“{}”上不存在属性“name”。
console.log(muInfo.age)  // 类型“object”上不存在属性“age”。
```

### 类型

#### Symbol类型(js)

可以通过symbol类型定义相同的名称，因为symbol函数返回的是不同的值

```javascript
const person = {
    name : "li",
    name : "zhang"  //报错
}

// symbol
const s1: symbol = Symbol("name")
const s2: symbol = Symbol("name")

const person = {
    [s1]: "li",
    [s2]: "zhang"
}
```



#### null & undefined

在typescript里，它们各自的类型也是undefined 和 null

null和undefined是所有其他类型的子类型，他们可以赋值给其他类型  

```typescript
let n: null = null
let u: undefined = undefined
```



```javascript
console.log(typeof null === 'null') // false
console.log(typeof null === 'object')  // true
console.log( typeof undefined === 'undefined')  // true 
```

虽然 `typeof null` 返回 `"object"`,但这实际上是 JavaScript 的一个历史遗留 bug。`null` 本身是一个独立的类型

`typeof null` 返回 `"object"` 是因为在 JavaScript 的最初版本中，`null` 被设计为表示一个空的对象引用。后来虽然这个设计被认定为是缺陷，但由于兼容性问题一直没有修复。



```javascript
0bject.prototype.toString.call(null)
```



通过添加`strictNullChecks: true`可以获得更严格的类型检查，null和undefined只能赋值给自身



#### unKnown类型

用于描述类型不确定的变量

和any类型有点类似，但是unKnown类型的值上做任何事情都是不合法的

<div style="color:red">unKnown类型在默认情况下进行任何操作都是非法的</div>

必须进行类型的校验（缩小）

```typescript
let foo: unknown = 'aaa';
foo = 123

console.log(foo.length) //非法

if(typeof foo === "string"){ // 类型缩小
    console.log(foo.length)
}
```



#### void类型

如果一个函数返回值是void类型，也可以返回undefined

当基于上下文的类型推导推导出返回类型为void的时候，并不会强制函数一定不能返回内容。



#### never类型

实际开发时很少会用到，一般是推导出为never

封装框架，工具库时会用到

**never表示永远不会发生值得类型，比如一个函数**

- 如果一个函数中是一个死循环或者抛出一个异常，这个函数不会返回东西，写void或者其他类型作为返回值都不合适，可以使用never类型。

1. 表示永不返回的函数

```typescript
function throwError(message: string): never {
  throw new Error(message);
}
```



封装框架

```typescript
function handleMessage(message: string | number){
    switch(typeof message){
        case "string":
            console.log(message.length);
            break;
        case "number":
            console.log(message);
            break;
        default:
            const check: never = message;
    }
}
```

如果不加never，这种情况下message添加boolean类型，不会报错。

`never` 类型表示不应该有任何值能赋给它。



#### tuple 元组类型

允许表示一个已知元素数量和类型的数组，各元素类型不必相同

```typescript
const tInfo: [string,number,number] = ["why",18,1.88];
const item1 = tInfo[0];
const item2 = tInfo[1];

const info:(string | number)[] = ["why",18,1.88];  // 数组表示
```

一般使用对象类型保存

```typescript
cosnt info2 = {
    name:"why",
    age:18,
    height:1.88
}
```



#### enmu 枚举类型

枚举的值可以为字符串和数字

##### 数字类型枚举

如果我们枚举里面的内容不指定默认的值那么将会默认赋值 从0开始

```typescript
// 定义枚举
enum 枚举名{
    枚举字段1 = 值1,
    枚举字段2 = 值2
}
```



```ty
enum NumerEnum {
    Zero,
    One,
    Two,
}

console.log(NumerEnum.Zero)  // 0
console.log(NumerEnum.One)  // 1
console.log(NumerEnum.Two)  // 2
```



如果其中一个被初始化，接下来的都会+1

```typescript
enum Direction {
  Up,   // 0
  Down = 3,  // 3
  Left,      // 4
  Right,     // 5
}
```



在函数中使用元组类型最多

```typescript
function useState<T>(init: T): [T, (newValue: T) => void] {
  let stateValue = init;
  function setValue(newValue: T) {
    stateValue = newValue;
    console.log("State updated to:", stateValue);
  }
  return [stateValue, setValue];
}

const [counter, setCounter] = useState(10);

console.log("Initial counter:", counter); // 输出: Initial counter: 10

setCounter(20); // 更新状态并输出: State updated to: 20
```



##### 字符串枚举

字符串的枚举是没有自增长的功能的

```typescript
enum Direction {
  Up = 'up',
  Down = 'down',
  Left = 'left',
  Right = 'right',
}
```

##### 编译为js

```javascript
var Level;
(function (Level) {
    Level[Level["level1"] = 0] = "level1";
    Level[Level["level2"] = 1] = "level2";
    Level[Level["level3"] = 2] = "level3";
})(Level || (Level = {}));
```



##### 使用枚举

```typescript
let playerDirection: Direction = Direction.Up;
console.log(playerDirection); // 输出：1

//枚举成员可以直接通过枚举类型来访问，也可以通过枚举的值来访问。
```

##### 枚举可重复

```tsx
import { defineComponent, Fragment } from 'vue'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {

        enum gender {
            "male" = "男",
            "female" = "女",
            "male1" = "男"
        }

        console.log(gender.male);
        console.log(gender.male1);
        return () => (
            <div>

            </div>
        )
    }
})
```

##### 反向映射

对于重复值的枚举，反向映射只会返回第一个匹配的成员名。



```typescript
import { defineComponent, Fragment } from 'vue'

export default defineComponent({
    setup(props, { slots, expose, emit, attrs }) {

        enum gender {
            "male",
            "female",
            "male1"
        }

        console.log(gender[1]); // "male"
		
        return () => (
            <div>

            </div>
        )
    }
})
```







#### 函数类型

```typescript
const foo: ()=>void = () => {
    console.log("hello")
}

type fooType = () => void;
const foo: fooType = () =>{
    xxx
}
```

```typescript
type CalcType = (num1: number, num2: number) => number;

const a1: CalcType = (num1, num2) => {
    return num1 + num2;
};
```



```typescript
function dealyFn(fn:()=>void){
	setTimeout(()=>{
        fn()
    },1000);
}
```



```typescript
type calcfunc =(num1: number, num2: number) => void
// 这里的num1，num2无法省略
```



#### 联合类型 |

从现有类型中构建新类型

```ts
function getLength(args: string | any[]){
    return args.length;
}
```



#### 交叉类型 &

表示一个值必须同时具备所有类型的特性

可以用于interface和type，但是只能定义type

```typescript
interface Person {
  name: string;
}

interface Employee {
  employeeId: number;
}

type EmployeePerson = Person & Employee;

const employee: EmployeePerson = {
  name: "Alice",
  employeeId: 123
};
```



```typescript
type MyType = number & string // 不存在，是never类型
```



#### 实用类型 Utility Types

1. Record<K, T>

   - 定义一个对象类型,其中键的类型为K,值的类型为T。

   - 示例:Record<string, number>表示一个对象,其中键为字符串类型,值为数字类型

     ```typescript
     type StringToNumber = Record<string, number>;
     
     const ages: StringToNumber = {
         'Alice': 25,
         'Bob': 30,
         'Charlie': 35
     };
     
     
     type E = Record<'username' | 'password', string>
     
     const e: E = {
         username: 'admin',
         password: '123456'
     }
     ```
     
     ```typescript
     type A = {
         id: number,
         name: string,
         age: number
     }
     type A1 = Record<keyof A, string>
     const a: A1 = {
         id: '123',
         name: 'zhangsan',
         age: '18'
     }
     ```
     
     ```typescript
     type Record<K extends keyof any, T> = {
         [P in K]: T;
     };
     ```

2. Pick<T, K>

   - 从类型T中选择一组属性K,构造一个新的类型。

   - 示例:Pick<{ a: number, b: string, c: boolean }, 'a' | 'b'>表示从原类型中选择属性a和b,构造一个新的类型{ a: number, b: string }。

     ```typescript
     interface Person {
         name: string;
         age: number;
         email: string;
     }
     
     type NameAndAge = Pick<Person, 'name' | 'age'>;
     
     const person: NameAndAge = {
         name: 'Alice',
         age: 25
     };
     ```

3. Exclude<T, U>

   - 从类型T中排除可分配给类型U的属性,构造一个新的类型。

   - 示例:Exclude<'a' | 'b' | 'c', 'a'>表示从联合类型'a' | 'b' | 'c'中排除'a',得到新的类型'b' | 'c'。

     ```typescript
     type T = 'a' | 'b' | 'c'; // 联合类型 T
     type U = 'a';             // 要被排除的类型
     
     type Result = Exclude<T, U>; 
     // 'a' 被排除，得到新的类型 'b' | 'c'
     ```

4. Extract<T, U>

   - 从类型T中提取可分配给类型U的属性,构造一个新的类型。

   - 示例:Extract<'a' | 'b' | 'c', 'a' | 'b'>表示从联合类型'a' | 'b' | 'c'中提取可分配给'a' | 'b'的属性,得到新的类型'a' | 'b'

     ```typescript
     type T = 'a' | 'b' | 'c'; // 联合类型 T
     type U = 'a' | 'b';       // 需要提取的类型
     
     type Result = Extract<T, U>;
     // 从 T 中提取出 'a' 和 'b'
     ```

5. Partial<T>

   - 将类型T中的所有属性转换为可选属性,构造一个新的类型。

   - 示例:Partial<{ a: number, b: string }>表示将原类型中的属性a和b都转换为可选属性,得到新的类型{ a?: number, b?: string }。

     ```typescript
     type Partial<T> = {
         [P in keyof T]?: T[P];
     };
     ```

     

6. Required<T>

   - 将类型T中的所有属性转换为必选属性,构造一个新的类型。

   - 示例:Required<{ a?: number, b?: string }>表示将原类型中的可选属性a和b都转换为必选属性,得到新的类型{ a: number, b: string }。

     ```typescript
     type Required<T> = {
         [P in keyof T]-?: T[P];
     };
     ```

     

7. Readonly<T>

   - 将类型T中的所有属性转换为只读属性,构造一个新的类型。
   - 示例:Readonly<{ a: number, b: string }>表示将原类型中的属性a和b都转换为只读属性,得到新的类型{ readonly a: number, readonly b: string }。

8. Omit<T, K>

   - 从类型T中排除一组属性K,构造一个新的类型。
   
   - 示例:Omit<{ a: number, b: string, c: boolean }, 'a' | 'b'>表示从原类型中排除属性a和b,得到新的类型{ c: boolean }。
   
   - Exclude<T, U> 适用于联合类型，用于排除联合类型中的某些部分。
   
   - Omit<T, K> 适用于对象类型，用于移除对象类型中的某些键及其属性。
   
     ```typescript
     type T = {
       a: string;
       b: number;
       c: boolean;
     };
     
     type Result = Omit<T, 'a'>;
     // 从对象类型 T 中排除键 'a' 和对应的属性
     // Result = { b: number; c: boolean }
     ```
   
9. NonNullable<T>

10. ReturnType<T>

    - 获取返回值类型

      ```typescript
      type Foo = () => { name: string, age: number }
      type R = ReturnType<Foo>
      ```

      



#### 映射类型

映射类型只能用别名实现不能用接口实现

```typescript
type A = {
    username: string,
    password: number
}

type B<T> = {
    [P in keyof T]: T[P]
}

type C = B<A>
```

修改别名为只读

```typescript
type A = {
    username: string,
    password: number
}

type B<T> = {
    readonly [P in keyof T]: T[P]
}

type C = B<A>
```

##### ts 内置工具（实用类型）

```typescript
type A = {
    username: string
    age: number
}

type B = Readonly<A>  // 只读
type C = Partial<A>  // 可选
```



#### infer 类型推断

在条件类型中推断类型变量的具体类型

```typescript
type A<T> = T extends Array<infer U> ? U : T;
```

**`infer` 的核心作用**

- **推断出复杂类型中的某些部分类型**。
- 必须搭配条件类型一起使用。
- 不可以在普通类型定义中独立使用（只能用于 `extends` 条件类型中）。



### 条件类型

```typescript
type A = string
type B = number
type C = A extends B ? {} : []
```

```typescript
type A = Exclude<string | number | boolean, string>
// type A = number | boolean
```



### PropType

PropType 仅用于定义组件的 props 类型

```typescript
props: {
    myProp: {
        type: Object as PropType<MyType>,
        required: true,
    }
}
    
props: {
    items: {
        type: Array as PropType<MyType[]>,
        required: true,
    }
}
```







### 类型别名

```typescript
type MyNumber = number
const age: MyNumber

type IDtype = number | string
function printID(id: IDType){
    console.log(id)
}
```



```typescript
type PointType = {x:number, y:number, z?:number }
function printCoordinate(point: PointType){
     xxxx
}

// 加逗号，分号，不加都可以
// 写在同一行需要加逗号或者分号
```



### 只读 readonly & 可选

```typescript
interface A {
    readonly username: string;
    age?: number;
}

let a: A = {
    username: 'xiaoming'
}

a.username = 'xiaobai'  // 错误
```





### 接口的声明

```typescript
interface PointType {
    x:number
    y:number
    z?:number
}
```



### interface & type

#### 区别

1. type 可以描述任何类型组合，interface 只能描述对象结构
2. interface 可以继承自（extends）interface 或对象结构的 type。type 也可以通过 `&` 做对象结构的继承；

```typescript
// Interface 继承 Interface
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}

const myDog: Dog = {
  name: "Buddy",
  breed: "Golden Retriever"
};

// Interface 继承 Type
type AnimalType = {
  name: string;
}

interface Cat extends AnimalType {
  color: string;
}

const myCat: Cat = {
  name: "Whiskers",
  color: "Gray"
};

// Type 使用交叉类型
type Animal = {
  name: string;
}

type Bird = Animal & {
  canFly: boolean;
}

const myBird: Bird = {
  name: "Tweety",
  canFly: true
};
// interface可以继承 interface和type
// type可以通过 & 合并 type和interface
```

3. <span style="color:red">多次声明的同名 interface 会进行声明合并，type 则不允许多次声明；</span>

```typescript
interface User {
  name: string;
}

interface User {
  age: number;
}

const user: User = {
  name: "Alice",
  age: 30
};
```

4. interface可以被类实现

```typescript
class Person implements IPerson{
    xxx
}
```

5. 继承多个

```typescript
type Employee = Person & Contact & {
  id: number;
  role: string;
};

interface C extends A, B {
  role: string;
}
```

6. 子接口不能覆盖父接口的成员

   交叉类型会把相同成员的类型进行交叉

7. type具备映射类型

```typescript
type A = {
    [p in 'username' | 'password' | 'email']: string
}

interface A {
    [p in 'username' | 'password' | 'email']: string
} // 报错  接口中的计算属性名称必须引用必须引用类型为文本类型或 "unique symbol" 的表达式。ts(1169)
  // 算术运算右侧必须是 "any"、"number"、"bigint" 或枚举类型。ts(2363)
```

等同于

```typescript
type A = {
    username: string;
    password: string;
    email: string;
}

interface A {
    username: string;
    password: string;
    email: string;
}
```



#### 冲突

```typescript
// 如果继承的多个接口中有相同的属性名，但属性类型一致，则会正常合并
// 如果继承的多个接口中有相同的属性名，但类型不一致，则会报错

interface A {
  x: number;
}

interface B {
  x: string;
}

interface C extends A, B {}  // 报错

// 如果继承的类型中存在冲突，可以通过类型合并或重写属性来解决
interface A {
  x: number;
}

interface B {
  x: string;
}

interface C extends A, B {
  x: number | string; // 手动定义为联合类型
}

const obj: C = {
  x: 42, // 或者 x: "Hello"
};
```



### extends

在 TypeScript 中，`extends` 用于表示“**类型约束**”或“**继承**”。它的具体含义依赖于上下文：

- **在接口或类中**：`extends` 表示类型继承，类似于面向对象语言中的继承机制。
- **在泛型中**：`extends` 用于对泛型参数进行约束，表示泛型类型必须符合某种条件。
- **在条件类型中**：`extends` 用于判断一个类型是否是另一个类型的子类型，并根据条件返回不同的结果。



1. 用于接口或类的继承

   ```typescript
   interface Animal {
     name: string;
   }
   
   interface Dog extends Animal {
     breed: string;
   }
   
   const myDog: Dog = {
     name: "Buddy",
     breed: "Golden Retriever",
   };
   ```

   ```typescript
   class Animal {
     name: string;
   
     constructor(name: string) {
       this.name = name;
     }
   
     move() {
       console.log(`${this.name} is moving`);
     }
   }
   
   class Dog extends Animal {
     breed: string;
   
     constructor(name: string, breed: string) {
       super(name); // 调用父类构造函数
       this.breed = breed;
     }
   
     bark() {
       console.log(`${this.name} is barking`);
     }
   }
   
   const myDog = new Dog("Buddy", "Golden Retriever");
   myDog.move(); // Buddy is moving
   myDog.bark(); // Buddy is barking
   ```

   



### 类型断言 as

有时候ts无法获取具体的类型信息，需要使用类型断言（Type Assertions）

  ```typescript
  const imgEl = document.querySelector("img") // const imgEl:HTMLImageElement | null
  const imgEl1 = document.querySelector(".img") // const imgEl:Element | null
  
  const imgEl1 = document.querySelector(".img") as HTMLImageElement
  ```

断言只能断言为更具体的类型，或者 不太具体的类型（any / unknown）

bug：

```typescript
const name = ("coderwhy" as unknown) as number
```



`as const` 用于将对象的所有属性设置为只读，并推断其为字面量类型



### 非空类型断言 !

比较危险，确定一定有值得时候才能使用

```typescript
interface IPerson{
    name:string,
    friend?:{
        name:string
    }
}

console.log(info.friend?.name) // 访问

// 方案1 类型缩小
if(info.friend){
    info.frined.name = "kobe"
}

// 方案2 非空类型断言
info.friend!.name = "james" 
// 如果你错误地断言了一个实际上是 null 或 undefined 的值，JavaScript 在运行时会抛出错误。
```

赋值表达式左侧不能是可选属性访问



### 字面量类型

```typescript
type Direction = "left" | "right"
const d1:Directon = "left"
alert(typeof d1);  // string
```

字面量类型（如 `"left" | "right"`）在编译时用于类型检查，而在运行时，`typeof` 只考虑实际的数据类型。



```typescript
type MethodType = "get" | "post"
function request(url:string,method:MethodType){
    xxx
}

const info = {
    url:"xxx",
    method:"post"
}

request(info.url,info.method) // info.method报错
/*
	const info:{
		url: string;
		method: string;
	}
*/

// 解决方案1
request(info.url,info.method as "post") // 进行类型断言

// 解决方案2
const info:{url: string, method: "post"} = {
    xxx
}

// 解决方案3
const info = {
    url:"xxx",
    method:"post"
} as const
```

`as const` 用于将对象的所有属性设置为只读，并推断其为字面量类型



### 类型索引访问

```typescript
type ComponentConfig = {  
 width: number;  
 height: number;  
 color: string;  
};  
 
type WidthType = ComponentConfig['width']; // number  
type ColorType = ComponentConfig['color']; // string
```



### 类型兼容

```typescript
let a: number = 123;
let b: string | number = "hello"
a = b  // 错误
b = a 

let a: { name: string } = { name: 'Tom' }
let b: { name: string, age: number } = { name: 'Jerry', age: 20 }
a = b  // 成功
b = a  // 错误
```

`number` 类型不能兼容 `string | number`，因为联合类型中可能存在不兼容的分支（例如 `string`）。

对象类型的兼容性是基于属性的子集关系进行检查的。`{ name: string }` 是 `{ name: string, age: number }` 的子集，因此赋值成功。

```typescript
function foo(n: { username: string }) {}

foo({ username: 'test', password: 'test' })  // 错误
foo({ username: 'test', password: 'test' } as { username: string });

let a = { username: 'test', password: 'test' }
foo(a)  // 成功
```

TypeScript 对对象字面量会进行**额外的属性检查**，而对变量则不会。

当你直接传递对象字面量时，TypeScript 认为你可能不小心添加了多余的属性，因此会进行严格的检查。而当你通过变量传递时，TypeScript 认为你已经明确知道变量的内容，因此不会进行额外的属性检查。





### 类型缩小(类型保护)

```typescript
if(typeof padding === "number"){  // 类型保护
    xxx
}
```

#### in运算符

检查对象是否有某个属性

```typescript
const obj = { a: 1, b: 2 };
console.log("a" in obj); // 输出: true
console.log("c" in obj); // 输出: false
```

迭代对象的属性 for in

```javascript
const obj = { a: 1, b: 2 };
for (const prop in obj) {
  console.log(prop); // 输出: "a", "b"
}
```

缩小类型范围

```typescript
interface ISwim {
	swim: () => void
}
interface IRun {
    run: () => void
}

function move(animal: ISwim | IRun){
    if("swim" in animal){
        animal.swim()
    }else if ("run" in animal){
        animal.run()
    }
}
```



#### instanceof

判断是否为某个类的实例,使用new关键字构造函数

```typescript
function printDate(date: string | Date){
    if(date instanceof Date){
        console.log(date)
    }
}
```

```javascript
function Person() {
  this.name = "wang";
}
let p1 = new Person();
console.log(p1 instanceof Person); // true
```



```typescript
class Person {
    name: string;
    age: number;
    address: string;
    constructor(name: string, age: number, address: string) {
        this.name = name;
        this.age = age;
        this.address = address;
    }
}

const p1: Person = new Person('Tom', 25, 'China');
        
console.log(p1 instanceof Person); // 输出：true
```



#### keyof

keyof 是 TypeScript 中的一个关键字，用于获取某个类型的所有公共属性名（public property names）组成的<span style="color:red">联合类型</span>（union type）。它可以与索引类型（indexed types）一起使用，以确保动态访问属性时的类型安全。



```typescript
interface IKun {
    name: string,
    age: number
}

type IKunKeys = keyof IKun // "name" | "age"
```



```typescript
interface Person {
    name: string,
    age: number,
}

const p1: Person = {
    name: 'Tom',
    age: 25
}

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const name = getProperty(p1, 'name'); 
const name = getProperty<Person, 'name'>(p1, 'name');
const age = getProperty(p1, 'age'); 
```



```typescript
type Person = {
    name: string,
    age: number,
    address: string
}

const p1: Person = {
    name: 'John',
    age: 25,
    address: 'New York'
}

const p2: Pick<typeof p1, keyof typeof p1> = {
    name: 'John',
    age: 25,
    address: 'New York'
}
```

1. `typeof p1` 获取 `p1` 对象的类型，在这个例子中，`typeof p1` 的结果是 `Person`。

2. `keyof typeof p1` 获取 `Person` 类型的所有属性名，结果是一个联合类型 `"name" | "age" | "address"`。

3. `Pick<typeof p1, keyof typeof p1>` 使用 `Pick` 工具类型，从 `Person` 类型中选择 `"name" | "age" | "address"` 这些属性，生成一个新的类型。



#### typeof

```typescript
let a = "hello"
type A = typeof a  // string

const b = {
    username: 'admin',
    password: '123456',
    email: 'admin@123.com'
}

let b1: keyof typeof b = 'username' 

type b2 = keyof B; // "username" | "password" | "email"

let b3: typeof b = {
    username: 'admin1',
    password: '1234561',
    email: 'admin11@123.com'
}
```



## 接口声明举例

例子1

```typescript
interface A {
    [index: number]: number;
}
```

这个接口的意思是：任何实现了 `A` 接口的对象 **必须支持用数字类型的索引来访问其值**，并且返回的值也必须是数字类型。

```typescript
const obj: A = {
    0: 42,
    1: 99,
    2: 21
};

console.log(obj[0]); // 输出 42
console.log(obj[1]); // 输出 99

const arr: A = [10, 20, 30];

console.log(arr[0]); // 输出 10
console.log(arr[1]); // 输出 20
```











## 函数类型参数个数

```typescript
type CalaType = (num1:number, num2:number) => number
function calc(calcFn:CalaType){
    xxx
}

calc(function(){   // 这里参数个数不进行校验，ts不报错
    return 123
})
```



ts对于很多类型是否报错，取决于它的内部规则

```typescript
inetrface IPerson {
	name:string,
    age:number
}

const info:IPerson {
	name:"why",
    age:18,
    height:1.88   // 报错
}

const p = {
	name:"why",
    age:18,
    height:1.88
}

const info:IPerson = p  // 不报错
```



在js中调用时有两个参数，但是函数本身只有一个参数

额外的实参会被忽略

为回调函数写函数类型不要写成可选

typescript对于传入的函数类型的多余的参数会被忽略掉

```typescript
type CalaType = (num1:number, num2:number) => number
function calc (calcFn:CalcType){
    calcFn(10,20)
}

calc(function(num1)){
	return 123     
})
```



## Call Signatures 调用签名

在js中，函数除了可以被调用，自己也可以有属性值

```typescript
type BarType = (num1:number) => number  // 这种写法不支持声明属性
```

函数的调用签名(从对象的角度看待函数)

```typescript
interface IBar {
    name:string,
    age:number,
    (num1:number): number
}

const bar: IBar = (num1:number):number => {
    return 123
}

bar.name = "aaa"
bar.age = 18
bar(123)
// 如果你去掉 (num1: number): number，那么 IBar 就只定义了 name 和 age 属性，不能再将 bar 作为函数调用。代码会报错，因为 bar 不再符合接口的要求。
```



## Construct Signatures 构造签名

js函数也可以用new操作符调用，当贝调用的时候，ts会认为这是一个构造函数，因为他们会产生一个新对象。

你可以写一个构造签名（Construct Signatures），方法是在调用签名签名加一个new关键词

```typescript
class Person {
    constructor(public name: string, public age: number) { }
}
interface PersonConstructor {
    new(name: string, age: number): Person;
}

function createPerson(ctor: PersonConstructor): Person {
    return new ctor("Alice", 25);
}

// 调用 createPerson 函数,传入 Person 类作为构造函数
const person = createPerson(Person);
```



## 可选参数

```typescript
function foo(x:number,y?:number){  // y 的类型为 number | undefined
    if(y!==undefined){
        console.log(y+10)
    }
}
```

1. 默认有值的情况下，参数的类型注解可以省略
2. 有默认值的参数可以接收一个undefined的值

```typescript
function foo(x:number,y = 100){
    console.log(y+10)
}

foo(10,undefined) // 不报错
```



## js剩余参数

剩余参数将不定数量的参数放到一个数组里

```javascript
function sum(...nums:number[]){
    let total = 0;
    for(const num of nums){
        total += num;
    }
    return total;
}

const result1 = sum(10,20,30)
```



## 函数的重载

在typescript中我们可以边写不同的重载签名

一般编写两个或以上的重载签名，再去编写一个通用的函数以及实现

开发中尽量使用联合类型实现

```typescript
function add(arg1:number, arg2:number): number
function add(arg1:string, arg2:string): string

function add(arg1:any, arg2:any): any{
    return arg1 + arg2
}


add(10,20)
add("abc","cba")
add(111,"aaa")  // 报错，没有匹配的重载
```



获取长度，对象方法实现

```typescript
function getLength(arg:{length:number}){
    return arg.length
}
```



## 类

### 类的定义

在早期JavaScript开发中（ES5），通过函数和原型链实现类和继承，从ES6开始，引入了class关键字

typescript可以对类的属性和方法进行静态类型检测



typescript成员属性必须进行声明

<span style="color:red">构造函数不需要返回值，默认返回当前示例</span>

```typescript
class Person{
    // ts要求使用属性列表描述类中的属性
    name: string
    age: number
    
    constructor(name:string, age:number){
        this.name = name
        this.age = age
    }
    
    say(){
        console.log(this.name, this.age)
    }
}

const p1 = new Person("why",10)

console.log(p1.name, p2.age)
```



### 类的继承

使用`extends`关键字, 子类使用`super`访问父类

```typescript
class Student extends Person{
    sno: number
    constructor(name:string, age:number, sno:number){
        super(name,age)
        this.sno = sno
    }
    
    studying(){
        console.log(this.name)
    }
    
    saying(){
        super.say()
        consoloe.log("hello")
    }
}
```



### 类的成员修饰符

在TypeScript中，类的属性和方法支持三种修饰符：public、private、protected 

- public 修饰的是在任何地方可见、公有的属性或方法，默认编写的属性就是public的； 

- private 修饰的是仅在同一类中可见、私有的属性或方法； 可以在类内部构造器，方法里使用。可以用getter/setter设置

  一般私有的变量前面加 下划线`_`

- protected 修饰的是仅在类自身及子类中可见、受保护的属性或方法



### 只读属性 readonly

1. 类属性

```typescript
class Person{
    readonly name: string
    constructor(name : string){
        this.name = name
    }
}

const p = new Person("li")
p.name = "zhang"  // 只读属性无法写入
```

2. 接口

```typescript
interface User{
    readonly id: number,
    name:string
}
```

只读修饰符不在编译结果中

```typescript
let arr: readonly number[] = [3, 4, 6]; // 无法使用push,pop,slice

```



### getter/setter

私有属性：属性前面加 _ 

可以对属性取值赋值进行拦截

```typescript
class Person{
    private _name: string
    constructor(name: string){
        this._name = name
    }
    
    set name(newValue: string){
        this._name = newValue
    }
    get name(){
        return this._name
    }
}

```

```typescript
class Person {
    private _age: number;

    constructor(age: number) {
        this._age = age;
    }

    // Getter: 用于获取属性
    get age(): number {
        console.log("Getting age...");
        return this._age;
    }

    // Setter: 用于设置属性
    set age(value: number) {
        if (value < 0) {
            console.error("Age cannot be negative!");
        } else {
            console.log("Setting age...");
            this._age = value;
        }
    }
}

// 使用示例
const person = new Person(25);

console.log(person.age); // 调用 getter，输出: "Getting age..." 然后输出: 25

person.age = 30;  // 调用 setter，输出: "Setting age..."
console.log(person.age); // 调用 getter，输出: "Getting age..." 然后输出: 30

person.age = -5;  // 调用 setter，输出: "Age cannot be negative!"
console.log(person.age); // 调用 getter，输出: "Getting age..." 然后输出: 30
```



### 参数属性

TypeScript 提供了特殊的语法，可以把一个构造函数参数转成一个同名同值的类属性

以通过在构造函数参数前添加一个可见性修饰符public private protected 或者 readonly 来创建参数属性，最后这些类 属性字段也会得到这些修饰符

```typescript
class Person{
    constructor(public name: string, private _age: number){}
    
    set age(newAge){
        this._age = newAge
    }
    get age(newAge){
        return this._age
    }
}
```



### 抽象类 abstract

继承是多态的前提。封装，继承，多态。

es5通过原型链实现继承，es6可以通过extends继承

调用接口时, 我们通常会让调用者传入父类，通过多态来实现更加灵活的调用方式

但是，父类本身可能并不需要对某些方法进行具体的实现，所以父类中定义的方法,我们可以定义为抽象方法

- 抽象方法，必须存在于抽象类中；

- 抽象类是使用abstract声明的类；



### 具有类型特性

类的作用

1. 可以创建类对应的实例对象
2. 类本身可以作为这个实例的类型
3. 类也可以当做一个有构造签名的函数

```typescript
class Person{
    constructor(public name: string,public age: number){}
}

function factory(ctor: new() => void){}
factory(Person)
```



## 鸭子类型

typescript对于类型检测的时候是用鸭子类型

*“一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子,那么这只鸟可以被称为鸭子”*

鸭子类型只关心属性和行为，不关心具体是不是对应的类型

```typescript
class Person{
    constructor(public name: string,public age: number){}
}

class Dog{
    constructor(public name: string,public age: number){}
}

function printPerson(p:Person){
    console.log(p.name,p.age)
}

printPerson(new Person("li",17))
printPerson({name:"zhang",age:18})  // 不报错
printPerson(new Dog("旺财",9))  // 不报错
cosnt person: Person = new Dog("a",3) // 不报错

printPerson({ name1: "zhang", age: 18 })  // 报错
```



```typescript
interface A {
    name: string,
    age: number
}
interface B {
    name: string,
    age: number
}
let a: A = {
    name: 'zhangsan',
    age: 18
}
let b: B = a; // ✅ 合法，因为 a 的结构符合 B 的定义
```



## 索引签名

索引签名必须是string或者number，不能是联合类型

```typescript
interface Iconllection{
    [index: number]: string  // 索引签名
    length: number
}

const names: string[] = ["abc", "bb"]
console.log(names[0])
console.log(names[1])

const tuple: [string,string] = ["why", "18"]
console.log(tuple[0])
console.log(tuple[1])
```



```typescript
interface Iconllection {
    [index: string]: string  // 索引签名
    length: number  // 报错，因为length也是string类型，被索引签名包括，但是返回的是number类型
}  
```

在定义 `Iconllection` 接口时，使用了索引签名 `[index: string]: string`，这意味着所有通过字符串索引访问的属性都必须是字符串类型。然而，你在接口中定义了一个名为 `length` 的属性，其类型为 `number`，这与索引签名的类型不匹配，因此导致了类型错误



## 接口继承 extends

接口可以继承type(类型别名)，type不能继承接口

```typescript
interface IPerson{
    name: string,
    age: numebr,
}

interface IKun extends IPerson{
    slogan: string
    playBasketball: () => void
}

// 类实现接口，必须实现所有属性
class Person implements IKun{
    name: string,
    age: number,
    slogan: string,
    playBasketball(){  
    }
}
```



类型约束

```typescript
type Animal = 'cat' | 'dog' | 'bird';

interface Pet extends Animal {
    name: string;
}
```



```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}
```





## ts严格字面量赋值检测

在第一次创建对象字面量，称之为fresh（新鲜的）

对于新鲜的字面量，会进行严格的类型检测，必须完全满足类型要求，不能有多余的属性。



现象1

```typescript
interface IPerson {
    name: string,
    age:number
}

const obj = {
	name: "why",
    age: 18,
    height: 1.88
}

const info: IPerson = obj  // 不报错

const obj = {
	name: "why",
}

const info: IPerson = obj // 报错，缺少age
```

现象2

```typescript
interface IPerson {
    name: string,
    age:number
}

function printPerson(person: IPerson){}

printPerson({name: "li", age: 18, height: 1.88}) // 报错，“height”不在类型“IPerson”中

const obj = {name: "li", age: 18, height: 1.88}
printPerson(obj)  // 不报错
```



## 英文

argument: 参数

parameter: 形参



## declare type

`declare` 关键字用于声明已经存在于外部环境中的变量、函数、类、模块等。它告诉 TypeScript 编译器这些实体的类型信息，但不生成实际的代码。

`type` 则是用于创建类型别名的关键字，它允许你为一个类型起一个新的名字。



## const modelData1 = () => ({ component: VNode2 })

`const modelData1 = () => ({ component: VNode2 })` 这种写法是一个箭头函数，它返回一个对象。

- 箭头函数 `() => ({ ... })` 用于返回一个对象字面量。
- 外层的括号 `()` 是必需的，因为它告诉 JavaScript 解释器这是一个对象字面量，而不是函数体的开始。
- 内层的花括号 `{}` 定义了对象的属性和值。

```tsx
//等同于
const modelData1 = () => {
  return {
    component: VNode2
  };
};
```

## ReturnType

用于获取函数类型的返回值类型

```tsx
interface User {
  id: number;
  username: string;
  email: string;
}

function getUsername(user: User): string {
  return user.username;
}

type GetUsernameReturnType = ReturnType<typeof getUsername>;
                                        
const username: GetUsernameReturnType = getUsername({ id: 1, username: 'john', email: 'john@example.com' });
console.log(username); // 输出：'john'                                        
```



## 回调函数

回调函数的确可以理解为将一个函数传递给另一个函数,让被调用的函数在适当的时候执行传入的函数,并将结果返回给外部



### 示例

```tsx
export function use1(fun: (getTable: () => string) => number) {
  const tableData = 'Hello, World!';

  const getT2 = () => {
    return tableData;
  };

  const result = fun(getT2);
  console.log('Result:', result);
}

// 使用示例
use1((getT1) => {
  const table = getT1();
  return table.length;
});

getTable只是一个占位符，表示fun期望接收一个返回字符串的函数。
实际上将getT2函数传递给了fun函数，getT2扮演了getTable的角色

两层回调
fun由`使用use1的`提供，给use1使用，返回table.length
getTable由use1提供，给`使用use1的`使用 返回一个string


/**
use1(function(getT1) {
  const table = getT1();
  return table.length;
});
*/

等于传入一个匿名函数，第一个参数是getT1，返回的是 getT1的返回值的length



function simpleUse1(callback: (data: string) => number) {
  const tableData = 'Hello, World!';

  const result = callback(tableData);
  console.log('Result:', result);
}

// 使用示例
simpleUse1((data) => {
  return data.length;
});
```



## 非空断言操作符 `!` 

它用于告诉 TypeScript 编译器，某个值不会是 `null` 或 `undefined`，即使 TypeScript 无法确定这一点。

```typescript
function getValue(value?: string) {
  if (typeof value === "undefined") {
    return;
  }
  
  console.log(value.length); // 错误：Object is possibly 'undefined'.
  console.log(value!.length); // 正确：使用非空断言操作符
}
```



## 泛型

```typescript
let nums: number[] = [2, 3, 4]
let nums: Array<number> = [2, 3, 4]
```



### 类型的参数化

```typescript
function bar<T>(arg: T){
    return arg
}

const bar = <T>(arg: T) => {
  return arg;
};
```

```typescript
function foo<T,E>(a1: T, a2: E){}
```

T：Type的缩写, 类型

K、V：key和value的缩写，键值对

E：Element的缩写，元素

O：Object的缩写，对象

```typescript
type MyArr<T> = T[];
let d: MyArr<number> = [1, 2, 3]
```



很多时候TS会根据传递的参数，推导出泛型的具体类型。

如果无法完成推导并且没有传递具体的类型，默认为空对象。

### 泛型的接口使用

```typescript
interface IKun<T>{
    name: T,
    age: number,
    slogan: T
}

const kun: IKun<string> = {
    name: "li",
    age: 18,
    slogan: "haha"
}
```

可以给默认值

```typescript
interface IKun<T = string>{
    name: T,
    age: number,
    slogan: T
}

const kun: IKun = {
    name: "li",
    age: 18,
    slogan: "haha"
}
```



### 泛型类的使用

```typescript
class Point<T = number> {
    x: T
    y: T
    constructor(x: T, y: T){
        this.x = x
        this.y = y
    }
}

const p1 = new Point(10,20)
const p1 = new Point<string>("10","20")
```



### 泛型约束

使用extends

```typescript
// 获取传入的内容，必须有length属性
// T相当于是一个变量，用于记录本次调用的类型 
interface ILength {
    length: number
}


function getInfo<T extends ILength>(args: T): T {
     return args
}

const info1 = getInfo("aaaa")  // const info1: "aaaa"
```



传入的key类型，obj当中key的其中之一

keyof

```typescript
function getObj<O, K extends keyof O> (obj: O, key: K) { // k必须是O对象的联合类型中的一个
    return obj[key]
}

const info = {
    name:"li",
    age: 18,
    height: 1.88
}

const name = getObj(info,"name")
```



### 映射类型

映射类型不能使用interface，只能使用type

- 有的时候，一个类型需要基于另外一个类型，但是你又不想拷贝一份，这个时候可以考虑使用映射类型

- 映射类型建立在索引签名的语法上： 
  - 映射类型，就是使用了PropertyKeys 联合类型的泛型；
  - 其中PropertyKeys 多是通过 keyof 创建，然后循环遍历键名创建一个类型；

拷贝一份IPerson

```typescript
interface IPerson {
	name: string,
    age: number
}

type MapPerson<T> = {
    [Property in keyof T]: T[Property]
}

type NewPerson = MapPerson<IPerson>
```

  

#### 映射修饰符

在使用映射类型时，有两个额外的修饰符可能会用到

- 一个是readonly，用于设置属性只读； 
- 一个是? ，用于设置属性可选；

你可以通过前缀-或者+ 删除或者添加这些修饰符，如果没有写前缀，相当于使用了+ 前缀



#### 可选类型转换

```typescript
type User = {
  name: string;
  age?: number;
  email: string;
};

type MapUser<T> = {
    [Property in keyof T]?: T[Property]  // 全部可选
}

type MapUser1<T> = {
    [Property in keyof T]-?: T[Property]  // 全部必选
}
```





#### 对象属性的只读转换

```typescript
type User = {
  name: string;
  age: number;
  email: string;
};

type ReadonlyUser = {
  readonly [P in keyof User]: User[P];
};
```



#### 部分属性的选择

```typescript
type Product = {
  id: number;
  name: string;
  price: number;
  description: string;
};

type ProductSummary = {
  [P in 'id' | 'name']: Product[P];
};
```

在上面的例子中，`ProductSummary` 类型使用映射类型语法 `[P in 'id' | 'name']` 从 `Product` 类型中选择 `id` 和 `name` 属性。`Product[P]` 用于获取选择的属性的类型。



## 例子

### 范例1

```typescript
const form = {
    name: '',
    age: 0,
    email: ''
};

interface IFormData<T> {
    data: T;
}

interface IUserFormData extends IFormData<typeof form> {
    // 这里可以添加其他属性或方法
}
                                          
// 根据 form 对象的类型，IUserFormData 接口将具有以下属性
interface IUserFormData {
    data: {
        name: string;
        age: number;
        email: string;
    };
}

//  可以创建一个符合 IUserFormData 接口的对象
const userFormData: IUserFormData = {
    data: {
        name: 'John',
        age: 25,
        email: 'john@example.com'
    }
};
```



## ts配置文件 tsconfig.json

命令行生成文件

```
tsc --init 
```



```json
{
  "compilerOptions": {  // 编译选项
    "target": "esnext", 
    // 指定 ECMAScript 的目标版本，这里是 "esnext"，表示支持最新的 ECMAScript 特性。
    // 浏览器可能不支持所有最新特性，因此可  能需要额外的转译。
    
    "useDefineForClassFields": true, 
    // 启用 `useDefineForClassFields`。在类字段声明上使用 `Object.defineProperty` 替代直接赋值。
    // 这会更接近 JavaScript 的标准行为，但可能与旧代码的行为不一致。

    "module": "esnext", 
    // 指定模块系统，这里是 "esnext"，表示生成使用 ES 模块系统（`import`/`export`）的代码。
    // 可选值包括 "commonjs"（Node.js 中常用）或其他模块规范。

    "moduleResolution": "node", 
    // 指定模块解析策略，这里使用 "node"，表示使用 Node.js 的模块解析逻辑。
    // 例如，自动解析 `node_modules` 中的模块。

    "strict": true, 
    // 启用 TypeScript 的严格模式。严格模式会打开所有类型检查的严格选项。
    // 例如：`noImplicitAny`, `strictNullChecks`, `strictFunctionTypes` 等。

    "jsx": "preserve", 
    // 指定 JSX 语法的处理方式。这里选择 "preserve"，表示保留 JSX 语法，不转译为 JavaScript。
    // 通常用于 React 或其他框架的编译器会进一步处理 JSX。

    "jsxFactory": "h", 
    // 指定用于 JSX 的工厂函数。这里设置为 `h`，通常用于像 Preact 等框架。
    // 如果你使用 React，默认值为 `React.createElement`，可以省略。

    "jsxFragmentFactory": "Fragment", 
    // 指定用于 JSX 片段的工厂函数。这里设置为 `Fragment`，通常用于支持空标签 `<></>` 的 JSX 语法。
    // 如果你使用 React，默认值为 `React.Fragment`，可以省略。

    "sourceMap": true, 
    // 生成 `.map` 文件，用于调试时将编译后的代码映射回原始 TypeScript 代码。
    // 对于调试和开发工具来说非常有用。

    "resolveJsonModule": true, 
    // 允许从 TypeScript 文件中直接导入 JSON 文件。
    // 开启后可以使用 `import data from './data.json'`。

    "isolatedModules": true, 
    // 强制每个文件作为独立的模块进行编译。通常配合 Babel 或其他工具使用。
    // 必须启用此选项来支持后续的单文件编译器行为。

    "esModuleInterop": true, 
    // 启用 ES 模块与 CommonJS 模块的互操作性。
    // 例如，可以用 `import fs from 'fs'` 替代 `import * as fs from 'fs'`。

    "lib": ["esnext", "dom"], 
    // 指定编译时包含的库文件。`esnext` 表示支持最新的 ES 特性，`dom` 表示支持 DOM API。

    "skipLibCheck": true, 
    // 跳过对声明文件（`.d.ts`）的类型检查。这可以加快编译速度，减少无关报错。

    "baseUrl": ".", 
    // 设置项目的基础路径，用于解析非相对模块的路径。
    // 这里设置为当前目录（`.`）。

    "paths": {
      "~/*": ["./*"],
      "@/*": ["./src/*"]
    }, 
    // 配置模块路径别名。`~/*` 表示当前项目根目录，`@/*` 指向 `src` 目录。
    // 例如，`import '~/utils'` 等价于 `import './utils'`。

    "types": ["node"], 
    // 指定包含的类型定义文件。这里包含 `node`，表示加载 Node.js 类型定义。
    // 例如，可以使用 Node.js 的全局变量和模块（如 `process`、`fs`）。
     
     "outDir": "./dist",  // 编译结果位置
      
     "removeComments": true  // 移除注释
  },
  "exclude": ["node_modules", "dist"],  // 指定要排除的文件
  // 指定要排除的文件或文件夹。这里排除了 `node_modules` 和 `dist` 文件夹。
  // 通常是避免编译外部库和输出目录。
  "include": ["src/**/*"] // 指定要包含在编译中的文件,仅包含 src 文件夹中的所有文件
}
```



## 声明文件`.d.ts`

以`.d.ts`为后缀的文件我们称之为`TypeScript声明文件`，描述JavaScript模块内所有导出接口的类型信息

`.d.ts`是TypeScript中用于声明类型定义的文件扩展名。它允许开发者为JavaScript库或没有直接包含类型信息的代码提供类型注释，使得TypeScript编译器能够理解这些代码的结构和类型，从而在开发阶段提供静态类型检查、智能提示等功能，而不会对原始代码进行任何修改或编译操作。

```typescript
// t1.ts
import { foo } from './t2';

foo(10);

// t2.js
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
exports.foo = foo;
function foo(n) {
    return `hello ${n}`;
}

// t2.d.ts
export declare function foo(n: number): string;
```

### `lib.d.ts`

在安装`TypeScript`时，会顺带安装一个lib.d.ts声明文件。这个文件包含`JavaScript`运行时以及`Dom`中存在各种常见的环境声明



### `global.d.ts`

定义全局作用域中的内容，也就是说，文件里的声明可以直接在项目的任何地方使用，而无需显式导入。



## `env.d.ts`

### 什么是`env.d.ts`文件

`env.d.ts` 是一个 TypeScript 类型声明文件，专门为 Vite 构建工具和其相关特性提供类型定义。Vite 是一个现代化的构建工具，旨在为开发者提供快速的开发体验，它支持热更新、模块化和按需构建等特性。由于 Vite 的动态特性和构建过程，它会自动生成一些特殊的环境变量和全局类型，这些内容需要通过 vite-env.d.ts 文件进行类型声明，确保 TypeScript 编译器能识别和正确处理这些内容。

```typescript
/// <reference types="vite/client" />

declare module '*.vue' {  // 所有以 .vue 结尾的文件，都是一个模块
  import type { DefineComponent } from 'vue'
  // eslint-disable-next-line @typescript-eslint/no-explicit-any, @typescript-eslint/ban-types
  const component: DefineComponent<{}, {}, any>
  export default component
}
```

### `env.d.ts`文件作用

在 TypeScript 项目中，vite-env.d.ts 文件的主要作用是为 Vite 特有的 API、环境变量和模块提供类型声明。没有这个文件，TypeScript 可能无法识别 Vite 自动注入的一些类型，如：

- import.meta.env
- Vite 特有的模块和路径类型

这些类型在开发中是非常常见的，尤其是在与 Vite 配合使用时，通过提供正确的类型声明，可以帮助开发者避免类型错误、提高代码的可读性和可维护性。



.d.ts 文件中的顶级声明必须以 "declare" 或 "export" 修饰符开头。

通过declare声明的类型或者变量或者模块，在include包含的文件范围内，都可以直接引用而不用去import或者import type相应的变量或者类型。



## 类型声明declare关键字

在TypeScript中，declare关键字主要用于描述已经存在的变量、函数、类、模块等，而不是定义一个新的实体。这通常用于告诉TypeScript编译器某个变量、函数、类等是在其他地方定义的，例如在一个JavaScript库中，这样TypeScript编译器就不会因为这些未定义的实体而报错。

### 



## 三斜杠语法

来源于C#写法，表示引用的意思，引用其他文件。

```typescript
/// <reference types="vite/client" />  
```

它加载 Vite 提供的类型定义文件。

常见的三斜线指令有以下两种：

1. `/// <reference path="..." />`：引入本地文件的类型声明。
2. `/// <reference types="..." />`：引入模块的类型声明（通常是从 `@types` 中加载）。



## ts模块化

### 非模块（Non-modules）

- JavaScript 规范声明任何没有 export 的 JavaScript 文件都应该被认为是一个脚本，而非一个模块

- 在一个脚本文件中，变量和类型会被声明在共享的全局作用域，将多个输入文件合并成一个输出文件，或者在HTML使用多个<script> 标签加载这些文件

- 如果你有一个文件，现在没有任何import 或者export，但是你希望它被作为模块处理，添加 

  ```typescript
  export {}
  ```

- 这会把文件改成一个没有导出任何内容的模块，这个语法可以生效，无论你的模块目标是什么



### 类型导入

导入的是类型的话，推荐加上type关键字

```typescript
import { type IFoo, type IDType } from './foo'
```

可以让一个非TypeScript 编译器比如Babel、swc或者esbuild知道什么样的导入可以被安全移除



Babel：es6以上转es5，ts转js







### 命名空间namespace（了解）

在ts早期时成为内部模块，将一个模块内部进行作用域划分，防止命名冲突

```typescript
export namespace Time {
    export function format(time: string){
        return "2022-10-01"
    }
    export const name = "time"
}
```



### 类型的查找

**.d.ts文件**

.d.ts 文件是 TypeScript 的类型声明文件，它们的主要作用是为 JavaScript 库提供类型支持，使我们能够在 TypeScript 中使用这些库时获得类型检查和智能提示。.d.ts 文件描述了库或模块的结构、函数、类、接口以及其他类型信息，让 TypeScript 编译器了解这些库的类型约束。



### declare

declare 关键字用于告诉 TypeScript 编译器：“某个变量、类型、模块等已经存在了”，即使它可能在当前文件中没有定义。这通常用于描述 JavaScript 库的类型信息，或者是在 TypeScript 中引用已经存在的全局变量而不实际导入它们。

1. 声明变量

如果你在 TypeScript 文件中使用了在其他地方定义的全局变量，你可以使用 `declare` 关键字来声明这个变量的类型。

```typescript
declare var myGlobalVar: string;
```

2. 声明类型

`declare` 也可以用来声明全局的类型，这通常在 `.d.ts` 文件中进行，这些文件用于定义和存储类型信息。

```typescript
// 在 .d.ts 文件中
declare type MyGlobalType = {
  name: string;
  age: number;
};
```

3. 声明模块

当使用非 TypeScript 编写的模块时，你可以使用 declare module 告诉 TypeScript 这个模块的存在，并描述它的类型。

```typescript
declare module 'some-external-module' {
  export function doSomething(): void;
}
```

4. 声明类和接口

`declare` 也可以用来声明类和接口，这在描述已有的 JavaScript 类型时非常有用。

```typescript
declare class MyClass {
  myMethod(arg: string): number;
}

declare interface MyInterface {
  myProperty: string;
}
```

5. 声明命名空间

你可以使用 `declare namespace` 来声明全局的命名空间，它可以包含类型、接口等。

```typescript
declare namespace MyNamespace {
  export interface SomeInterface {
    doSomething(): void;
  }
}
```

6. 声明文件

当你创建 `.d.ts` 文件时，你会使用 `declare` 关键字来告诉 TypeScript 这个文件只包含类型声明而不包含具体的实现。

```typescript
// some-declarations.d.ts
declare module 'another-module' {
  export function anotherFunction(): void;
}

declare module "*.png"
declare module "*.jpg"
declare module "*.svg"
```







