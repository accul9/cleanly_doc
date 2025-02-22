# JavaScript & React コーディング規則

## 1. `let` と `const` の使用

`var` の代わりに `let`（再代入可能）と `const`（再代入不可）を使用する。

```javascript
let name = "Allen"; // 再代入可能
const age = 30; // 再代入不可
```

## 2. 関数の書き方

### アロー関数 (`arrow function`)

`function` を使った定義の代わりに、アロー関数を使用する。

```javascript
// 従来の関数
function add(a, b) {
  return a + b;
}

// アロー関数
const add = (a, b) => a + b;
```

### 引数の省略

引数が 1 つだけの場合は、括弧を省略できる。

```javascript
const square = (x) => x * x;
```

### 戻り値の省略

関数の中身が `return` だけの場合、`{}` を省略できる。

```javascript
const multiply = (a, b) => a * b;
```

### 戻り値がオブジェクトの場合

オブジェクトを返すときは `()` で囲む。

```javascript
const createUser = (name, age) => ({ name, age });
```

## 3. 分割代入 (`Destructuring Assignment`)

オブジェクトや配列から値を直接取り出す。

```javascript
const user = { name: "Allen", age: 30 };
const { name, age } = user; // name, age を変数として取得

const numbers = [1, 2, 3];
const [first, second] = numbers; // 配列の分割代入
```

## 4. React のコーディング規則

### 関数コンポーネントを使用

クラスコンポーネントではなく、関数コンポーネントと React Hooks を使用する。

```jsx
const MyComponent = () => {
  return <div>Hello</div>;
};
```

### React Fragments（フラグメント）の使用

フラグメントを使用することで、不要な `<div>` を省略し、DOM のネストを減らすことができる。

```jsx
const MyComponent = () => (
  <>
    <h1>Hello</h1>
    <p>Welcome to my app</p>
  </>
);
```

フラグメントには `<>...</>` の短縮記法と `<React.Fragment>...</React.Fragment>` の記法がある。

```jsx
import React from "react";

const MyComponent = () => (
  <React.Fragment>
    <h1>Hello</h1>
    <p>Welcome to my app</p>
  </React.Fragment>
);
```

**違い:**

- `<></>` は **属性（props）を指定できない**。
- `<React.Fragment>` は `key` などの **属性を設定できる**。

**例：リストレンダリングで `key` を指定する場合**

```jsx
const items = ["Apple", "Banana", "Cherry"];

const ItemList = () => (
  <>
    {items.map((item, index) => (
      <React.Fragment key={index}>
        <h2>{item}</h2>
        <p>Delicious fruit</p>
      </React.Fragment>
    ))}
  </>
);
```

フラグメントを使用すると、余計な `<div>` を減らしてコードをすっきりさせることができる。

### useState の初期値を明示

```jsx
const [count, setCount] = React.useState(0); // 数値型
const [user, setUser] = React.useState(null); // オブジェクト or null
const [items, setItems] = React.useState([]); // 配列
```

### useEffect の依存配列を適切に設定

```jsx
useEffect(() => {
  console.log("Component mounted");
}, []);

useEffect(() => {
  console.log(`Count updated: ${count}`);
}, [count]);
```

### JSX の記法

```jsx
const MyComponent = () => (
  <div>
    <h1>Hello</h1>
    <p>Welcome to my app</p>
  </div>
);
```

### props の書き方

```jsx
const Greeting = ({ name = "Guest" }) => <h1>Hello, {name}!</h1>;
```

### コンポーネントの命名規則

PascalCase を使用する。

```jsx
const MyButton = () => <button>Click Me</button>;
```

## 5. Named Export と Default Export

この部分はルールではないような気もしますので、削除していいかもしれません。

### Named Export（名前付きエクスポート）

```javascript
// export.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
```

```javascript
// import
import { add, subtract } from "./export";
```

### Default Export（デフォルトエクスポート）

```javascript
// export.js
const multiply = (a, b) => a * b;
export default multiply;
```

```javascript
// import
import multiply from "./export";
```
