# 型パラメータの制約

TypeScriptではジェネリクスの型パラメータを特定の型に限定する事ができます。

### ジェネリクス型パラメータで直面する問題

`changeBackgroundColor`という関数を例に考えてみます。この関数は指定されたHTML要素の背景色を変更して、そのHTML要素を返す関数です。  
ジェネリクス型 `T` を定義することで、`HTMLButtonElement` や `HTMLDivElement` などの任意のHTML要素を受け取れるようにしています。

```typescript
function changeBackgroundColor<T>(element: T) {
    // Property 'style' does not exist on type 'T'.(2339)
    element.style.backgroundColor = 'red';
    return element;
}
```

このコードはコンパイルに失敗します。ジェネリクスの型 `T` は、任意の型が指定可能なので、渡す型によっては `style`プロパティが存在しない場合があるからです。コンパイラは存在しないプロパティへの参照が発生する可能性を検知してコンパイルエラーとしているのです。

`any` を使えばコンパイルエラーを回避する事は可能ですが、型のチェックがされないので、将来バグが発生する危険性もあるので、出来る限り避けたいです。

```typescript
function changeBackgroundColor<T>(element: T) {
    // any にキャストすればコンパイルエラーは回避できる
    // 型チェックされないのでバグの可能性
    (element as any).style.backgroundColor = 'red';
    return element;
}
```

### 型パラメータに制約を付ける

TypeScriptでは `extends` キーワードを用いることで、ジェネリクスの型 `T` を特定の型に限定する事ができます。

今回の例では、 `<T extends HTMLElement>`とすることで、型 `T` は必ず `HTMLElement` またはそのサブタイプの `HTMLButtonElement` や `HTMLDivElement`である事が保証されるため、`style` プロパティに安全にアクセスできるようになります。

```typescript
function changeBackgroundColor<T extends HTMLElement>(element: T) {
    element.style.backgroundColor = 'red';
    return element;
}
```

