# 类型推论

如果没有指定一个明确的类型，那么 TypeScript 会根据类型推论 Type Inference 的规则推断出一个类型

```typescript
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
``` 

上面的代码会报错，因为上面的代码其实等价于下面的代码

```typescript
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;
```

**但是，如果定义的时候没有赋值，不管后面有没有赋值，都会被推断为 any 类型，而完全不被类型检查**

```typescript
let myFavoriteNumber;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```
