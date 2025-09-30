## JSX記法
- JavaScriptの中でHTMLのようなタグがかける記法
- 関数の返却値としてHTMLのタグが記述でき、コンポーネントとして扱える
### JSXの重要なルール
- return以降は1つのタグで囲われている必要がある
    ```js
    //NG
    const App = () => {
        return (
            <h1>こんにちは！</h1>
            <p>お元気ですか？</p>
        );
    };
    ```
    ```js
    //OK
    const App = () => {
        return (
            <div>
                <h1>こんにちは！</h1>
                <p>お元気ですか？</p>
            </div>
        );
    };
    ```
- 回避方法としては、divや空タグを入れるかFragmentを利用
```js
<>
    <h1>こんにちは！</h1>
    <p>お元気ですか？</p>
</>
```
```js
import { Fragment } from "react";
<Fragment>
    <h1>こんにちは！</h1>
    <p>お元気ですか？</p>
</Fragment>
```
- エラーを回避したいだけなら、不要なDOMが出ない上記２手法がおすすめ

## コンポーネント
- 関数名をタグで囲むことでコンポーネントとして扱えるようになる（関数コンポーネント）
    ```jsx
    const App = () => {
        return <h1>こんにちは！</h1>
    };

    root.render(<App />);
    ```
- 基本的にコンポーネント用には`.jsx`拡張子を使用する（IDE補完が効くため）

## createRoot
```javascript
const root = ReactDOM.createRoot(document.getElementById("root"));
// 実際のrootはbody > divタグにid="root"が振られている
```
- SPAの場合はHTMLファイルは一つで、これを書き換えていくイメージ
- アプリケーションのルートでHTMLのどこにコンポーネントをレンダリングするか指定している

## export
- 他のファイルで定義したコンポーネントを利用するための所作
    ```jsx
    export const App = () => {
        return ...;
    };
    ```
- これにより、利用したいファイル側でimportすれば利用可能
    ```jsx
    import { App } from "<file_path>";
    ```

## Reactでイベントを割り当てる
- コンポーネント内のreturnでは`{}`で囲んだ部分にjsが書けることを前提に、以下のように記述する
    ```jsx
    const onClickButton = () => {...};
    return (
        <>
            <button onclick={onClickButton}></button>
        </>
    )
    ```

## スタイルの適用
- **JavaScriptのオブジェクト**として、style属性でCSSの各要素を書ける
  ```jsx
  <h1 style={{ color: "red" }}>こんにちは！</h1>
  ```
- 先にオブジェクトを定義しておいてから記述も可
  ```jsx
      const contentStyle = {
        color: "blue",
        fontSize: "20px"
    };
    
    return (
        <>
            <h1 style={{ color:"red" }}>こんにちは！</h1>
            <p style={contentStyle}>お元気ですか？</p>
            <button onClick={onClickButton}>ボタン</button>
        </>
    );
  ```
- JavaScriptのオブジェクトのプロパティ名ではハイフンがNG
  - `font-size`ではなく`fontSize`になる

## Props
- コンポーネントを再利用できるように、コンポーネントに対して渡せる引数のようなもの
- コンポーネント内でPropsとして指定する引数名と値を記述する。
    ```js
    <ColoredMessage color="blue" message="お元気ですか？"/>
    ```
    この場合は`color`と`message`がProps
- コンポーネント側では、上記で宣言したものをオブジェクトとして受け取れる（基本的には`props`を利用）
    ```js
    export const ColoredMessage = (props) => {
        props.color;
        proms.message; ...
    };
    ```

## children
- 特別なPropsとして定義される
- 以下の場合では、`お元気ですか？`がchildrenになる
    ```js
    return(
        <>
            <ColoredMessage color="blue">お元気ですか？</ColoredMessage>
        </>
    )
    ```
    ```js
        export const ColoredMessage = (props) => {
        props.color;
        proms.children; //ここに入る
    };
    ```
- コンポーネントのタグで挟んだ要素を、すべてchildrenに入れ込むことができる

## State
- コンポーネントの状態を管理する値
- React Hooks機能群にある`useState`という関数で利用できる
    ```js
    import { useState } from "react";
    ```
- useState関数の返却値は配列で、１つ目が変数・２つ目が変数を更新する専用の関数
    ```js
    const [num, setNum] = useState();
    ```
    - `useState(0)`のように定義すると、numの初期値を設定できる（0以外でも可）
    - 変数更新用関数は、基本的にsetXXXの形で定義する
- **関数実行直前のstateの値を取得する場合**は、変数更新用関数に対して変数を振ることで取得可
    ```js
    setNum((prev) => prev + 1)
    ```

## 再レンダリング
- DOMの変更を検知して、コンポーネントが再処理されること
- Stateが更新されると、コンポーネントは再レンダリングされて関数コンポーネントが再度頭から実行される
  - ただし`usestate(X)`で設定される初期値`X`は維持

## useEffect
- ある値が変わったときに限って、ある処理を実行する機能
  - stateが多くなってきたときに、都度再レンダリングが重たくなる
  　→特定の値が変わったときだけ動作させたいニーズに対応するため
- React Hooks機能群における`useEffect`を利用
    ```js
    import { useEffect } from "react";
    ```
- numというStateの値が変わったときのみアラートを出す場合の実装例
    ```js
    export const App = () => {
        useEffect(() => {
            alert();
        }, [num]);

        return (
            {...}
        );
    };
    ```
    - 第1引数はアロー関数、第２引数は**必ず配列で指定する**。（複数指定なら`[num1, num2]`）
    - 処理が実行されるタイミングは２つ
      1. コンポーネントのマウント時（初回実行時）
      2. 指定した配列の値
        - 逆に`[]`を設定すれば、コンポーネントを表示した"初回だけ"実行する処理が書ける
 - **StrictModeかつ開発時のみコンポーネント表示時にマウントが２度実行される**点に注意

## exportの種類
### `named export`
- 通常はこちらを利用して関数を定義する
```js
export const SomeComponent = () => {...};
import { SomeComponent } from "./SomeComponent";
```
### `default export`
- 1つのファイルで1度しか使えないので、ファイルの内容をまとめてexportするようなときのみ利用
- importする際に任意の名前をつけることができる
```js
const SomeComponent = () => {...};
export default SomeComponent;

import Some from "./SomeComponent";
```