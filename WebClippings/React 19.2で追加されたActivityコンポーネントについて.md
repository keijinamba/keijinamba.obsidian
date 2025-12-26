---
type: "web-clipping"
title: "React 19.2で追加された<Activity>コンポーネントについて"
source: "https://tech.iimon.co.jp/entry/2025/12/25"
author:
  - "[[Hodiland]]"
site: "iimon TECH BLOG"
domain: "iimon.co.jp"
published: 2025-12-25
created: 2025-12-26
description: "はじめに こんにちは、保田です。本記事はiimonアドベントカレンダー25日目の記事となります。 試験的機能として開発されていた<Activity>コンポーネントが、2025年10月にリリースされたReact 19.2で正式に導入されました。 普段の業務で使えるものなのか気になったので、今回調べてみることにしました。 https://ja.react.dev/reference/react/Activity タブ切り替えで状態が消える問題 Reactでタブを切り替えるとき、よく使われるのが条件付きレンダリングです。 import { useState } from 'react'; const…"
favicon: "https://tech.iimon.co.jp/icon/link"
image: "https://cdn.image.st-hatena.com/image/scale/a44add1b0e3005c21aa3cb50dc55bb333496815e/backend=imagemagick;version=1;width=1300/https%3A%2F%2Fcdn-ak.f.st-hatena.com%2Fimages%2Ffotolife%2FH%2FHodiland%2F20251223%2F20251223153351.png"
summarized: false
tags:
  - "WebClipping"
  - Programming
  - Javascript
  - React
related:
in:
  - "[[MOC/WebClipping]]"
---

## はじめに

こんにちは、保田です。本記事はiimon [アドベントカレンダー](https://d.hatena.ne.jp/keyword/%A5%A2%A5%C9%A5%D9%A5%F3%A5%C8%A5%AB%A5%EC%A5%F3%A5%C0%A1%BC) 25日目の記事となります。

試験的機能として開発されていた `<Activity>` [コンポーネント](https://d.hatena.ne.jp/keyword/%A5%B3%A5%F3%A5%DD%A1%BC%A5%CD%A5%F3%A5%C8) が、2025年10月にリリースされたReact 19.2で正式に導入されました。 普段の業務で使えるものなのか気になったので、今回調べてみることにしました。

[https://ja.react.dev/reference/react/Activity](https://ja.react.dev/reference/react/Activity)

## タブ切り替えで状態が消える問題

Reactでタブを切り替えるとき、よく使われるのが条件付き [レンダリング](https://d.hatena.ne.jp/keyword/%A5%EC%A5%F3%A5%C0%A5%EA%A5%F3%A5%B0) です。

```tsx
import { useState } from 'react';

const App = () => {
  const [tab, setTab] = useState<'A' | 'B'>('A');

  return (
    <div>
      <button onClick={() => setTab('A')}>Tab A</button>
      <button onClick={() => setTab('B')}>Tab B</button>

      {/* タブを切り替えるとコンポーネントが消えてしまう */}
      {tab === 'A' && <ContentA />}
      {tab === 'B' && <ContentB />}
    </div>
  );
};
```

この処理では、タブを切り替えたときに状態が消えてしまうという問題があります。 その結果、以下のような不都合が起きる場合があります。

- `<ContentB />` に入力した内容が、Tab Aに移動した瞬間に消える
- `<ContentA />` でスクロールした位置が、タブを切り替えるとリセットされる
- 各 [コンポーネント](https://d.hatena.ne.jp/keyword/%A5%B3%A5%F3%A5%DD%A1%BC%A5%CD%A5%F3%A5%C8) の内部状態がすべて初期化される

「この箇所の状態はそのまま保持しておきたい」というケースは、普段の開発でもよくあるのではないでしょうか。

### これまでの解決策と、その問題点

この問題に対して考えられる回避策はいくつかありますが、デメリットを伴う場合もあります。

- **グローバルState**
	- やり方：ReduxやContext等で状態を外部に保存
	- 問題点：単純なUI状態管理に対しては過剰
- **[CSS](https://d.hatena.ne.jp/keyword/CSS) で非表示**
	- やり方： `style={{ display: isActive ? 'block' : 'none' }}`
	- 問題点： `useEffect` が動き続ける
- **状態のリフトアップ**
	- やり方：親 [コンポーネント](https://d.hatena.ne.jp/keyword/%A5%B3%A5%F3%A5%DD%A1%BC%A5%CD%A5%F3%A5%C8) で全状態を管理
	- 問題点：バケツリレーなど、コードが複雑化することがある

一番シンプルなやり方は [CSS](https://d.hatena.ne.jp/keyword/CSS) での非表示ですが、見えなくなっても `useEffect` が動き続けるという問題は解決できません。 また、グローバルStateで状態を外部に保存する方法は、コードが複雑化しがちで避けたいところです。

```tsx
// CSSで隠す方法の問題点
const HiddenComponent = () => {
  useEffect(() => {
    const handleScroll = () => {
      console.log('スクロール処理が実行され続ける');
    };
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  return <div>テスト</div>;
};
```

## <Activity> の基本

### 基本的な使い方

`<Activity>` の使い方は簡単で、 `mode` で表示と非表示を切り替えるだけです。

```tsx
import { useState, Activity } from 'react';

const App = () => {
  const [tab, setTab] = useState<'A' | 'B'>('A');

  return (
    <div>
      <button onClick={() => setTab('A')}>Tab A</button>
      <button onClick={() => setTab('B')}>Tab B</button>

      {/* Activityで状態を保ったまま切り替え */}
      <Activity mode={tab === 'A' ? 'visible' : 'hidden'}>
        <ContentA />
      </Activity>
      <Activity mode={tab === 'B' ? 'visible' : 'hidden'}>
        <ContentB />
      </Activity>
    </div>
  );
};
```

`mode` は2つの値を取ります。

- `'visible'`: 普通に表示。 `useEffect` も動く
- `'hidden'`: 非表示。 `useEffect` は止まる（状態は残る）

※ `mode` のデフォルト値は `'visible'` です。

### <Activity>では何が行われているか？

`<Activity>` の動きを理解すると使用イメージが掴みやすくなります。 内部でどのような処理が行われているかを解説します。

`mode='hidden'` になったとき

1. [CSS](https://d.hatena.ne.jp/keyword/CSS) の `display: none` で見えなくする
2. `useEffect` のクリーンアップ関数を実行する（タイマー解除やイベントリスナーの削除など）
3. ただし、状態は残しておく

`mode='visible'` に戻ったとき

1. 保存しておいた状態を使って元通りに表示
2. `useEffect` を再開する

`<Activity>` の特徴は、 `hidden` にしても状態が破棄されないことです。 各手法の違いを比較すると以下のようになります。

| 手法 | DOM | 状態 | useEffect |
| --- | --- | --- | --- |
| 条件付き [レンダリング](https://d.hatena.ne.jp/keyword/%A5%EC%A5%F3%A5%C0%A5%EA%A5%F3%A5%B0) | 削除される | 破棄される | 停止（アンマウント） |
| [CSS](https://d.hatena.ne.jp/keyword/CSS) (`display: none`) | 残る | 保持される | 動き続ける |
| `<Activity mode="hidden">` | 残る | 保持される | 停止（クリーンアップ実行） |

## <Activity> の活用パターン

### 内部状態を保持する

サイドバー、 [アコーディオン](https://d.hatena.ne.jp/keyword/%A5%A2%A5%B3%A1%BC%A5%C7%A5%A3%A5%AA%A5%F3) 、折りたたみメニューなどで、展開状態や選択状態を保持したい場合に便利です。 展開状態や選択状態が消えてしまう問題を解決できます。

```tsx
// Before - 状態が消えてしまう
{isShowingSidebar && <Sidebar />}

// After - 状態が保持される
<Activity mode={isShowingSidebar ? 'visible' : 'hidden'}>
  <Sidebar />
</Activity>
```

サイドバー内の [アコーディオン](https://d.hatena.ne.jp/keyword/%A5%A2%A5%B3%A1%BC%A5%C7%A5%A3%A5%AA%A5%F3) を開いた状態などが、非表示→再表示しても残ります。 これだけで状態を保持できるのは嬉しいです。

### 入力内容を保持する

ページ切り替えやステップ形式のフォームなどで、入力内容を保持したい場合に便利です。 テキストエリアの入力内容が戻ってしまう問題を解決することができます。

```tsx
import { useState, Activity } from 'react';

const App = () => {
  const [page, setPage] = useState<'A' | 'B'>('B');

  return (
    <>
      <button onClick={() => setPage('A')}>Page A</button>
      <button onClick={() => setPage('B')}>Page B</button>

      <hr />

      <Activity mode={page === 'A' ? 'visible' : 'hidden'}>
        <PageA />
      </Activity>
      <Activity mode={page === 'B' ? 'visible' : 'hidden'}>
        <PageB /> {/* ここでの入力中の内容などが消えない */}
      </Activity>
    </>
  );
};
```

テキストエリアに書きかけの内容があっても、ページを切り替えてから戻った際も、内容が消えずにそのまま残ります。

## ハマりポイント

`<Activity>` は便利ですが、いくつか注意すべき点もあります。

### 動画や音声が止まらない

**問題:**`<video>` や `<audio>` は、 `hidden` にしても再生が止まりません。 見えなくなるだけで、DOMは残っていることが原因です。 当然といえば当然ですが、うっかりハマりやすいポイントです。

```tsx
// hidden状態でも動画が再生され続ける
<Activity mode={isActive ? 'visible' : 'hidden'}>
  <video src="/movie.mp4" autoPlay />
</Activity>
```

**解決策:**`useLayoutEffect` で自分で止める処理を書くことによって解決することができます。

```tsx
import { useRef, useLayoutEffect } from 'react';

// 解決方法は、hidden時に再生を停止
const Video = () => {
  const ref = useRef<HTMLVideoElement>(null);

  useLayoutEffect(() => {
    const videoRef = ref.current;
    return () => {
      videoRef?.pause(); // hiddenになったら一時停止
    };
  }, []);

  return <video ref={ref} controls playsInline src="/movie.mp4" />;
};
```

`useLayoutEffect` のクリーンアップはUIの変更と同期的に実行されるため、 [コンポーネント](https://d.hatena.ne.jp/keyword/%A5%B3%A5%F3%A5%DD%A1%BC%A5%CD%A5%F3%A5%C8) が非表示になるタイミングで確実に再生を停止することができます。

### テキストだけのコンポーネントは何も出力されない

**問題:** テキストだけを返す [コンポーネント](https://d.hatena.ne.jp/keyword/%A5%B3%A5%F3%A5%DD%A1%BC%A5%CD%A5%F3%A5%C8) は、 `hidden` モードではDOMに何も出力されません。

```tsx
// hiddenモードでは何も出力されない
const ComponentThatJustReturnsText = () => 'テストテキスト';

<Activity mode="hidden">
  <ComponentThatJustReturnsText />
</Activity>
```

`<Activity>` は子要素のホスト要素（ `div` や `span` などのDOM要素）に `display: none` を適用することで非表示を実現しています。 テキストノードには対応するDOM要素がないため、 `hidden` モードでは何も出力されません。

**解決策:** divやspanで囲むことで正しく非表示にできます。

```tsx
// spanなどでラップすれば動く
<Activity mode="hidden">
  <span>
    <ComponentThatJustReturnsText />
  </span>
</Activity>

// または、コンポーネント側でDOM要素を返す
const TextWithWrapper = () => <div>テストテキスト</div>;
```

## いつ使えばいいか？

`<Activity>` は例えば、以下のような場面で有効です。

- ページ切り替え
- サイドバーの開閉
- フォームのステップ移動など

判断基準は「頻繁に表示/非表示が切り替わり、状態を残したいかどうか」で考えるのが良さそうです。

### おわりに

`<Activity>` は、状態を残したままUIを切り替えたいという問題に対して、とても便利な [コンポーネント](https://d.hatena.ne.jp/keyword/%A5%B3%A5%F3%A5%DD%A1%BC%A5%CD%A5%F3%A5%C8) です。 これまでライブラリを使ったり、いろいろ工夫したりしていた問題が、React本体の機能でシンプルに解決できるようになりました。 主なメリットは以下の通りです。

- **コードがシンプルになる** - 状態管理の複雑さが減る
- **パフォーマンスが良くなる** - `useEffect` の適切な停止と再開
- **ユーザー体験が良くなる** - 入力内容や操作状態が消えない

普段の業務で遭遇する「フォームの入力内容が消える」「サイドバーの状態がリセットされる」といった問題に対して、シンプルに解決できる手段だと感じました。 次に該当するケースに遭遇したら、積極的に使っていこうと思います。 それでは、良いクリスマスを！ 今年一年皆さん本当にお世話になりました。

ここまで読んでくださってありがとうございます！

弊社ではエンジニアを募集しています！少しでもご興味がありましたら、ぜひカジュアル面談でお話しましょう！

[iimon採用サイト](https://recruit.iimon.co.jp/) / [Wantedly](https://www.wantedly.com/companies/company_2248610)

## 参考リンク

- [https://react.dev/reference/react/Activity](https://react.dev/reference/react/Activity)

---

# NotebookLM 要約



---