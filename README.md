# Typescriptコーディング規約

1. ローカル変数宣言には、極力constを使用します。
```
  // good
  function func() {
    const variable = 123;
    return variable; // 参照できる
  }

  // bad
  function func() {
    var variable = 123;
    return variable; // 参照できる
  }
```

2. 変数宣言にはconstを使用します。極力letは使用しません。どうしても再代入する場合のみ、letを使用します。
```
  // bad
  var firstName = "hoge";
  // good
  const lastName = "fuga";
```

3. 等価演算子(==)は使用せず、厳密等価演算子(===)を使用します。ただしNULL CHECKのみ等価演算子を使ってもよいです。 

```
  const num = "1";
  const value = null;
  // bad
  num == 1

  // good
  num === 1

  value == null

```
4. 明示的に使用不可能にするために、null、undefinedどちらも使用しないことを推奨します。

    理由: これらの値は、値間の一貫した構造を維持するためによく使用されます。TypeScriptでは型を使用して構造を表します

```
  // bad
  const foo = { x: 123, y: undefined };

  // good
  const foo: { x: number, y?: number } = { x: 123 };
```

5. どうしても必要な場合は、undefinedを使用してください。

    `{valid:boolean,value?:Foo}`のなどのオブジェクトを返すことを検討してください。

```
  // bad
  return null;

  // good
  return undefined;
```

6. 後置!の使用を禁止します。

    理由: 値がnullやundefinedかもしれない可能性を無視するため。

7. 三項演算子より論理チェックを使ってオブジェクトが存在するか確認します。
```
  x || y or x ?? y instead of x ? x : y:
```

8. 配列にアノテーションをつけることを推奨します。

```
  // bad
  foos: Array<Foo>

  // good
  foos: Foo[]
```

    理由: 読みやすいため。TypeScriptチームによって使用されています。


9. number型、string型、any型をそのまま真偽値として使用しません。

    理由:数値や文字列をそのまま真偽値として使うと、下記のように予期せぬ結果を招くことがある。

```
  数値は、0かNaN の場合は false
  文字列は、空文字 "" の場合は false
```

10. 整合性が取れない型キャストを強引にしない。 
```
  {} as any or {} as TypeX
```


11. function式よりもアロー関数式を推奨します。
    理由: 関数を短く書ける。thisを束縛しない。


12. オブジェクトのプロパティにアクセスする場合は.を使用します。 

    理由: 統一をすることで読みやすくなるため。

13. 文字列の合成にはTemplate literal（``）を使用します。

    理由: +などを利用して結合するよりも標準的でコードがわかりやすくなるため。

例:
```
  const firstName = "hoge";
  const lastName = "fuga";
  const fullName = `${firstName} ${lastName}`;
```

14. 制御構文やコールバックのネストは2回までを目安とし、それ以上の場合は関数に分割します。


15. ユニオン型や交差型が必要な場合にはtypeを使います：
```
  type Foo = number | { someProperty: number }
```

16. extendやimplementsをしたいときはinterfaceを使います。
```
  interface Foo {
  foo: string;
  }
  interface FooBar extends Foo {
  bar: string;
  }
  class X implements FooBar {
  foo: string;
  bar: string;
  }
```

17. `default export`　ではなく、`named exports`を推奨しています。

    理由: `defalut export`だと、import 側の裁量で対象を自由に命名できるため。


18. 型アノテーション (型注釈) は一目見てデータ型が推測できる場合のみ省略可能とする。関数の引数、戻り値の型アノテーションは省略しない。


19. もしコンポーネントが関数のオプショナルなパラメーターを取る場合は、以下のパターンを使ってチェックしてから関数を呼び出します。

```
// bad
optionalFunction?.()

// good 
if (optionalFunction) optionalFunction()
```


```
interface CompanyListProps {
  onNavigate?: () => void
}

const CompanyList: React.FC<CompanyListProps> = ({companies, containerClass, itemClass, onNavigate, followBadge}) => {
  ...
  const onClick = (e: React.MouseEvent<HTMLElement>) => {
    ...
    if (onNavigate) onNavigate()
  }
  ...
}
```

20. Key RemappingよりもRecordを使用することを推奨します。
```
// bad
Record[key in Locale]: string
// good
Record<Locale, string>

// example
export type HeadingsHighlightColors = Record<string, HighlightColors>
```

21. Enumの禁止 Union型を使いましょう
```
以下リンクに理由の詳細
```
https://engineering.linecorp.com/ja/blog/typescript-enum-tree-shaking/
