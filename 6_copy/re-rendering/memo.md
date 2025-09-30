## 再レンダリングの条件
1. Stateが更新されたとき
2. Propsが更新されたとき
3. 再レンダリングされたコンポーネント配下のコンポーネントすべて

## React.memo
- 前回の処理結果を保持して処理を早める技術
- コンポーネントをメモ化しておき、親が再レンダリングしても子がしないようにする（パフォーマンスの観点）
  - Propsに変更がない限り、再レンダリングされない
- コンポーネント全体を括弧で囲むだけ
    ```js
    const Component = memo(()=>{});
    ```

## React.useCallBack
- Propsを渡すときにメモ化しても再レンダリングが起きてしまう要因は、関数の再生成
- 以下のように定義されていると、再レンダリングなどでこのコードが実行されるたびに、新しい関数が再生成され続けている
    ````js
    const onClickReset = () => {
        setNum(0);
    };
    ````
    - 関数自体をPropsで受け取る側はPropsが変化したとして、再レンダリングしてしまう
    ```js
    export const Child1 = memo((props) => {
    console.log("Child1 レンダリング ");

    const { onClickReset } = props;

    return (
        <div style={style}>
        <p>Child1</p>
        <button onClick={onClickReset}>リセットボタン</button>
        <Child2 />
        <Child3 />
        </div>
    );
    });
    ```
- そのため、再生成されないように定義されている部分で`useCallback(()=>{});`しておく
    ```js
    const onClickReset = useCallback(() => {
        setNum(0);
    }, []);
    ```
    - もし関数内で変数を使っている場合は、それが更新されたら動作する必要がある。
    - その場合は依存配列に変数を入れて、”それら変数が更新されたときだけ”再レンダされるように

## React.useMemo
- 変数もメモ化することができる
    ```js
    const sum = useMemo(()=>{
        return 1+3;
    }, []);
    ```
    - この場合、初回だけreturn内が計算される。
    - その後に再レンダリングされたら、最初に算出した値を使い回せるようになる
    - 変数があれば依存配列に含めておくことで、この関数に影響する変数が何かわかりやすくなる