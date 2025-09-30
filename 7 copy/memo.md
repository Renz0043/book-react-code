- Reactにもともと備わっているグローバルなState管理機能として、`Context`がある
  - 一つずつPropsを下にあるコンポーネントに渡してもできるが、不要なコンポーネントの再レンダリングが発生する。
  - これを抑止するために**グローバルなState管理**を行う必要がある
### useContextを使う手順
1. `React.createContext`でContextの器を作成する（Providerコンポーネントを作る）
    ```js
    import { createContext } from "react";
    export const AdminFlagContext = createContext({});
    ```

2. 作成したContextの`Provider`でグローバルStateを扱いたいコンポーネントを囲む
    ```js
    export const AdminFlagProvider = props => {
        const { children } = props;

        const sampleObj = { sampleValue: "テスト" };

        return (
            <AdminFlagContext.Provider value={sampleObj}>
                {children}
            </AdminFlagContext.Provider>
        );
    };
    ```
    - AdminFlagProviderはあくまでProviderする関数としているだけ
    - **`AdminFlagContext`に`Provider`があり、グローバルにしたい値が`value`に入るという点が重要**

    ```js
    import { AdminFlagProvider } from "./components/providers/AdminFlagProvider";

    const root = ReactDOM.createRoot(document.getElementById("root"));
    root.render(
    <AdminFlagProvider>
        <App />
    </AdminFlagProvider>
    );
    ```
    - 先ほど作成した`AdminFlagProvider`で`<App />`を囲んでいる
3. Stateを参照したいコンポーネントで`React.useContext`を使う
    ```js
    const value = useContext(AdminFlagContext);
    ```
### グローバル管理すべきポイント
- アプリケーションのいろんなところから呼ばれる値
  - ログインしているユーザ情報など
### 再レンダリングのタイミング
- Contextを利用する場合、再レンダリングされるのは**1つのContextオブジェクトの値が何か更新されたとき**。
- **そのContextを参照しているコンポーネントはすべて再レンダリングされる**点にも注意。
  - たとえその値を使っているだけで更新しなくても再レンダリングされる
  - なので実際の値と更新関数を別のContextとして管理することで、値を見ているだけなら再レンダリングせずに済む実装も可