## inline Styles
- JavaScriptのオブジェクトの形で定義できるCSS
- タグに直接記述するので可読性が下がりがち
    ```js
    <div style={{ width: 100%, padding: "16px" }}>
    ```

## CSS Modules
- cssやscssで定義する方法
- コンポーネント（XXX.jsx）などのファイルと対になるようcss/scssを記述する。
  - css側のファイル名称は必ず（`XXX.module.css`）にする
- 利用時はsassをnpmにインストールすること
```bash
npm install sass
```
### 実装例
- CSS側: `study/React/book-react-code/5/react-css/src/components/CssModules.module.scss`
- コンポーネント側: `study/React/book-react-code/5/react-css/src/components/CssModules.jsx`
  - コンポーネント側では、returnの部分でタグ内に`className`でクラス名を付与

## Styled JSX
- Next.jsに標準組み込みされているライブラリ。
    ```bash
    npm install styled-jsx
    ```
- **Scss記法は利用できない点に注意！（別途ライブラリなどインストールする）**
- コンポーネントファイルにCSSを書いていく（CSS-in-JS）。JSX記法なのでreturn以下はフラグメント等、１つのタグにまとめる
    ```js
    export const Button = () => {
        return (
            <>
                <p className="name">なまえ</p>

            <style jsx>{`
                .name {
                    color: #aaa;
                }
            `}</style>
            </>
        )
    };
    ```

## styled components
```bash
npm install styled-components
```
- スタイルを当てたコンポーネントを定義するライブラリ。
- divタグなどにスタイルがついたタグを新たに作成して、JSXで利用するイメージで直感的
- **冒頭のインポートが必要な点に注意する**
    ```js
    import styled from 'styled-components';
    export ABC = () => {
        return (
            <>
                <SBeautifulName>なまえ</BeautifulName>
            </>
        );

        const SBeautifulName = styled.p`
            color: #bbb;
        `;
    };
    ```
- 慣習的に、最初に大文字のSをつけて定義する

## Emotion
```bash
npm install @emotion/react @emotion/styled(styled-components利用時)
```
- ここまで紹介された`inline Styles`、`Styled JSX`、`styled components`など色々な書き方に対応している
- **冒頭でインポートが必要な点に注意する**
    ```js
    import { jsx, css } from "@emotion/react";
    import styled from "@emotion/styled";(styled-components利用時)
    ```

## Storybook
- コンポーネントのカタログのようなもの

## Tailwind Css
- ユーティリティファーストなフレームワーク
- クラス名として設定するようなflex, text-centerなどのみを提供する
    ```js
    export const TailwindCss = () => {
        return (
            <div className="border border-gray-400 rounded-2xl p-2 m-2 flex justify-around items-center">
                <p className="m-0 text-gray-400">TailwindCss</p>
                <button className="bg-gray-300 border-0 p-2 rounded-md hover:bg-gray-400 hover:text-white"> ボタン </button>
            </div>
        );
    };
    ```
- 初期設定が少し重たいが、以降はclassName付与だけでほぼ完結する
  - `tailwind.config.js`
  - `index.css`
  - `index.js` //import "./index.css";
