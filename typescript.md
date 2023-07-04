---
banner: "![[3ee89d866edcb450c164bca52bd110af5d53e248.jpg@942w_564h_progressive_waifu2x_art_noise3_scale.png]]"
banner_y: 0.5
---
# 基础类型和使用方法
````ad-seealso
title: typescript的类型声明和定义
```typescript
// 基础类型 number string boolean 
let num: number = 1
let str: string = '11'
let bl: boolean = true

// 空类型 null undefined void
let nu: null = null
let unde: undefined = undefined
let vo: void = void
```

```ad-warning
title: null 和 undefined 是所有类型的子集，所以void类型中也可以声明null 和 undefined
```ad-success
~~~typescript
let vo1: void = null
let vo2: void = undefined
~~~
```
```ad-error
title: 但是反之null 和 undefined类型中不能声明void
~~~typescript
let vo3: undefined = void
let vo4: null = void
~~~
```
```
````
# 类型推论
如果在声明变量的时候没有给出类型，则typescript会`自动推导`出一个类型
```typescript
let str - '123'
----------> 等价于
let str: string = '123'
```
```ad-warning
title: 如果变量没有被赋值，则typescript会推导成一个any类型, 随后无论赋值给什么都是any类型
```typescript
let str;
----------> 等价于
let str: any;
let str: any = '123'
```
`any类型可以不会被约束，可以任意访问属性和方法`

# 联合类型
`在一个变量被定义类型后，如果想改成另一个类型，则可以使用联合类型`
````ad-seealso
title: typescript的联合类型声明和定义
```typescript
let str: string | number;
let str1 = '123'
let str2 = 123
```

```ad-warning
title: 联合类型会校验类型的共同的方法和属性
```ad-success
~~~typescript
let str: string | number;
let str1 = '123'
let str2 = 123

console.log(str1.length)
~~~
```
```ad-error
title: 因为number类型并没有length这个方法，所以报错
~~~typescript
console.log(str2.length)
~~~
```
```
````
# 接口
`接口是用来描述一个抽象事物的数据结构，接口可以用类去实现`
````ad-seealso
title: typescript的接口类型声明和定义
```typescript
// 普通接口
interface User {
	id: string,
	name: string
}
let user: User = {
	id: '123',
	name: 'xxxx'
}

// 带有可选属性的接口，可选属性意味着在访问属性的时候可有可无
interface User {
	id: string,
	name: string,
	age?: number
}
let user: User = {
	id: '123',
	name: 'xxxx'
}

// 只读属性，只读属性在第一次赋值的时候会保存，初始化的时候不会
interface User {
	id: string,
	readonly name: number
}
let user: User = {
	id: '123',
	name: 'xxxx',
	aa: 123
}

// 允许接口接受任意值
interface User {
	id: string,
	name: string,
	[propName: string]: any
}
let user: User = {
	id: '123',
	name: 'xxxx',
	aa: 123
}

```

```ad-warning
title: 接口接受任意值的时候必须是所有类型的子集才行
`[propName: string]: string | number` 代表的是user的索引的类型是字符串时，那么值的类型必须是字符串或者数字
```ad-success
title: 任意值的类型满足其他字段的类型的子集
~~~typescript
interface User {
	id: string,
	name: number,
	[propName: string]: string | number
}
let user: User = {
	id: '123',
	name: 'xxxx',
	aa: 123
}
~~~
```
```ad-error
title: 因为number类型并不是propName的子集，所以报错
~~~typescript
interface User {
	id: string,
	name: number,
	[propName: string]: string
}
let user: User = {
	id: '123',
	name: 'xxxx',
	aa: '123'
}
~~~
```
```ad-error
title: 使用user[id] = '456' 索引类型不满足[propName: number]: string的规则
~~~typescript
interface User {
	id: string,
	name: number,
	[propName: number]: string
}
let user: User = {
	id: '123',
	name: 'xxxx',
	aa: '123',
	1: '456'
}
~~~
```
```
````
# 数组
数组定义使用`类型 + []`的方式
````ad-seealso
title: typescript的数组类型声明和定义
```typescript
// 普通定义   推荐
let arr: number[];
arr = [1,2,3]

// 泛型定义   推荐
let arr: Array<number> = [11,2,3]

// 接口定义  通常接口用来定义类数组，不推荐去定义数组
interface Arr {
	[index: number] = number
}

let arr:Arr = [1,2,3,4]
```

```ad-success
title: 最常见的用法，使用any去定义数组类型
~~~typescript
let arr: any[] = [1, 'str', null, undefined, true]
~~~
```

```ad-success
title: 用接口去定义arguments类数组
~~~typescript
interface IArguments {
	[index: number]: any
	length: number;
    callee: Function;
}
~~~
```
````
# 函数
````ad-seealso
title: typescript的函数类型声明和定义
```typescript
// 函数表达式定义  根据=>来判断，箭头左边表述参数类型，右边表示返回值类型 
let fun: (a: number, b: number) => number = function (a, b) {
	return a + b
}

// 函数字面量定义  直接在参数中进行类型约束
function fun (a: number, b: number):number {
	return a + b
}

// 函数参数默认值  在参数类型后面加入=赋予默认值
function fun1 (a: number = 2, b: number = 3): number {
	return a + b
}

// 可选参数        在参数类型前面加入?，可选参数不能在必要参数之前
function fun2 (a: number, b?: number): number {
	return a + b
}

// 获取参数
function fun3 (a:number, ...argu: any[]): number {
	return a
}

// 函数重载   允许传入不同的参数，每次传参对应一个函数签名，所以在最后的函数声明中需要兼容上面的所有类型
function fun4 (a: number): number
function fun4 (a: string, b: string): string
function fun4 (a: number | string, b?: string): number | string {
	return a + b
}
```
````
# 类型断言
## 语法
通过统一使用`值 as 类型`这种方法，例如：`123 as number`
## 使用场景1：将联合类型进行更具体的约束
````ad-seealso
title: 函数fn调用接受参数中的run方法，但是参数可能没有这个方法
```typescript
interface obj1 {
	name: number
	play: () => void
}

interface obj2 {
	name: number
	run: () => void
}

let obj1: obj1 = {
	name: 123,
	play: () => {}
}

let obj2: obj2 = {
	name: 234,
	run: () => {}
}
```
```ad-error
title: JS写法：以下代码不会过ts的编译，因为obj2没有play这个方法
```typescript
function fn (obj: obj1 | obj2) {
  obj.play()
}
```
```ad-success
title: 使用as来骗过ts编译阶段，但是可能会导致代码执行出错
```typescript
function fn (obj: obj1 | obj2) {
  (obj as obj1 ).play()
}
```
```


```
````

123