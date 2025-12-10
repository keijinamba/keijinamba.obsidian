---
type: programming
date: 2025.10.10
links:
tags:
  - Programming
  - Chrome
  - ブラウザ
  - Debug
related:
in:
  - "[[MOC/Programming]]"
---

> [!summary] 概要
> Chrome DevToolsでCanvasやポップアップなど、通常の要素検証が難しいUIを強制的に表示させたまま検証するテクニック。Sourceタブで実行を一時停止し、hover状態を固定する。

![image](https://pub-af1bf2eacb4144a2ba4ea3ff8d6dc608.r2.dev/5c07bd074c609214a4a2a50a25d25b4e.png)

## 手順

1. DevToolsで `Sources` タブを開く。
2. 対象要素を表示させる（ポップアップやホバー状態）。
3. キーボードの `F8`（Pause/Resume script execution）を押して実行を一時停止。
4. 表示が固定された状態で `Elements` などから対象要素を検証する。

> [!tip] ショートカットが効かない場合
> タッチバーやFnキー搭載キーボードでは `Fn + F8` が必要な場合があります。
