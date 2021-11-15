---
title: type vs. interface
date: 2021-05-26 20:21:10
tags: Typescript
---

# type vs. interface

使用 `typescript` 时相信不少人和我一样，时常会对 `interface` 与 `type` 这两个关键字感到疑惑，关于两者的异同官方并没有系统的说明，所以下面是参考官网及 Stackoverflow 相关问答的补充整理：

## 语法比较

定义一个类型：

```ts
interface User {
  name: string;
  age: number;
}
type User = {
  name: string;
  age: number;
};
```

定义一个方法：

```ts
interface SetName {
  (name: string): void;
}
type SetName = (name: string) => void;
```

## 可定义类型差异

`interface` 只能表示对象类型，`type` 可声明所有类型,包括基本基本数据类型(undefined, null, boolean, string, number)、联合和交集类型，所以 `type` 更加灵活。

```ts
// 基本数据类型
type MyString = string;

// 对象
type ObjectOne = { a: string };
type ObjectTwo = { b: string };

// 联合
type AllObject = ObjectOne | ObjectTwo;

// 交集
type SameObeject = ObjectOne & ObejctTwo;

// tuple
type Data = [number, string];
```

## 扩展方式

两者都可以扩展，且两者的扩展并不互斥，`interface` 可以扩展 `type`，反之亦然。

`interface` extends `interface`

```ts
interface PartialPointX {
  x: number;
}
interface Point extends PartialPointX {
  y: number;
}
```

`type` extends `type`

```ts
type PartialPointX = { x: number };
type Point = PartialPointX & { y: number };
```

`interface` extends `type`

```ts
type PartialPointX = { x: number };
interface Point extends PartialPointX {
  y: number;
}
```

`type` extends `interface`

```ts
interface PartialPointX {
  x: number;
}
type Point = PartialPointX & { y: number };
```

## 使用差异

### 声明合并

使用 `interface` 重复定义名称相同的类型会被合并。

```ts
interface User {
  name: string;
}
interface User {
  age: number;
}
const user: User = { name: "Enivia", age: 1 };
```

### 索引

联合类型不能作为 `interface` 的索引。

```ts
type Keys = "a" | "b";

type ObjectType = {
  [key in Keys]: string;
};
```

## 总结

在大多数场景下这两者都可以相互替换。从语法和含义上来看，两者都能满足的场景下没有哪一个选择更优或更劣。

从规范性上考虑，项目可以统一使用 `interface` / `type` 提升代码观感。其它情况，其实官方 Handbook 已经给出了回答：

> Type aliases and interfaces are very similar, and in many cases you can choose between them freely.

**参考链接：**

- [Typescript Handbook](https://microsoft.github.io/TypeScript-New-Handbook/everything/#interface-vs-alias)
- [Stackoverflow interfaces-vs-types-in-typescript](https://stackoverflow.com/a/52682220/7923765)
