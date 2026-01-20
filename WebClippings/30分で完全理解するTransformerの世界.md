---
type: web-clipping
title: 30分で完全理解するTransformerの世界
source: https://zenn.dev/zenkigen_tech/articles/2023-01-shimizu
author: Zenn
site: Zenn
domain: zenn.dev
published: 2023-02-14
created: 2026-01-20T13:08:35+09:00
description:
favicon: https://static.zenn.studio/images/logo-transparent.png
image: https://res.cloudinary.com/zenn/image/upload/s--8DmImf08--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:30%25E5%2588%2586%25E3%2581%25A7%25E5%25AE%258C%25E5%2585%25A8%25E7%2590%2586%25E8%25A7%25A3%25E3%2581%2599%25E3%2582%258BTransformer%25E3%2581%25AE%25E4%25B8%2596%25E7%2595%258C%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:%25E3%2581%25AF%25E3%2581%25BE%25E3%2581%25AA%25E3%2581%2599%25E3%2581%25AA%25E3%2581%258E%25E3%2581%2595%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzNhNzk4YmFjMTcuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:ZENKIGEN%25E3%2583%2586%25E3%2583%2583%25E3%2582%25AF%25E3%2583%2596%25E3%2583%25AD%25E3%2582%25B0%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2IxZTcxOTc1ZTMuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT
summarized: false
tags:
  - WebClipping
  - Programming
  - AI
  - LLM
  - Transformer
related:
in:
  - "[[MOC/WebClipping]]"
---

[ZENKIGENテックブログ](https://zenn.dev/p/zenkigen_tech) [Publicationへの投稿](https://zenn.dev/faq#what-is-publication)

941

123[tech](https://zenn.dev/tech-or-idea)

## はじめに

初めまして。ZENKIGENデータサイエンスチームの [はまなす](https://twitter.com/RosaRugosaBeach) です。正式な所属はDeNAデータ本部AI技術開発部なのですが [^1] 、業務委託という形で今年度から深層学習系の開発等に携わっています。

深層学習界隈では、2017年に衝撃的なタイトル（ [Attention Is All You Need](https://arxiv.org/abs/1706.03762) ）の論文が発表されてから早5年半、元出自の機械翻訳タスクを大きく越えて、Transformer関連の技術が様々な領域で用いられる汎用アーキテクチャとして目覚ましく発展し続けています。

今回はそんなTransformerが現時点までにどのように活用されてきたか、また、どのように工夫されてきたかをざっくりと俯瞰し、流れをおさらいする目的の記事になります。本記事の大枠は、2021年時点でのサーベイ論文である [A Survey of Transformers](https://arxiv.org/abs/2106.04554) に倣いつつ、適宜、2023年2月上旬現在までの情報で簡潔に肉付けしたものになっています。

## Transformerって？

発展の系譜を眺める前に、Transformerがどんな仕組みのモデルだったかについて簡単に触れておきたいと思います。Transformer完全理解者の諸兄は本章はスキップして次章よりご覧ください。

## 構造的な話

### 最も大事な構成要素：Attention

Transformerは、Attentionと呼ばれる仕組みを効率的に積層した深層学習モデルです。Attentionをとても抽象的に表現すると、データを検索するための鍵（Key）と実際の値（Value）のペア集合に対して、問い合わせ（Query）を投げて値を取り出す操作という説明がしっくりくるのではないかなと思います。このとき、QueryとKeyは厳密に一致する必要はなく、各QueryとKeyの類似度に基づいて連続的に重みが計算され、その加重平均としてValueが引き出されます。

もう少し詳しく見ていきましょう。

深層学習では、基本的にあらゆる特徴量はベクトルを並べたテンソルとして表現されます。例えば $d_k$ 次元のベクトルとして各Queryが表され、それらが $N$ 個並んだ行列として $Q\in\mathbb{R}^{N\times d_k}$ を考えることができます。同様に、Keyを $K\in\mathbb{R}^{M\times d_k}$ 、Valueを $V\in\mathbb{R}^{M\times d_v}$ と表すと、最も基本的なAttentionは次のような行列演算の関数として表せます。

$$
{\rm Attention}(Q, K, V)={\rm softmax}\left(\frac{QK^{\top}}{\sqrt{d_k}}\right)V=AV\in\mathbb{R}^{N\times d_v},
$$

ただし、softmax関数は行方向、つまり各Queryに対して、全てのKeyへの重みの総和が $1$ になる方向に適用されるものとします。このようにして計算された $A$ は各QueryとKeyのペアの類似度を示すヒートマップのような役割を担い、一般にAttention Matrixなどと呼称されます。

さらに、各特徴量をより小さい次元のベクトルへ分割し、それぞれの分割グループでAttentionを適用して最後に結合する手法が一般に採用されます。これはMulti-Head Attentionと呼ばれ、QueryからKeyへ注目するパターンを並列で複数学習できることから表現力の向上が期待できます。

![](https://storage.googleapis.com/zenn-user-upload/ab80af93368e-20231010.png)  
*Fig.1. 後述する [BERT](https://doi.org/10.18653/v1/N19-1423) において観測されるAttentionの例。 **左図と右図がそれぞれある層のあるHeadにおけるAttention-Matrixを表しており、トークン同士が強く反応している箇所が濃い色で表示されている** 。（中央図の補足：FrameNetと呼ばれる語彙同士の接続に関するデータセットに基づき、複数の例文中でコアとなる語彙同士の繋がりを特定。BERTのSelf-Attentionがそれらの接続に関して反応した強度の平均を集計することで、そのような接続を捉えるSelf-Attentionが実際に学習されているかを判断する試み。）  
[Revealing the Dark Secrets of BERT](https://arxiv.org/abs/1908.08593) より引用。*

---

さて、このようなAttention機構ですが、 $Q, K, V$ を用意する経路によってさらに呼び名が変わります。基本的にKeyとValueはセットで与えられるため、具体的には以下のように分類できます。

- Self-Attention：ある共通の入力 $X$ に対しそれぞれの変換行列を適用して、 $Q=XW_Q,$$K=XW_K,$$V=XW_V$ を用意する。自分自身の要素との注目度合いを抽出する。
- Masked Self-Attention：自己回帰生成（順に要素を予測していくタスク）などに用いられる場合、Self-Attentionの各要素が自身より未来の要素を参照できないようにする必要がある。このため、Attention Matrixに三角状のマスクを適用し、各要素が未来の要素にアクセスできないようにすることで、過去と現在の情報のみから未来の情報を予測できるように学習させる。
- Cross-Attention：異なる入力行列 $X,$$Y$ から、 $Q=XW_Q,$$K=YW_K,$$V=YW_V$ として用意する。これは、 $X$ が異なる情報源 $Y$ から情報を抽出する処理として解釈できる。

### Transformerの全体像

前節のAttention機構を積層しつつ、線形層や正規化層を適切に挟み込んだアーキテクチャとしてTransformerは提案されました。また、対象入力を特徴ベクトルとして埋め込む層や、位置情報を符号化して付加する層も重要な構成要素です。原典の [Attention Is All You Need](https://arxiv.org/abs/1706.03762) では翻訳タスクに適用されたため、埋め込み層は言語のトークンに対する処理として実装されました。

オリジナルのTransformerは、翻訳元言語の入力文を処理するエンコーダ、および翻訳文を自己回帰生成するデコーダから構成されます。エンコーダは入力文を受け取り、Self-Attention等を繰り返し適用しながらトークン列を加工していきます（Fig.2 左側）。エンコーダの出力はデコーダのCross-Attentionに与えられ、Key-Valueを計算するために用いられます（Fig.2 左側から右側中央に伸びる矢印）。すなわち、デコーダの条件付けを計算していることになります。

翻訳文の生成を担当するのはデコーダです（Fig.2 右側）。ここでは、最初に `[BOS]` （Begin Of Sentence：文頭）というトークンだけから成る入力を用意します。これをMasked Self-AttentionやCross-Attentionを繰り返し適用しながら加工していき、最終的な出力として `[BOS]` の次のトークン、すなわち、翻訳文の最初に来るトークンを予測します。この予測結果は `[BOS]` に繋げられて再びデコーダに入力され、同様に次の次のトークンが予測されます。この処理を逐次的に繰り返すことにより、徐々に翻訳文が生成されていくのです。最終的に、デコーダが `[EOS]` （End Of Sentence：文末）という特殊トークンを出力したら生成終了です。

学習時は正解トークン列を一度にデコーダに入力しますが、Masked Self-Attentionにより、未来の情報をリークしないようにしながら全トークンを並列で処理することが可能になります。

![](https://storage.googleapis.com/zenn-user-upload/e6cedd4d4808-20231010.png)  
*Fig.2. 初期Transformerの概要。 [A Survey of Transformers](https://arxiv.org/abs/2106.04554) より引用。*

### Transformerの利活用

このようにして提案されたTransformerですが、その汎用性が認知されるにつれ、大きく以下の3つに大別される使用法が主流となってきました。

- エンコーダ・デコーダ方式：オリジナルのTransformerと同様の利用法。機械翻訳など、ある系列を異なる系列に変換するタスクにおいて典型的に用いられる。
- エンコーダのみ：入力系列の表現学習に利用。系列分類やラベリングタスクへの転用も多い。
- デコーダのみ：エンコーダとのCross-Attentionを除外し、自己回帰生成のデコーダ部のみを残した構造。LM（Language Model：言語モデル）など、生成タスクでの利用が主。

## 性質的な話

### Transformerの帰納バイアス

学習データではなく、機械学習モデルそれ自体が有している仮定や構造上の偏りを帰納バイアスと呼びます。例えば線形回帰では入出力が線形の関係にあることを暗に仮定していますし、深層学習モデルの一大巨頭であるCNNsは、重みを共有した小さな局所カーネル関数を画像全体に適用するという構造上の制約が帰納バイアスとして立ち現れます。

Transformerは扱うデータにほとんど仮定を置かないぶん、柔軟で普遍的な表現力を獲得できますが、その反面帰納バイアスに乏しく、データ量が不十分な環境では過学習に脆弱であるという弱点を持つことには留意が必要です。実際、後述する様々な事前学習手法や画像分野にTransformerを適用する手法では、従来研究と比較してもより大規模なデータが用いられており、その傾向は年々加速しています。

### Transformerのスケーリング則

前節では若干ネガティブな書き方をしましたが、一方で、 **Transformerは学習データ量が増大するほど際限なく性能向上する** 可能性が示唆されており、実験的な裏打ちや、それに伴う効率的な実践上の提案も為されています。

より具体的には、 **Transformerの性能は『（埋め込み層を除く）モデルのパラメータ数 $N$ 』『訓練データセットに含まれるトークン数 $D$ 』『訓練計算量 $C$ 』等の変数によって記述される冪乗則に従う** ことが、かのGPT系列を開発するOpenAIにより発表された2本の先行研究により指摘されています。まず [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) にてこの冪乗則が観測され、続く [Scaling Laws for Autoregressive Generative Modeling](https://arxiv.org/abs/2010.14701) にて、このスケーリング則が画像やマルチモーダルを含めた複数のドメインにも敷衍されることが発見されました。ただし、（埋め込み層を除く）訓練計算量は、バッチあたりのトークン数 $B$ と訓練ステップ数 $S$ を用いて $C\approx6NBS$ と推定できることが示されており、固定サイズの $B$ においては $D=BS$ と表せるため、実質的な束縛変数は2種類であると見做すことができます。 [^2]

さらにOpenAIはこのスケーリング則の上限を [GPT-3](https://arxiv.org/abs/2005.14165) の開発により押し上げ、今もなおより巨大なモデルによりその際限なさを実証し続けています。一方で、2022年にはさらにDeepMindから『 [パラメータサイズとデータサイズには実践的に最適なバランスが存在する](https://arxiv.org/abs/2203.15556) 』という研究成果が発表されており、スケーリング則に関する今後のさらなる理解が待たれます。

また後述するように、一定以上の大規模自然言語モデルには『創発性』が備わる場合があることも知られており、スケーリング則の予想から不連続に性能が飛躍しうることが確認されています。

2022年末は後述するChatGPTの台頭が大きな話題となりましたが、これもまさにTransformerのスケーリング則の上に実現した成果だと言えます（勿論、それを基盤に数々の工夫が込められています）。まことしやかにGPT-4が噂されはじめて久しく、直近では [GPT-3.75とでも呼ぶべきモデルが検索エンジンのBingに搭載される](https://www.bing.com/new?toWww=1&redig=03DAD8A4ED1A4ED58FD0968BBC55DD67) など慌ただしい幕開けとなった2023年ですが、本年は我々がこれまでにない新たな驚愕を甘受する年になるかもしれません。

![](https://storage.googleapis.com/zenn-user-upload/8aa777af639c-20231010.png)  
*Fig.3. GPT-3の提案論文にて示されたスケーリング則の敷衍。点線が先行研究に基づき導かれた冪乗則の理想的挙動。各実線は異なる設定で実装されたGPT-3の、学習に伴うvalidation lossの曲線。 [Scaling Laws for Autoregressive Generative Modeling](https://arxiv.org/abs/2001.08361) での観測からさらに冪乗則の水準を2桁推し進めた。黄色の曲線が収束していないのは、十分な学習資源を投入する前に当初の予算を消費したためであるとされている。 [Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165) より引用。*

## Transformerの分類体系

オリジナルの台頭以降、その汎用性の高さからあらゆる領域に浸透したTransformerですが、その派生は主に『モデルアーキテクチャの改修』『事前学習方法』『応用領域』の違いにより分類することが可能です。例えば、2021年時点のサーベイ論文では Fig.4 のように各カテゴリの代表的な手法が挙げられています。以降では主にこちらを参照しつつ、時折2022年以降の手法も交えながらTransformerの発展の系譜を眺めてみましょう。

![](https://storage.googleapis.com/zenn-user-upload/fd0cd661b00d-20231010.png)  
*Fig.4. 幾つかの観点で分類したTransformerの派生手法。 [A Survey of Transformers](https://arxiv.org/abs/2106.04554) より引用。*

## 各構成要素単位での改修

Transformerが様々な領域に応用され、そのモデルサイズやデータ規模が巨大になるにつれて、各構成要素をいかにメモリ効率良く、あるいは高速に計算するかへの関心が高まっています。特によく言及されるのは、Attentionは『入力系列長の2乗に比例して計算量が増大する』『機構それ自体が入力系列の順序を考慮できない』という性質であり、これらの弱点を緩和するアプローチを中心に、極めて多くの派生要素が提案されてきました。同時に、Transformerの要であるAttention機構を拡張することで、さらなる表現力向上を図る動きも盛んになっています。

本章ではそれらの概観を捉えると同時に、それぞれの技術的内容への橋渡しを目的に、幾つかの要素に分けてモデルアーキテクチャの改修手法を簡単に紹介します。詳細な数式や技術的背景は、 [A Survey of Transformers](https://arxiv.org/abs/2106.04554) や [Efficient Transformers: A Survey](https://arxiv.org/abs/2009.06732) 、および各原典をご覧ください。

## Attention

#### スパース化

通常のAttentionでは各要素が自身を含めた全ての要素と関わり合いますが、そのAttention Matrixの特定部分以外を計算しないようスパース化（疎行列化）することで、上述の計算複雑性を軽減しようとする研究が多数存在します。これは、通常のTransformerも学習の結果スパースなAttention Matrixを獲得していることが多いという観測に基づく工夫であり、入力モーダルやタスクに応じて要素同士の関わり方には偏りが生じることを反映することによって、精度を極力犠牲にせず計算量を削減することが可能です。場合によっては、不要な接続を排除することが精度向上に寄与することもあります。

上記を実現する主流な方法は、Fig.5 のようにトークン位置に基づいてAttention Matrixを制限することです。また Fig.6 のようにそれらを複合することで、スパース性を保ちつつより複雑な関係性を抽出する試みへも派生していきます。例えば [BigBird](https://arxiv.org/abs/2007.14062) では、計算量を系列長に比例するオーダに下げ、より長い系列を扱えるようにするため、先頭の特殊トークン付近に広域的な情報を集約し分配するマスク、局所位置に着目するマスク、およびランダムマスクを組み合わせています。

![](https://storage.googleapis.com/zenn-user-upload/8c4a9d76f252-20231010.png)  
*Fig.5. 位置に基づくスパースなAttentionの例。 [A Survey of Transformers](https://arxiv.org/abs/2106.04554) より引用。*

![](https://storage.googleapis.com/zenn-user-upload/20e17ad59874-20231010.png)  
*Fig.6. 位置に基づくスパースなAttentionを複合した例。 [A Survey of Transformers](https://arxiv.org/abs/2106.04554) より引用。*

さらに、二分木の要領で徐々に情報集約していく [BP-Transformer](https://arxiv.org/abs/1911.04070) や、画像領域に特化して、 [局所領域](https://arxiv.org/abs/1802.05751) や [軸方向](https://arxiv.org/abs/1912.12180) にAttention Matrixを制限する手法等も提案されています。

その他、入力内容に基づいてAttention Matrixのスパース性を制御する手法群も存在します。イメージとしては、各Queryに対してそれぞれ類似性が高いKey集合だけを予め取り出すことができるならば、全てのKeyに対してAttentionを計算する必要がなくなり、計算量を削減することができそうです。最大内積探索問題とも関連の深いこの問題設定において、k-meansクラスタリングを利用した [Routing Transformer](https://arxiv.org/abs/2003.05997) や、局所性鋭敏型ハッシュを利用した [Reformer](https://arxiv.org/abs/2001.04451) 、入力系列をグラフと見做してタスク特化な接続を学習する [SAC](https://arxiv.org/abs/2003.09833) 、Sinkhornアルゴリズムを用いて特定のKey集合をQueryに分配する [Sparse Sinkhorn Attention](https://arxiv.org/abs/2002.11296) など、多岐に亘る手法が提案されています。

#### 線形化

Attentionのコアとなる計算要素は ${\rm softmax}\left(QK^{\top}\right)V$ であり、非線形な $\rm softmax$ 関数に挟まれている $Q$ と $K$ の行列積を先に計算しなければならないことが、先述の計算量の原因となっています。この部分を何らかの方法で ${\rm softmax}\left(QK^{\top}\right)\simeq \hat{Q}\hat{K}^{\top}$ のように置換できれば、 $\hat{K}$ と $V$ の行列積を先に計算することで計算量を系列長に比例するオーダに削減することができます。

より正確な洞察のため、Attentionの計算を各Query, Key, Valueを特徴ベクトルで捉えた観点で見てみましょう（e.g., $Q=\left\{\mathbf{q}_i\in\mathbb{R}^{d_k}\right\}_{i=1}^{N}$ ）。このとき、通常のAttentionの出力は次のように表すことができます。

$$
\mathbf{z}_i = \sum_j\frac{\exp(\mathbf{q}_i^\top\mathbf{k}_j )}{\sum_l \exp (\mathbf{q}_i^\top\mathbf{k}_l )} \mathbf{v}_j.
$$

この内積と $\exp$ を用いた類似度関数を、ある関数 $\phi$ を適用した内積に置換して、次のように書き換えてみます。ただし、 $\mathbf{z}_i\in\mathbb{R}^{d_v}$ です。

$$
\begin{aligned}
\mathbf{z}_i^\top &= \sum_j\frac{\phi(\mathbf{q}_i)^\top\phi(\mathbf{k}_j )}{\sum_l \phi (\mathbf{q}_i)^\top\phi(\mathbf{k}_l )} \mathbf{v}_j^\top\\
&= \frac{\phi(\mathbf{q}_i)^\top\sum_j\phi(\mathbf{k}_j )\mathbf{v}_j^\top}{ \phi (\mathbf{q}_i)^\top\sum_l\phi(\mathbf{k}_l )} \\
&= \frac{\phi(\mathbf{q}_i)^\top S}{ \phi (\mathbf{q}_i)^\top\mathbf{u}}.
\end{aligned}
$$

このとき、 $S\in\mathbb{R}^{d_k\times d_v}$ かつ $\mathbf{u}\in\mathbb{R}^{d_k}$ です。 $S$ と $\mathbf{u}$ の計算には系列長に比例する計算量しか要さないため、総合的にAttentionの計算量を削減できるという理屈になっています。またこれらは自己回帰生成の際は逐次的に計算できるため、さらに効率化を図れるという副次的な利点も得られます。この文脈では、 $\phi$ にELU関数を利用した [Linear Transformer](https://arxiv.org/abs/2006.16236) や、乱択化フーリエ特徴を用いた [Performer](https://arxiv.org/abs/2006.03555) 等が提案されています。また、集約特徴量である $S$ と $\mathbf{u}$ の計算にゲート処理を取り入れた [RFA](https://openreview.net/forum?id=QtTKTdVrFBB) や、それらの記憶容量を拡張する工夫を考案した [Delta Net](https://arxiv.org/abs/2102.11174) 等も提案されています。

#### Queryのプロトタイプ化とKey-Valueのメモリ圧縮

上記のような工夫のほかに、計算すべきトークン数を直接減らすことにより計算量を削減するアプローチも存在します。Fig.7 はその概要です。

![](https://storage.googleapis.com/zenn-user-upload/84c5d0306b58-20231010.png)  
*Fig.7. 計算すべきトークン数を減らす工夫。 [A Survey of Transformers](https://arxiv.org/abs/2106.04554) より引用。*

プロトタイプを利用してQueryのトークン数を減らす手法としては、Queryを幾つかのクラスタに分類する [Clustered Attention](https://arxiv.org/abs/2007.04825) や、各QueryにおけるAttentionの分布と一様分布間のKLダイバージェンスを近似的に求めることで、情報量の多い一定数のQueryのみを計算に用いる [Informer](https://arxiv.org/abs/2012.07436) 等が提案されています。

また、Key-Valueペアを情報圧縮することでトークン数を減らす手法としては、ストライドの大きな畳み込みを噛ませる [Memory Compressed Attention](https://arxiv.org/abs/1801.10198) 、情報集約するための学習可能なグローバルトークンで一度入力を要約する [Set Transformer](https://arxiv.org/abs/1810.00825) や [Luna](https://arxiv.org/abs/2106.01540) 、線形写像によってKey-Valueペアをより小さいトークン数へ射影して用いる [Linformer](https://arxiv.org/abs/2006.04768) 、Fig.5 (b) に示したスパースなAttentionを計算しつつ、Memory Compressed Attentionを組み合わせる [Poolingformer](https://arxiv.org/abs/2105.04371) 等が提案されています。

#### Self-Attentionの低ランク化

経験的にAttention Matrixがスパース性を持つこととも関連しますが、Attention Matrixは大抵の場合に低ランク行列となることが報告されています。この性質は、 [Self-Attentionを低ランク行列で直接パラメータ化する手法](https://ieeexplore.ieee.org/document/8894858) 、Nyström近似に基づく [CSALR](https://www.ijcai.org/proceedings/2020/285) や [Nyströmformer](https://arxiv.org/abs/2102.03902) といった手法に活かされています。

#### 事前分布の利用

入力とは別の情報源、あるいは事前知識から事前分布となるAttention Matrixを用意して複合することで、性能向上や省パラメータ化を図る研究も存在します。例えば、データ系列の局所性に着目し、それぞれのトークン位置を中心にガウス分布状の事前分布を計算する [Gaussian Transformer](https://ojs.aaai.org/index.php/AAAI/article/view/4614) や、隣接する層のAttention Matrixは類似するという観測に基づき直前の層の計算結果を利用する [Predictive Attention Transformer](https://openreview.net/forum?id=YQVjbJPnPc9) や [Realformer](https://arxiv.org/abs/2012.11747) 等の手法です。後者のタイプの極端な例として、一度計算されたAttention Matrixを複数層で使い回す [Lazyformer](https://arxiv.org/abs/2102.12702) も提案されています。

さらに、タスクに応じて事前分布を適応的に計算する [CAMTL](https://arxiv.org/abs/2009.09139) や、入力系列とは完全に独立な事前分布のみをAttention Matrixとして用いる [Synthesizer](https://arxiv.org/abs/2005.00743) のような手法群も存在します。

#### Multi-Head Attentionの改善

Multi-Head Attentionは、異なるトークン位置にある複数の表現部分空間から同時に情報を集約できる点において魅力的ですが、実際に各Headがそれぞれ異なる特徴を捉えていることを保証する機構はありません。そこで例えば、 [異なるHead間の多様性を促進する制約項を損失関数に加える手法](https://aclanthology.org/D18-1317/) や、 [事前学習済みTransformerモデルに特徴的なAttentionパターンを促進する制約項を加えることで学習の収束を早めたり頑健性を向上させる手法](https://aclanthology.org/2020.findings-emnlp.419/) 、追加の線形射影や重み共有によりHead間で情報伝達が行われる経路を設けた [Talking-Heads Attention](https://arxiv.org/abs/2003.02436) や [Collaborative Multi-head Attention](https://arxiv.org/abs/2006.16362) 等が提案されています。

また、各トークン位置からの距離に応じてAttentionの計算間隔をHead単位で制御し、効率的に局所特徴に注意を向ける手法として、そうしたスパンを適応的に学習する [Adaptive Attention Span](https://doi.org/10.18653/v1/P19-1032) 、各層やHeadごとに固定のスパンを予め定める [Multi-Scale Transformer](https://arxiv.org/abs/1912.00544) も提案されています。さらに、複数Headの出力を単に再結合して線形射影する通常のTransformerの流れを再考し、Capsule Networksに着想を得た反復的な情報伝達経路選択過程を適用した手法（ [Dynamic Routingを適用した手法](https://doi.org/10.1007/978-3-030-32233-5_25) や [EM Routingを適用した手法](https://doi.org/10.18653/v1/N19-1359) ）も存在します。

その他、Queryのみ複数Headに分割し、Key-Valueは共通の重みで射映することでメモリ消費を抑える [Multi-Query Attention](https://arxiv.org/abs/1911.02150) や、特徴ベクトルをHead数に応じて分割する慣例を再考し、 [各Headの次元数を系列長に一致させることで性能向上を図った手法](http://proceedings.mlr.press/v119/bhojanapalli20a.html) 等、多種多様な工夫が行われています。

---

さらに、Attention以外の構成要素に着目した改修も簡単に紹介したいと思います。

## 位置表現

Transformerの構成要素であるAttention、および位置ごとに適用される線形層は、いずれも順列不変な関数です。したがって、自然言語処理や画像処理など、各トークンの位置が重要となるモダリティにおいては、いかにして入力の位置情報をモデルの処理に反映させるかが重要になります。

通常のTransformerでは、トークンの絶対位置と特徴ベクトルの各次元位置に応じて周波数の異なる三角関数を計算するヒューリスティックな位置符号化が用いられました。

#### 絶対位置の利用

絶対位置を利用する手法の系列としては、 [BERT](https://doi.org/10.18653/v1/N19-1423) のように位置埋め込みを学習可能にすることでより柔軟な位置表現を獲得できることが期待されますが、一方で予め定められた最大系列長以上を扱えないという欠点を内包することには注意が必要です。その改善として、 [位置符号化の周波数部分のみを学習可能にすることで三角関数による帰納バイアスを活かす手法](https://openreview.net/forum?id=onxoVA9FxMw) や、位置表現を連続化してNeural ODEを適用する [FLOATER](http://proceedings.mlr.press/v119/liu20n.html) のような手法も存在します。

さらに、通常のTransformerのように位置表現を最初にトークン埋め込みに加算するだけでは層を経るごとに情報が薄まるという考察から、 [Universal Transformers](https://openreview.net/forum?id=HyzdRiR9Y7) のように、Transformerの各層において位置表現を注入するような手法群が効果的であることも報告されています。

#### 相対位置の利用

位置を表現する他の方法としては、トークンの絶対位置ではなく、 [各トークンから他のそれぞれのトークンに対する相対位置を符号化する方法](https://doi.org/10.18653/v1/N18-2074) も考えられます。このような手法はより長い系列を扱う能力に長けていることが知られており、相対位置の表現や与え方には様々な派生が提案されています（e.g., [InDIGO](https://transacl.org/ojs/index.php/tacl/article/view/1732) 、 [Transformer-XL](https://doi.org/10.18653/v1/P19-1285) 、 [Music Transformer](https://openreview.net/forum?id=rJe4ShAcF7) 、 [DeBERTa](https://arxiv.org/abs/2006.03654) 、 [T5](https://arxiv.org/abs/1910.10683) ）。

その他、絶対位置表現とのハイブリッド手法である [TUPE](https://arxiv.org/abs/2006.15595) や、回転位置埋め込みを用いることで絶対位置と相対位置の双方の役割を果たす [RoFormer](https://arxiv.org/abs/2104.09864) も提案されています。RoFormerはAttentionの線形化とも互換性があることが知られています。

#### 明示的な位置表現の廃止

位置情報をそれ単体で埋め込む代わりに、 [トークン埋め込みを位置に関する複素連続関数として学習する手法](https://openreview.net/forum?id=Hke-WTVtwr) のように、モデル内の他の処理によって位置に関する情報を非明示的に獲得させる研究も存在します。他に、局所RNNを用いる [R-Transformer](https://arxiv.org/abs/1907.05572) や、ゼロパディングを含む二次元畳み込みは画像内位置を非明示的に符号化できるという洞察に基づく [CPE](https://arxiv.org/abs/2102.10882) が提案されています。

#### デコーダにより獲得される位置表現の活用

Attentionは順列不変であると述べましたが、Masked Self-Attentionについてはその限りではありません。実際、 [位置に関するマスクのみを適用し、位置符号化を取り除いたTransformerもチューリング完全である](https://arxiv.org/abs/2006.09286) ことが証明されています。とりわけ、言語モデルのようにデコーダのみを用いるタイプのモデルは、 [明示的な位置表現を与えずとも位置に関する情報を獲得しうる](https://doi.org/10.21437/Interspeech.2019-2225) ことが報告されているほか、 [絶対位置埋め込みを排除することがむしろ性能向上に寄与する場合がある](https://arxiv.org/abs/2102.11174) ことも示されています。

## 正規化層

#### 正規化層の適用位置

Fig.2 からもわかるように、通常のTransformerでは正規化層（LayerNorm）は各残差接続の後に配置されています。のちにこのタイプはPost-LNと呼ばれるようになりますが、Post-LNは層が深くなるほど学習が比較的不安定となることが知られています。

これに対し後続の研究では、正規化層を残差接続の中、特に各Attentionや線形層の直前に入れるPre-LNという方式が提案され、学習の安定性に寄与することが広く知られています。一般にPre-LNでは、最終出力のノルム規模を保つために、最終層の後に追加のLayerNormが追加されます。複数の理論的な研究により、Pre-LNでは勾配が安定し、Post-LNのような学習初期の大きな勾配を引き起こしにくいことがわかっています。一方、正常に学習を遂行することができれば、収束後の最終的な性能はPost-LNのほうが一般に勝ることも報告されています。

![](https://storage.googleapis.com/zenn-user-upload/25f8cf4976f0-20231010.png)  
*Fig.8. 正規化層の位置の比較。 [A Survey of Transformers](https://arxiv.org/abs/2106.04554) より引用。*

Post-LNが不安定な直接の原因は勾配の不均衡ではなく、 [残差接続への重度の依存関係が招く出力変動の増幅効果である](https://doi.org/10.18653/v1/2020.emnlp-main.463) とする研究もあります。例えば、パラメータに僅かな摂動を加えたり、学習により重みが更新されたりすると、Pre-LNに比べてPost-LNは桁違いの規模で出力が変動してしまうということを突き止めています。この研究では、追加パラメータと初期化の工夫によって増幅効果の緩和手法を提案し、安定性を担保しつつPost-LNの性能を向上させることに成功しています。

#### 正規化層の置換

オリジナルで採用されたLayerNormの妥当性を再考し、他の正規化層や新たなパラメータ化を提案する研究も存在します。例えば、LNを簡略化し性能を維持しつつ計算量を削減した [RMSNorm](https://arxiv.org/abs/1910.07467) 、LNの学習可能パラメータは性能にほぼ寄与しないばかりでなく過学習の恐れがあり、かつLNの重要性はその勾配にあるとの解析結果から提案された [AdaNorm](https://proceedings.neurips.cc/paper/2019/hash/2f4fe03d77724a7217006e5d16728874-Abstract.html) 、CNNsでは一般的なBatchNormをTransformerにて効果的に活用するための考察から提案された [PowerNorm](http://proceedings.mlr.press/v119/shen20e.html) 等があります。

#### 正規化層の撤廃

そもそも学習を安定化させるために正規化層以外の構造は考えられないのか、という発想から、単に残差接続に学習可能な係数を乗ずる [ReZero](https://arxiv.org/abs/2003.04887) という手法も提案されています。この係数はゼロで初期化されるため、学習初期のモデルはほぼ恒等写像となり学習の安定化が見込めます。

## 位置ごとに適用される線形層

線形層は単純な構成要素ですが、 [単にAttentionのみを積層するだけではモデル性能が低下する](https://arxiv.org/abs/2103.03404) ことが報告されているように、Transformerにおける重要な要素のひとつとなっています。

#### 活性化関数の変更

通常のTransformerでは最も単純な活性化関数のひとつであるReLUが用いられていますが、この部分を代替する関数として、ReLUと類似した形状でありながら滑らかな勾配を持つ [Swish](https://openreview.net/forum?id=Hkuq2EkPf) や [GeLU](https://arxiv.org/abs/1606.08415) 、ゲート処理を導入した [GLU](http://proceedings.mlr.press/v70/dauphin17a.html) 等が提案されています。

#### 適応的な手法によるモデル容量の拡張

線形層を他の構成要素に置換する工夫も存在します。有名な手法としては、入力をQueryに変換した上で、モデルパラメータとして学習される広域的なKey-Valueペアから情報を集約して出力する [Product-Key Memory](https://proceedings.neurips.cc/paper/2019/hash/9d8df73a3cfbf3c5b47bc9b50f214aff-Abstract.html) が知られています。Attentionとも類似していますが、実際にMulti-Head化による恩恵も示されています。一方で、Key-Valueペアが入力由来ではなく外部辞書的な扱いであることや、低次元ベクトルの直積としてKeyを表現することで多様性を確保する工夫、k近傍法を用いてQueryとKeyの類似性計算を効率化する工夫などに差異が見られます。Product-Key Memoryはモデル全体の計算量増加を無視できる程度に抑制しつつ、顕著な性能向上を達成しています。

![](https://storage.googleapis.com/zenn-user-upload/c9190b93af96-20231010.png)  
*Fig.9. Product-Key Memoryの概要。 [Large Memory Layers with Product Keys](https://proceedings.neurips.cc/paper/2019/hash/9d8df73a3cfbf3c5b47bc9b50f214aff-Abstract.html) より引用。*

さらに、線形層を複数用意し、その複合によって表現容量を拡張するMoE（Mixture of Experts）系の手法も複数提案されています。各層で並列に用意されたひとつひとつの線形層がExpertと見做されるイメージです。この文脈では、学習可能な関数によりトークンごとに上位k個のExpertを選択し、その出力の加重平均を求める [GShard](https://arxiv.org/abs/2006.16668) や、より極端に常にひとつのExpertのみが選択されるようにした [Switch Transformer](https://arxiv.org/abs/2101.03961) 等が提案されています。

## アーキテクチャレベルの改修

個別の構成要素を改善するだけでなく、モデルアーキテクチャにおける層の位置や意義さえ再考するような研究も盛んに取り組まれています。本章ではその例を概観します。

## モデル軽量化

モデルの軽量化は一般に広く関心を集める事柄ですが、先に紹介した工夫だけでなく、アーキテクチャレベルでの取り組みも提案されています。 [Lite Transformer](https://openreview.net/forum?id=ByeMPlHKPH) では、通常のAttention機構を広域的文脈を捉えるAttention、および局所特徴を捉える畳み込みと線形層に分離した構造で置換することを提案し、軽量化とともに翻訳タスクでの性能向上を実現しました。

他に、エンコーダを漏斗形にして系列長を絞り込む [Funnel Transformer](https://proceedings.neurips.cc/paper/2020/hash/2cd2915e69546904e4e5d4a2ac9e1652-Abstract.html) や、3つの独自モジュールから成る構成ブロックで通常のTransformerブロックを代替した [DeLighT](https://arxiv.org/abs/2008.00623) 等が提案されています。

## 適応的な演算回数

入力に応じて関数評価回数を変更することで、易しい入力に対しては計算量を低減し、難しい入力にはより多くの計算量を割く [ACT（Adaptive Computation Time）](https://arxiv.org/abs/1603.08983) という仕組みを搭載した手法群も存在します。例えば、トークンごとに層の適用回数を動的に決定する [Universal Transformers](https://openreview.net/forum?id=HyzdRiR9Y7) や、各層で計算をスキップするか否かを判断する [Conditional Computation Transformer](https://arxiv.org/abs/2002.07106) が知られています。また、全ての層を経る前に途中で計算を完了するような手法として、 [DeeBERT](https://doi.org/10.18653/v1/2020.acl-main.204) や [PABEE](https://arxiv.org/abs/2006.04152) が提案されています。

## 分割統治法

先述のように通常のAttentionは系列長の2乗に比例する計算量を要するため、長い系列を扱う場合に困難が生じます。これを解決する一案として、入力系列をいくつかの小さな領域に分割し、それらを個別に処理しながら適切に文脈情報を集約するという方法が考えられます。主な方式はFig.10 に示すように2種類あり、各セグメントを順に処理しつつ文脈情報をキャッシュして伝達していく再帰的方式と、各セグメントの処理結果をさらにまとめて処理する階層的方式に分けられます。

![](https://storage.googleapis.com/zenn-user-upload/e1ac1af441cc-20231010.png)  
*Fig.10. 入力系列を分割して処理する方式の概要。 [A Survey of Transformers](https://arxiv.org/abs/2106.04554) より引用。*

再帰的方式の代表的な例には [Transformer-XL](https://doi.org/10.18653/v1/P19-1285) や [Compressive Transformer](https://openreview.net/forum?id=SylKikSYDH) 、 [Memformer](https://arxiv.org/abs/2010.06891) があります。また、 [学習済みモデルに再帰性を付与するファインチューニング手法](https://arxiv.org/abs/2008.07027) も提案されています。

階層的方式の代表例としては [HIBERT](https://doi.org/10.18653/v1/P19-1499) や [Hi-Transformer](https://arxiv.org/abs/2106.01040) 、 [TENER](https://arxiv.org/abs/1911.04474) が挙げられます。また、画像領域のTransformerであるViTに特化した階層的方式として [Transformer in Transformer (TNT)](https://arxiv.org/abs/2103.00112) も提案されています。

## 代替的なモデル構造

Transformerの基本構造よりも最適な構造を模索するような研究も数多く展開されています。例えば、線形層とAttentionの位置関係を変更するようなアーキテクチャ改変としては、Transformerの処理を対流拡散方程式に倣う常微分方程式（ODE）と見做した上で、各Attentionを2つの線形層で挟む構造を採用した [Macaron Net](https://openreview.net/forum?id=SJl1o2NFwS) や、浅い層ほどAttentionを、深い層ほど線形層を集中させるよう構成要素を並び替えた [Sandwich Transformer](https://doi.org/10.18653/v1/2020.acl-main.270) が挙げられます。また、Self-Attentionにおけるマスクを入力系列から動的に求めて局所性を促進することで性能向上を図る [Mask Attention Network](https://www.aclweb.org/anthology/2021.naacl-main.135) も提案されています。

他に、2019年頃から急激に白熱したニューラルネットワークの自動構造探索（NAS）の流れで獲得された [Evolved Transformer](http://proceedings.mlr.press/v97/so19a.html) や [DARTSformer](https://arxiv.org/abs/2105.14669) も、人手での設計とは異なる観点からTransformerの構造に示唆を与える興味深い研究です。

さらに近年では、ともすれば神聖視されがちなAttention機構がTransformerの中で果たす役割を疑問視し、より単純な構成要素で置き換える手法も提案されてきています。特にこの流れは画像分野に顕著で、チャネル方向と空間方向に線形層を適用するだけの構造でAttentionを置き換えることにより、スループットを大幅に改善しつつSOTA研究に並ぶ画像分類精度を実現した [MLP-Mixer](https://arxiv.org/abs/2105.01601) が火付け役となり、空間方向にゲート処理を追加した [gMLP](https://arxiv.org/abs/2105.08050) へと続きました。

![](https://storage.googleapis.com/zenn-user-upload/a506444c0c4a-20231010.png)  
*Fig.11. MLP-Mixerの概要。 [MLP-Mixer: An all-MLP Architecture for Vision](https://arxiv.org/abs/2105.01601) より引用。*

こうした研究の示唆を敷衍し、『 **Transformerの高性能さはAttentionではなく、Transformerの構造それ自体に依存している** 』と主張する [MetaFormer](https://arxiv.org/abs/2111.11418) では、プーリング層という『恥ずかしいほどに』単純な構成要素でAttentionを置き換えたPoolFormerにより、計算効率と画像分類精度のトレードオフが大幅に改善できることを示しました。すなわち、トークン情報を何らかの方法で混合する構造自体が重要なのであり、Attentionはその一実現に過ぎないという提起でした。

![](https://storage.googleapis.com/zenn-user-upload/2fc9a01755c1-20231010.png)  
*Fig.12. (a) MetaFormerの概念図とその実装例。 (b) 積和演算回数と画像分類精度のトレードオフ。 [MetaFormer Is Actually What You Need for Vision](https://arxiv.org/abs/2111.11418) より引用。*

[HyperMixer](https://arxiv.org/abs/2203.03691) では特に自然言語処理においてこの流れを加速させています。入力系列から動的にトークンを混合する線形層を定義するHyperMixerは、Attentionのように位置不変性や可変長系列への対応性を有します。一方で計算量は系列長に比例するため、2乗に比例するAttentionよりも一般に軽量です。GLUEベンチマークにおいて、他のMLP系手法を上回り、また通常のTransformerと同等かそれ以上の性能を実現しました。HyperMixerはAttentionよりも単純な構造であるが故、データセット規模が小さいほど相対的な性能向上が顕著であることも示されています。

![](https://storage.googleapis.com/zenn-user-upload/d3e2429653dd-20231010.png)  
*Fig.13. HyperMixerの概要。 [HyperMixer: An MLP-based Green AI Alternative to Transformers](https://arxiv.org/abs/2203.03691) より引用。*

## ところでこれらの改修は本当に意味があるの？

ここまで非常に幅広い改修のための研究を紹介しましたが、これらがどれほど汎用的に使える手法なのかは気になるところです。特に応用レベルではオリジナルの構造がほぼ改変なく用いられることも多く、それぞれの変種が実際にどれほど有用なのかには疑問が残ります。こうした問いに、Google Researchによる研究チームが [2021年時点で一定の網羅的な回答を与えています](https://arxiv.org/abs/2102.11972) 。ただし、 **本記事で紹介した全ての手法が比較されているわけではない点と、性能比較のためのタスクが自然言語処理に限定されている点には留意してください** 。具体的にどのような手法が比較されたかについては、ぜひ原典をご覧ください。

この研究では、ハイパーパラメータやデータセット等の学習条件を揃えた上で各手法を適用した [T5](https://arxiv.org/abs/1910.10683) モデルにおいて、事前学習からのSuperGLUE、XSum、WebQuestionsといった転移タスクでの精度、およびWMT'14翻訳タスクでの性能を比較検証しました。

結果としては、GLUやその派生関数が一貫して有効であることが示されたほか、RMSNorm、層数の深化、デコーダの入出力埋め込みの共有に一定の効果があることが確認されました。また、アーキテクチャレベルの改修では、Product-Key Memory、Switch Transformer、Synthesizer系列による改善が見られました。一方で、その他の多くの手法については、オリジナルのTransformerからの優位性はほとんど見られないという結論が示されました。

著者らはこの結果について、『 **オリジナルで提案された構造が極めて完成度の高いものであり、改善の余地がほとんどない可能性** 』を挙げています。また、他の可能な説明として、『 **Transformerのために提案された改修手法は多くの場合限定的な効果しか持たず、応用先を超えて汎化しないのではないか** 』とも考察しています。

## 事前学習手法と応用領域

大規模なデータセットで事前学習したTransformerモデルを下流タスクでファインチューニングする方式の有効性は、数々の研究により実証されています。Transformerを用いた自然言語処理における初期研究である [BERT](https://doi.org/10.18653/v1/N19-1423) の顕著な有効性が確認されて以降、事前学習を前提とするモデルが次々に提案されてきました。十分なデータ規模と計算量のもとで学習された事前学習モデルは高い汎化性能を獲得することで知られ、近年ではとりわけ基盤モデルと呼称されることもあります。こうしたモデルの発展が、Transformerを様々なモダリティへ展開させる大きな基礎を形作りました。

2021年時点での事前学習モデルの体系は、 [Pre-trained Models for Natural Language Processing: A Survey](https://arxiv.org/abs/2003.08271) によくまとまっています。

![](https://storage.googleapis.com/zenn-user-upload/9bc3829851dc-20231010.png)  
*Fig.14. 事前学習手法の概要。 [Pre-trained Models for Natural Language Processing: A Survey](https://arxiv.org/abs/2003.08271) より引用。*

## アーキテクチャレベルの分類

本記事では主にアーキテクチャレベルの差異に着目して、簡単にこのトピックの概要を掴もうと思います。個別の手法の詳細は、ぜひ Fig.14 中の各原著論文をご覧ください。

#### エンコーダのみ

このカテゴリの代表例は先述の [BERT](https://doi.org/10.18653/v1/N19-1423) です。BERTは教師なし表現学習手法で、入力系列の一部をランダムで欠落させてその部分を予測するタスク（MLM）、および2種類の結合された文章が連続するものか否かを予測するタスク（NSP）を同時に解くことで、ラベルのない大量のテキストコーパスから汎用的な自然言語表現能力が獲得できることを示し、事前学習の流れの開拓に大きな寄与を果たしました。

さらに [RoBERTa](https://arxiv.org/abs/1907.11692) ではBERTの性能向上に寄与する要因を調査し、データ規模拡大、学習時間延長、動的なMLMやNSPの撤廃などを提案しました。 [BERT派生研究は2020年時点でも大量に現れ](https://kai760.medium.com/nlp-2020%E5%B9%B4%E3%81%AB%E7%94%9F%E3%81%BE%E3%82%8C%E3%81%9Fbert%E3%81%AE%E6%B4%BE%E7%94%9F%E5%BD%A2%E3%81%BE%E3%81%A8%E3%82%81-36f2f455919d) 、以降、上位互換と呼べる他手法が台頭するまでの間、洪水のように提案され続けました。

他に、本記事で紹介したBigBirdやSwitch Transformerもこの括りで捉えることができます。

#### デコーダのみ

入出力が同一のモダリティであれば、あるいは、異なる言語や異なるモダリティを統合して単一のトークン集合にまとめて扱ってしまえば、エンコーダは要らず、デコーダのみでTransformerを運用することができます。そのような言語モデルで最も有名なのは先述のGPT系列（ [GPT](https://gwern.net/doc/www/s3-us-west-2.amazonaws.com/d73fdc5ffa8627bce44dcda2fc012da638ffb158.pdf) 、 [GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) 、 [GPT-3](https://arxiv.org/abs/2005.14165) ）でしょう。これらのモデルの驚くべき点は、莫大な言語データを取り込むことで獲得されたFew-shot性能、あるいはZero-shot性能にあるといえます。

特にGPT-3以降のモデルでは、いわゆる『プロンプトエンジニアリング』と呼ばれる工夫によってFew-shot性能が顕著に向上しています。このような枠組みでは **モデルに個別のタスクについて学習させる必要はなく** 、入力プロンプトにタスクの説明や幾らかの例を示した上で問いを投げかけると、それに適した回答を出力文として返してくれます。あたかも人が他者の簡潔な説明を理解し、内容を類型化して答えを述べるかのような振る舞いです。このような大規模モデルによる転移性能は、後続の研究でさらに拡大していくこととなります。Google系列により発表された [LaMDA](https://arxiv.org/abs/2201.08239) や [PaLM](https://arxiv.org/abs/2204.02311) もこのカテゴリに属します。

#### エンコーダ・デコーダ方式

オリジナルの形状を継承した手法としては、BERTの拡張である [BART](https://doi.org/10.18653/v1/2020.acl-main.703) や、テキストからテキストへの転移学習として統一的に様々な言語タスクを学習する [T5](https://arxiv.org/abs/1910.10683) がよく知られています。特にT5は、タスクに応じた接頭辞をプロンプトに付け加える方式を提案した初期の研究としても有名です。これらの手法はエンコーダ部を持つため、入力に対して得られた表現特徴を他タスクへ流用する手法へも発展しています。

## 大規模言語モデルに見られる創発性

GPT-3やLaMDA、PaLMなど、一定規模を超えた大規模言語モデルには、ある種の『 **創発性** 』が発現することが観測されています。創発性とは、小さいモデル規模ではランダムな水準の精度しか達成できなかったタスクにおいて、ある閾値を境に不連続的にモデルがタスクに適応し、精度向上を実現する現象のことを指します。

![](https://storage.googleapis.com/zenn-user-upload/cbb7e69daea7-20231010.png)  
*Fig.15. GPT-3では、パラメータ数が13B以降のモデルで四則演算タスクの飛躍的な精度向上が見られた。創発性が報告された初期の例。 [Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165) より引用。*

#### 思考連鎖

特に、 [Chain-of-Thought](https://arxiv.org/abs/2201.11903) （思考連鎖）と呼ばれるタイプのプロンプトは、創発性が見られる典型的な例であるとされています。思考連鎖では、推論を段階的な思考を経て行うことをモデルに直接促したり、そのような段階的思考を例示するプロンプトを与えることにより、算術推論や常識推論、記号操作など、その背景にある論理の流れを適切に把握しなければ回答できない類のタスク性能が多くの場合で劇的に改善されることが確認されています。また、 [モデル出力が自己一貫性を持つようサンプリングすることで、思考連鎖による推論がさらに改善する](https://arxiv.org/abs/2203.11171) ことも示されています。

[Emergent Abilities of Large Language Models](https://arxiv.org/abs/2206.07682) では、上述したようなモデル群を総浮動小数点演算数（FLOPs）やパラメータ数の観点から調査し、様々なタスクにおいて創発性が現れる閾値を特定することを試みました。

![](https://storage.googleapis.com/zenn-user-upload/0e037fbc773d-20231010.png)  
*Fig.16. BIG-Benchに含まれる膨大な種類のタスクにおいて、創発性が見られたか、単に計算量に応じて連続的に精度向上するタスクであったか、またはランダム予測を超える精度を実現するモデルがなかったかを集計した結果。 [Emergent Abilities of Large Language Models](https://arxiv.org/abs/2206.07682) より引用。*

![](https://storage.googleapis.com/zenn-user-upload/08a052d78d40-20231010.png)  
*Fig.17. あるFLOPsを境にランダム予測水準を抜けて精度改善するタスクの例。 [Emergent Abilities of Large Language Models](https://arxiv.org/abs/2206.07682) より引用。*

こうした挙動は、『系における量の変化が振る舞いの質に影響する』という相転移の性質を彷彿とさせます。

[A Closer Look at Large Language Models Emergent Abilities](https://yaofu.notion.site/A-Closer-Look-at-Large-Language-Models-Emergent-Abilities-493876b55df5479d80686f68a1abd72f) では、このような創発性が見られる条件や傾向について、2022年11月末時点で読み取れる潜在的な説明を整理しています。特に、一般に公開されている大規模言語モデルの中で、思考連鎖を含む強力な創発性が見られるGPT-3の2種類のモデルカード（text-davinci-002とcode-davinci-002）を例に挙げ、 **プログラミングのコードを学習データに含むことが創発性に大きく寄与しているのではないか** とする考察は非常に興味深いものです（ただし、当該記事が執筆された直後にtext-davinci-003がOpenAIより公開され、そちらとの性能比較は為されていないことには注意が必要です）。こうした仮説は、Tab.1 に見られるように、code-davinci-002が一貫してtext-davinci-002よりも高性能であることや、対話データやWEB文書から学習されたLaMDAが、コードをデータに含むcode-davinci-002やPaLMに大きく劣る結果を与えていることからも支持されます。

![](https://storage.googleapis.com/zenn-user-upload/5434f73eb5d0-20231010.png)  
*Tab.1. 5つの算術推論ベンチマークにて、思考連鎖を適用した様々な大規模言語モデルが標準プロンプトの精度を上回った。 [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903) より引用。*

記事中では『表面上はコードと自然言語はほとんど関係がなく、有用である理由は不明』であると述べられていますが、定められた規則のもとで整然と理論展開するコードの特性や記号列の配置を学習することが思考連鎖に寄与するという仮説は、直感的にはかなり納得できるものなのではないかとも思えます。

一方で、このような思考連鎖においては、答えに辿り着く過程と答えが必ずしも一致しない場合が発生しうるという課題も確認されています。これに対し2023年2月初旬には、プロンプトを用いた推論タスクを次の2段階、すなわち『言語モデルにより自然言語入力を記号推論連鎖へ翻訳する段階』と『決定的ソルバにより推論連鎖から回答導出する段階』に分解する『 [忠実な思考連鎖](https://arxiv.org/abs/2301.13379) 』も提案されました。この方式では自然言語をまさにコードのような形式に翻訳させることで性能向上することが示されており、先述の見解と綺麗に接続する点が非常に興味深いです。

![](https://storage.googleapis.com/zenn-user-upload/367d1c9fbbb7-20231010.png)  
*Fig.18. 忠実な思考連鎖の概要。算数の文章題やマルチホップQA、論理的推論等、推論タスクに応じて適切な記号操作方式へ翻訳し、それに基づいて回答させる。 [Faithful Chain-of-Thought Reasoning](https://arxiv.org/abs/2301.13379) より引用。*

![](https://storage.googleapis.com/zenn-user-upload/3b2e71f7f6e3-20231010.png)  
*Fig.19. 算数の文章題データセット（GSM8K）におけるプロンプトの提示例。忠実な思考連鎖では、問題文の例示において記号操作への置換を陽に示すことで、言語モデルの出力がそれに倣い入力問題を翻訳することを促す。これにより、出力の推論の流れと回答が一致しつつ正しい回答が得られる可能性を高める。 [Faithful Chain-of-Thought Reasoning](https://arxiv.org/abs/2301.13379) より引用。*

#### 強化学習との併用

さて、再び [A Closer Look at Large Language Models Emergent Abilities](https://yaofu.notion.site/A-Closer-Look-at-Large-Language-Models-Emergent-Abilities-493876b55df5479d80686f68a1abd72f) に戻ると、当該記事ではさらに、創発性が生まれる要因のひとつとして『 [人のフィードバックに基づく指示ベースの強化学習](https://arxiv.org/abs/2203.02155) 』が寄与している可能性を挙げています。この研究ではいわゆる [RLHF](https://arxiv.org/abs/1706.03741) と呼ばれる方式が応用され、人手による望ましい応答例や好みに基づくランク付けを反映して、言語モデルの出力をより自然で安全なものに改良しています。結果、GPT-3の拡張であるInstructGPTが提案されました。

![](https://storage.googleapis.com/zenn-user-upload/f476b20aaeda-20231010.png)  
*Fig.20. InstructGPTの概要。教師ありファインチューニング、報酬モデルの学習、PPOに基づく強化学習の3段階から構成される。 [Training language models to follow instructions with human feedback](https://arxiv.org/abs/2203.02155) より引用。*

2022年末に突如としてOpenAIからオープンβが開始されていた [ChatGPT](https://openai.com/blog/chatgpt/) は、InstructGPTをさらに対話に特化させたモデルであるとされています。その応答の自然性や汎用性、創作やロールプレイ能力（あるいは、ある種の『それらしい嘘』を吐く能力）は使用者が増加するにつれて爆発的に解明され、年の瀬に一躍話題になったことは記憶に新しいですね。

ChatGPTの活用例は非常に幅広く、登場して数ヶ月で有志による解説記事や解説動画が無数に展開されていることからも、その尋常ではない影響力が窺い知れます。とりわけ、コンプライアンス意識が高く安全なAIであるように設計されたChatGPTに非倫理的な応答も許容するようハックする『脱獄』手法も日進月歩で開発されるなど、ユーザー母数の拡大に伴ってまだ見ぬ特性が明かされていく様子にリアルタイムで立ち会えることには静かな興奮を覚えます。

ChatGPTの台頭で検索エンジンが脅かされるとの言説も見られましたが、実際OpenAIと提携するMicrosoftは2023年2月初旬、 [ChatGPTで得られた知見をもとに開発されたと見られるPrometheusをBingに搭載し、新たな検索体験を提供しはじめました](https://www.bing.com/new?form=MY0291&OCID=MY0291) 。競合であるGoogleも [LaMDAを継承した対話モデルであるBardを公開予定である](https://japan.googleblog.com/2023/02/ai.html) としており、開発競争は熾烈を極めています。

#### 微小な追加計算量による大規模言語モデルの性能向上

創発性に関する話題において現時点で触れるべき研究としては、他に [UL2R](https://arxiv.org/abs/2210.11399) および [指示ベースのファインチューニング](https://arxiv.org/abs/2210.11416) が挙げられます。UL2Rは [UL2](https://arxiv.org/abs/2205.05131) と呼ばれる研究の改良手法で、相対的に少ない計算量の追加で言語モデルのスケーリング則をさらに推し進めることに成功しました。

[UL2](https://arxiv.org/abs/2205.05131) では、言語モデルの事前学習における従来の訓練方法である自己回帰型生成や破損箇所推定タスクを統一化したフレームワークを提案し、入力文章の欠損箇所を予測するノイズ除去を3つの異なる方法で行う混合ノイズ除去器として言語モデルを学習することを提案します。

![](https://storage.googleapis.com/zenn-user-upload/eeeb9086454c-20231010.png)  
*Fig.21. UL2の概要。図中でグレーアウトされた部分をマスクした入力を与え、これらを特殊トークンで連結したものが予測対象となる。短スパン・低破損のR-Denoising、文章を丸ごと欠損させることで自己回帰生成やプレフィックス学習に対応させたS-Denoising、より破損水準が挑戦的なX-Denoisingから成る。 [UL2: Unifying Language Learning Paradigms](https://arxiv.org/abs/2205.05131) より引用。*

[UL2R](https://arxiv.org/abs/2210.11399) では、PaLM等の事前学習済みモデルの学習をさらにUL2を用いて継続することを提案し、無視できるほどの追加計算量で劇的な性能向上が実現できることを示しました。またその結果、パラメータ数540BのPaLMに対し、UL2Rを適用した僅か64BのU-PaLMが性能を凌駕するタスクも確認されたり、PaLMでは創発性が見られなかったタスクで創発性が観測されたりするなど、総じてUL2Rの有効性が確かめられました。

![](https://storage.googleapis.com/zenn-user-upload/578f5c701c7c-20231010.png)  
*Fig.22. UL2Rの効果例。各PaLMの事前学習済みチェックポイント（ ■ ）に対し、僅か0.1%程度の追加学習をUL2を用いて行うことで、点線で繋いだような劇的な性能向上（ ★ ）を果たす。いずれも、PaLMをそのまま継続学習していた場合に到達していた性能と同等の水準にその数割の総計算量で到達できている。 [Transcending Scaling Laws with 0.1% Extra Compute](https://arxiv.org/abs/2210.11399) より引用。*

さらに [指示ベースのファインチューニング](https://arxiv.org/abs/2210.11416) では、指示形式や思考連鎖の形式で記された1800以上のタスクにて事前学習済み言語モデルをファインチューニングすることで、やはり0.2%程度の僅かな追加計算量によって総合的な性能向上を実現しています（とはいえ、540BのPaLMの事前学習では莫大な計算量を必要とするので、その0.2%でも依然として相当量ではありますが）。筆者らはこの手法により追加学習されたモデルにFlanの接頭辞をつけることで区別しています。

![](https://storage.googleapis.com/zenn-user-upload/52bd67f4b3ff-20231010.png)  
*Fig.23. Flanモデルの概要。 [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416) より引用。*

Flanモデルの優位性は単なる性能向上に留まりません。未知タスクへの汎化性能が上がるほか、これまでの定番のプロンプトエンジニアリングのように入力に少数の解答例を含めずとも、単なる質問文のみから適切に出力してくれるZero-shot性能も向上しています。PaLMほどの大規模モデルでも、Zero-shotではFew-shotよりも生成品質は落ち、質問に答えずに質問に関連するテキストを生成し続けたり、質問文をただ言い換えるだけだったり、生成が止めどなく溢れたりといった課題点が知られていました。一方、Flan-PaLMではこれらの課題がより緩和されており、よりユーザーフレンドリーな言語モデルに近づきました。

![](https://storage.googleapis.com/zenn-user-upload/f60349e9f889-20231010.png)  
*Fig.24. Flan-PaLMではZero-shot入力へより適切に回答できている。『2人のAI研究者がデートに行くことを表す言葉を作りなさい』という入力に対して、デートとデータマイニングを掛けて『デートマイニング』と回答しているユーモアはポイントが高い。 [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416) より引用。*

また、これらの工夫を組み合わせたFlan-U-PaLMが、それぞれを単体で適用した場合よりも高性能であることも示され、両手法が互いに相補的な効果を持つことがわかりました。大規模言語モデルが今後も巨大化を続けていくことが予想される中、投入計算量に対して見返りの大きいこうした技術の重要性はますます高まっていくものと考えられます。

## 言語以外の応用領域

もとは自然言語処理分野にて提案されたTransformerですが、現在ではその他の分野でもデファクトスタンダードの座を恣にしつつあります。本章では最後に、言語と掛け合わせたマルチモーダル手法も含め、他モダリティでの代表的な研究を簡単に紹介します。

## 画像処理分野

画像分類、物体検出、画像生成、動画処理など、幅広く応用されています。特に貢献が大きいのは、画像を小領域でパッチ化しTransformerに与える構造を普及させた [ViT](https://arxiv.org/abs/2010.11929) でしょう。ViTは様々な派生を生み出しましたが、特にピラミッド型の構造を採用した [Swin Transformer](https://arxiv.org/abs/2103.14030) では計算量や解像度に起因するViTの課題を解決することを試みています。さらに、動画に適用する手法として [ViViT](https://arxiv.org/abs/2103.15691) へと発展していきました。

ViTにおいては、2023年2月中旬、 [新たな効率化の工夫の上で言語モデルと同様にモデルをスケールアップさせることにより、画像領域におけるZero-shot性能の向上を達成する研究](https://arxiv.org/abs/2302.05442) が登場しました。この時点でのパラメータ数は22Bと、540BのPaLM等のスケールにはまだ一歩及ばぬ規模感ですが、今後の更なる発展により自然言語処理以外での創発的能力が明らかにされていく日も遠くないかもしれません。

関連手法は [A Survey on Vision Transformer](https://arxiv.org/abs/2012.12556) や [Transformers in Vision: A Survey](https://arxiv.org/abs/2101.01169) に詳しいほか、日本語資料では [cvpaper.challengeによるメタサーベイ](https://www.slideshare.net/cvpaperchallenge/transformer-247407256) も非常に参考になります。また、ViTを含めた医療分野への展開などは [Transforming medical imaging with Transformers? A comparative review of key properties, current progresses, and future perspectives](https://arxiv.org/abs/2206.01136v1) によくまとまっています。

言語との結びつきとしては、言語と画像を共通の潜在空間に埋め込む基盤モデルである [CLIP](https://arxiv.org/abs/2103.00020) の貢献は特に大きいでしょう。CLIPの成功を受け、言語と画像を繋ぐ基盤モデルは [Florence](https://arxiv.org/abs/2111.11432) 、 [BLIP](https://arxiv.org/abs/2201.12086v2) 、 [BLIP-2](https://arxiv.org/abs/2301.12597v1) へと続いています。また、テキストからの画像生成において画像生成部にTransformerを用いる系譜も盛んに研究されており、界隈の先駆的研究である [DALL·E](https://arxiv.org/abs/2102.12092) をはじめ、 [CogView](https://arxiv.org/abs/2105.13290v2) 、 [ERNIE-ViLG](https://arxiv.org/abs/2112.15283) 、 [Make-A-Scene](https://arxiv.org/abs/2203.13131) 、 [CogView2](https://arxiv.org/abs/2204.14217) 、 [Parti](https://arxiv.org/abs/2206.10789) 、そして [Muse](https://arxiv.org/abs/2301.00704) へと続いていきます。さらに、テキストからの動画生成として、 [CogVideo](https://arxiv.org/abs/2205.15868) や [Phenaki](https://arxiv.org/abs/2210.02399) も提案されています。

近年の画像生成トレンドは専ら拡散モデルに集中していますが、PartiやMuseのように、現在でもTransformerベースの手法が現役で提案され続けている点にはある種の構図が見え隠れして面白いです。一方で、拡散モデルのコア部分にViTインスパイアのTransformer構造を採用した [DiT](https://arxiv.org/abs/2212.09748) も提案されるなど、ますます各方面へ浸透している様子はさらなる期待感を煽ります。

## 音声分野

音声認識、音声合成、音声強調、音楽生成など、やはり幅広いタスクでTransformerが採用されています。音声はそれ自体が一次元の系列であるほか、音声認識や合成など、入出力に言語情報を自然に含むタスクも多く、相性が良いといえるでしょう。特に近年では、大量の音声データを用いた教師なし学習により汎用的な表現特徴を獲得する [wav2vec 2.0](https://arxiv.org/abs/2006.11477) や [HuBERT](https://arxiv.org/abs/2106.07447) 等の手法も提案されており、音声認識や音声感情認識等の下流タスクで良好な性能を収めています。一方直近では、68万時間にも及ぶ膨大な量の書き起こしコーパスを用いた教師あり学習により、音声認識にて劇的な精度改善を実現した [Whisper](https://arxiv.org/abs/2212.04356) も話題となりました。

音楽生成の初期研究としては、音楽特有の長い系列の扱いやメロディ展開における文脈の反映に取り組んだ [Music Transformer](https://arxiv.org/abs/1809.04281) が後続研究に影響を与えています。2023年には、音の言語モデルともいえる [AudioLM](https://arxiv.org/abs/2209.03143) 、および自然言語と音声を潜在空間で結びつける [MuLan](https://arxiv.org/abs/2208.12415) の2手法に基づく [MusicLM](https://arxiv.org/abs/2301.11325) が発表され、 [テキストからの高品質な音響・音楽生成が実現されました](https://google-research.github.io/seanet/musiclm/examples/) 。

## 結び

躍進を続けるTransformerの系譜について、本記事ではモデルアーキテクチャの改修、事前学習手法、および応用例の観点から多角的に流れを追いました。本記事が、乱立する手法群を俯瞰したり、『名前は聞いたことある研究だけどどんな立ち位置だったかよくわからない』といったときに立ち返ることのできる辞書的な場所になれば望外の喜びです。

当初のタイトルが『15分で完全理解する〜』だったのに、執筆につれてどんどん時間が伸びたのは秘密です。ここまでご覧いただいた皆様に、心よりの感謝を。

## 参考文献

- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) \[Vaswani et al., NeurIPS, 2017\]
- [A Survey of Transformers](https://arxiv.org/abs/2106.04554) \[Lin et al., AI Open, 2022\]
- [Revealing the Dark Secrets of BERT](https://arxiv.org/abs/1908.08593) \[Kovaleva et al., EMNLP-IJCNLP, 2019\]
- [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) \[Kaplan et al., 2020\]
- [Scaling Laws for Autoregressive Generative Modeling](https://arxiv.org/abs/2010.14701) \[Henighan et al., 2020\]
- [Language Models are Few-Shot Learners](https://arxiv.org/abs/2005.14165) \[Brown et al., NeurIPS, 2020\]
- [Training Compute-Optimal Large Language Models](https://arxiv.org/abs/2203.15556) \[Hoffmann et al., 2022\]
- [Efficient Transformers: A Survey](https://arxiv.org/abs/2009.06732) \[Tay et al., ACM Computing Surveys, 2022\]
- [Big Bird: Transformers for Longer Sequences](https://arxiv.org/abs/2007.14062) \[Zaheer et al., NeurIPS, 2020\]
- [BP-Transformer: Modelling Long-Range Context via Binary Partitioning](https://arxiv.org/abs/1911.04070) \[Ye et al., 2019\]
- [Image Transformer](https://arxiv.org/abs/1802.05751) \[Parmar et al., ICML, 2018\]
- [Axial Attention in Multidimensional Transformers](https://arxiv.org/abs/1912.12180) \[Ho et al., 2019\]
- [Efficient Content-Based Sparse Attention with Routing Transformers](https://arxiv.org/abs/2003.05997) \[Roy et al., TACL, 2021\]
- [Reformer: The Efficient Transformer](https://arxiv.org/abs/2001.04451) \[Kitaev et al., 2020\]
- [SAC: Accelerating and Structuring Self-Attention via Sparse Adaptive Connection](https://arxiv.org/abs/2003.09833) \[Li et al., NeurIPS, 2020\]
- [Sparse Sinkhorn Attention](https://arxiv.org/abs/2002.11296) \[Tay et al., ICML, 2020\]
- [Transformers are RNNs: Fast Autoregressive Transformers with Linear Attention](https://arxiv.org/abs/2006.16236) \[Katharopoulos et al., ICML, 2020\]
- [Masked Language Modeling for Proteins via Linearly Scalable Long-Context Transformers](https://arxiv.org/abs/2006.03555) \[Choromanski et al., 2020\]
- [Random Feature Attention](https://openreview.net/forum?id=QtTKTdVrFBB) \[Peng et al., ICLR 2021\]
- [Linear Transformers Are Secretly Fast Weight Programmers](https://arxiv.org/abs/2102.11174) \[Schlag et al., ICML, 2021\]
- [Fast Transformers with Clustered Attention](https://arxiv.org/abs/2007.04825) \[Vyas et al., NeurIPS, 2020\]
- [Informer: Beyond Efficient Transformer for Long Sequence Time-Series Forecasting](https://arxiv.org/abs/2012.07436) \[Zhou et al., AAAI, 2021\]
- [Generating Wikipedia by Summarizing Long Sequences](https://arxiv.org/abs/1801.10198) \[Liu et al., 2018\]
- [Set Transformer: A Framework for Attention-based Permutation-Invariant Neural Networks](https://arxiv.org/abs/1810.00825) \[Lee et al., ICML, 2019\]
- [Luna: Linear Unified Nested Attention](https://arxiv.org/abs/2106.01540) \[Ma et al., NeurIPS, 2021\]
- [Linformer: Self-Attention with Linear Complexity](https://arxiv.org/abs/2006.04768) \[Wang et al., 2020\]
- [Poolingformer: Long Document Modeling with Pooling Attention](https://arxiv.org/abs/2105.04371) \[Zhang et al., ICML, 2021\]
- [Low-Rank and Locality Constrained Self-Attention for Sequence Modeling](https://ieeexplore.ieee.org/document/8894858) \[Guo et al., IEEE/ACM Transactions on Audio, Speech, and Language Processing, 2019\]
- [Compressed Self-Attention for Deep Metric Learning with Low-Rank Approximation](https://www.ijcai.org/proceedings/2020/285) \[Chen et al., IJCAI, 2020\]
- [Nyströmformer: A Nyström-Based Algorithm for Approximating Self-Attention](https://arxiv.org/abs/2102.03902) \[Xiong et al., AAAI, 2021\]
- [Gaussian Transformer: A Lightweight Approach for Natural Language Inference](https://ojs.aaai.org/index.php/AAAI/article/view/4614) \[Guo et al., AAAI, 2019\]
- [Predictive Attention Transformer: Improving Transformer with Attention Map Prediction](https://openreview.net/forum?id=YQVjbJPnPc9) \[Wang et al., 2021\]
- [RealFormer: Transformer Likes Residual Attention](https://arxiv.org/abs/2012.11747) \[He et al., 2020\]
- [LazyFormer: Self Attention with Lazy Update](https://arxiv.org/abs/2102.12702) \[Ying et al., 2021\]
- [Conditionally Adaptive Multi-Task Learning: Improving Transfer Learning in NLP Using Fewer Parameters & Less Data](https://arxiv.org/abs/2009.09139) \[Pilault et al., 2020\]
- [Synthesizer: Rethinking Self-Attention in Transformer Models](https://arxiv.org/abs/2005.00743) \[Tay et al., 2020\]
- [Multi-Head Attention with Disagreement Regularization](https://aclanthology.org/D18-1317/) \[Li et al., EMNLP, 2018\]
- [Guiding Attention for Self-Supervised Learning with Transformers](https://aclanthology.org/2020.findings-emnlp.419/) \[Deshpande et al., EMNLP, 2020\]
- [Talking-Heads Attention](https://arxiv.org/abs/2003.02436) \[Shazeer et al., 2020\]
- [Multi-Head Attention: Collaborate Instead of Concatenate](https://arxiv.org/abs/2006.16362) \[Cordonnier et al., 2020\]
- [Adaptive Attention Span in Transformers](https://doi.org/10.18653/v1/P19-1032) \[Sukhbaatar, et al., ACL, 2019\]
- [Multi-Scale Self-Attention for Text Classification](https://arxiv.org/abs/1912.00544) \[Guo et al., AAAI, 2020\]
- [Improving Multi-head Attention with Capsule Networks](https://doi.org/10.1007/978-3-030-32233-5_25) \[Gu et al., NLPCC, 2019\]
- [Information Aggregation for Multi-Head Attention with Routing-by-Agreement](https://doi.org/10.18653/v1/N19-1359) \[Li et al., NAACL, 2019\]
- [Fast Transformer Decoding: One Write-Head is All You Need](https://arxiv.org/abs/1911.02150) \[Shazeer et al., 2019\]
- [Low-Rank Bottleneck in Multi-head Attention Models](http://proceedings.mlr.press/v119/bhojanapalli20a.html) \[Bhojanapalli et al., ICML, 2020\]
- [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://doi.org/10.18653/v1/N19-1423) \[Devlin et al., NAACL, 2019\]
- [On Position Embeddings in BERT](https://openreview.net/forum?id=onxoVA9FxMw) \[Wang et al., ICLR, 2021\]
- [Learning to Encode Position for Transformer with Continuous Dynamical Model](http://proceedings.mlr.press/v119/liu20n.html) \[Liu et al., ICML, 2020\]
- [Universal Transformers](https://openreview.net/forum?id=HyzdRiR9Y7) \[Dehghani et al., ICLR, 2019\]
- [Self-Attention with Relative Position Representations](https://doi.org/10.18653/v1/N18-2074) \[Shaw et al., NAACL, 2018\]
- [Insertion-based Decoding with Automatically Inferred Generation Order](https://transacl.org/ojs/index.php/tacl/article/view/1732) \[Gu et al., TACL, 2019\]
- [Transformer-XL: Attentive Language Models beyond a Fixed-Length Context](https://doi.org/10.18653/v1/P19-1285) \[Dai et al., ACL, 2019\]
- [Music Transformer: Generating Music with Long-Term Structure](https://openreview.net/forum?id=rJe4ShAcF7) \[Huang et al., ICLR, 2019\]
- [DeBERTa: Decoding-enhanced BERT with Disentangled Attention](https://arxiv.org/abs/2006.03654) \[He et al., 2020\]
- [Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer](https://arxiv.org/abs/1910.10683) \[Raffel et al., JMLR, 2020\]
- [Rethinking Positional Encoding in Language Pre-training](https://arxiv.org/abs/2006.15595) \[Ke et al., 2020\]
- [RoFormer: Enhanced Transformer with Rotary Position Embedding](https://arxiv.org/abs/2104.09864) \[Su et al., 2021\]
- [Encoding word order in complex embeddings](https://openreview.net/forum?id=Hke-WTVtwr) \[Wang et al., ICLR, 2020\]
- [R-Transformer: Recurrent Neural Network Enhanced Transformer](https://arxiv.org/abs/1907.05572) \[Wang et al., 2019\]
- [Conditional Positional Encodings for Vision Transformers](https://arxiv.org/abs/2102.10882) \[Chu et al., 2021\]
- [On the Computational Power of Transformers and its Implications in Sequence Modeling](https://arxiv.org/abs/2006.09286) \[Bhattamishra et al., 2020\]
- [Language Modeling with Deep Transformers](https://doi.org/10.21437/Interspeech.2019-2225) \[Irie et al., Interspeech, 2019\]
- [Understanding the Difficulty of Training Transformers](https://doi.org/10.18653/v1/2020.emnlp-main.463) \[Lie et al., EMNLP, 2020\]
- [Root Mean Square Layer Normalization](https://arxiv.org/abs/1910.07467) \[Zhang et al., NeurIPS, 2019\]
- [Understanding and Improving Layer Normalization](https://proceedings.neurips.cc/paper/2019/hash/2f4fe03d77724a7217006e5d16728874-Abstract.html) \[Xu et al., NeurIPS, 2019\]
- [PowerNorm: Rethinking Batch Normalization in Transformers](http://proceedings.mlr.press/v119/shen20e.html) \[Shen et al., ICML, 2020\]
- [ReZero is All You Need: Fast Convergence at Large Depth](https://arxiv.org/abs/2003.04887) \[Bachlechner et al., UAI, 2021\]
- [Attention is Not All You Need: Pure Attention Loses Rank Doubly Exponentially with Depth](https://arxiv.org/abs/2103.03404) \[Dong et al., ICML, 2021\]
- [Searching for Activation Functions](https://openreview.net/forum?id=Hkuq2EkPf) \[Ramachandran et al., ICLR Workshop, 2018\]
- [Gaussian Error Linear Units (GELUs)](https://arxiv.org/abs/1606.08415) \[Hendrycks et al., 2016\]
- [Language Modeling with Gated Convolutional Networks](http://proceedings.mlr.press/v70/dauphin17a.html) \[Dauphin et al., ICML, 2017\]
- [Large Memory Layers with Product Keys](https://proceedings.neurips.cc/paper/2019/hash/9d8df73a3cfbf3c5b47bc9b50f214aff-Abstract.html) \[Lample et al., NeurIPS, 2019\]
- [GShard: Scaling Giant Models with Conditional Computation and Automatic Sharding](https://arxiv.org/abs/2006.16668) \[Lepikhin, 2020\]
- [Switch Transformers: Scaling to Trillion Parameter Models with Simple and Efficient Sparsity](https://arxiv.org/abs/2101.03961) \[Fedus et al., JMLR, 2021\]
- [Lite Transformer with Long-Short Range Attention](https://openreview.net/forum?id=ByeMPlHKPH) \[Wu et al., ICLR, 2020\]
- [Funnel-Transformer: Filtering out Sequential Redundancy for Efficient Language Processing](https://proceedings.neurips.cc/paper/2020/hash/2cd2915e69546904e4e5d4a2ac9e1652-Abstract.html) \[Dai et al., NeurIPS, 2020\]
- [DeLighT: Deep and Light-weight Transformer](https://arxiv.org/abs/2008.00623) \[Mehta et al., 2020\]
- [Adaptive Computation Time for Recurrent Neural Networks](https://arxiv.org/abs/1603.08983) \[Graves, 2016\]
- [Controlling Computation versus Quality for Neural Sequence Models](https://arxiv.org/abs/2002.07106) \[Bapna et al., 2020\]
- [DeeBERT: Dynamic Early Exiting for Accelerating BERT Inference](https://doi.org/10.18653/v1/2020.acl-main.204) \[Xin, ACL, 2020\]
- [BERT Loses Patience: Fast and Robust Inference with Early Exit](https://arxiv.org/abs/2006.04152) \[Zhou et al., NeurIPS, 2020\]
- [Compressive Transformers for Long-Range Sequence Modelling](https://openreview.net/forum?id=SylKikSYDH) \[Rae et al., ICLR, 2020\]
- [Memformer: A Memory-Augmented Transformer for Sequence Modeling](https://arxiv.org/abs/2010.06891) \[Wu et al., AACL-IJCNLP, 2022\]
- [Adding Recurrence to Pretrained Transformers for Improved Efficiency and Context Size](https://arxiv.org/abs/2008.07027) \[Yoshida et al., 2020\]
- [HIBERT: Document Level Pre-training of Hierarchical Bidirectional Transformers for Document Summarization](https://doi.org/10.18653/v1/P19-1499) \[Zhang et al., ACL, 2019\]
- [Hi-Transformer: Hierarchical Interactive Transformer for Efficient and Effective Long Document Modeling](https://arxiv.org/abs/2106.01040) \[Wu et al., 2021\]
- [TENER: Adapting Transformer Encoder for Named Entity Recognition](https://arxiv.org/abs/1911.04474) \[Yan et al., 2019\]
- [Transformer in Transformer](https://arxiv.org/abs/2103.00112) \[Han et al., NeurIPS, 2021\]
- [Understanding and Improving Transformer From a Multi-Particle Dynamic System Point of View](https://openreview.net/forum?id=SJl1o2NFwS) \[Lu et al., 2019\]
- [Improving Transformer Models by Reordering their Sublayers](https://doi.org/10.18653/v1/2020.acl-main.270) \[Press et al., ACL, 2020\]
- [Mask Attention Networks: Rethinking and Strengthen Transformer](https://www.aclweb.org/anthology/2021.naacl-main.135) \[Fan et al., NAACL, 2021\]
- [The Evolved Transformer](http://proceedings.mlr.press/v97/so19a.html) \[So et al., ICML, 2019\]
- [Memory-Efficient Differentiable Transformer Architecture Search](https://arxiv.org/abs/2105.14669) \[Zhao et al., 2021\]
- [MLP-Mixer: An all-MLP Architecture for Vision](https://arxiv.org/abs/2105.01601) \[Tolstikhin et al., NeurIPS, 2021\]
- [Pay Attention to MLPs](https://arxiv.org/abs/2105.08050) \[Liu et al., NeurIPS, 2021\]
- [MetaFormer Is Actually What You Need for Vision](https://arxiv.org/abs/2111.11418) \[Yu et al., CVPR, 2022\]
- [HyperMixer: An MLP-based Green AI Alternative to Transformers](https://arxiv.org/abs/2203.03691) \[Mai et al., 2022\]
- [Do Transformer Modifications Transfer Across Implementations and Applications?](https://arxiv.org/abs/2102.11972) \[Narang et al., 2021\]
- [Pre-trained Models for Natural Language Processing: A Survey](https://arxiv.org/abs/2003.08271) \[Qiu et al., 2020\]
- [RoBERTa: A Robustly Optimized BERT Pretraining Approach](https://arxiv.org/abs/1907.11692) \[Liu et al., 2019\]
- [Improving Language Understanding by Generative Pre-Training](https://gwern.net/doc/www/s3-us-west-2.amazonaws.com/d73fdc5ffa8627bce44dcda2fc012da638ffb158.pdf) \[Radford et al., 2018\]
- [Language Models are Unsupervised Multitask Learners](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) \[Radford et al., 2019\]
- [LaMDA: Language Models for Dialog Applications](https://arxiv.org/abs/2201.08239) \[Thoppilan et al., 2022\]
- [PaLM: Scaling Language Modeling with Pathways](https://arxiv.org/abs/2204.02311) \[Chowdhery et al., 2022\]
- [BART: Denoising Sequence-to-Sequence Pre-training for Natural Language Generation, Translation, and Comprehension](https://doi.org/10.18653/v1/2020.acl-main.703) \[Lewis et al., ACL, 2020\]
- [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903) \[Wei et al., 2022\]
- [Self-Consistency Improves Chain of Thought Reasoning in Language Models](https://arxiv.org/abs/2203.11171) \[Wang et al., 2022\]
- [Emergent Abilities of Large Language Models](https://arxiv.org/abs/2206.07682) \[Wei et al., 2022\]
- [Faithful Chain-of-Thought Reasoning](https://arxiv.org/abs/2301.13379) \[Lyu et al., 2023\]
- [Training language models to follow instructions with human feedback](https://arxiv.org/abs/2203.02155) \[Ouyang et al., 2022\]
- [Deep Reinforcement Learning from Human Preferences](https://arxiv.org/abs/1706.03741) \[Christiano et al., NeurIPS, 2017\]
- [Transcending Scaling Laws with 0.1% Extra Compute](https://arxiv.org/abs/2210.11399) \[Tay et al., 2022\]
- [Scaling Instruction-Finetuned Language Models](https://arxiv.org/abs/2210.11416) \[Chung et al., 2022\]
- [UL2: Unifying Language Learning Paradigms](https://arxiv.org/abs/2205.05131) \[Tay et al., 2022\]
- [An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://arxiv.org/abs/2010.11929) \[Dosovitskiy et al., ICLR, 2021\]
- [Swin Transformer: Hierarchical Vision Transformer using Shifted Windows](https://arxiv.org/abs/2103.14030) \[Liu et al., ICCV, 2021\]
- [ViViT: A Video Vision Transformer](https://arxiv.org/abs/2103.15691) \[Arnab et al., ICCV, 2021\]
- [Scaling Vision Transformers to 22 Billion Parameters](https://arxiv.org/abs/2302.05442) \[Dehghani et al., 2023\]
- [A Survey on Vision Transformer](https://arxiv.org/abs/2012.12556) \[Han et al., IEEE, 2022\]
- [Transformers in Vision: A Survey](https://arxiv.org/abs/2101.01169) \[Khan et al., CSUR, 2022\]
- [Transforming medical imaging with Transformers? A comparative review of key properties, current progresses, and future perspectives](https://arxiv.org/abs/2206.01136v1) \[Li et al., Medical Image Analysis, 2023\]
- [Learning Transferable Visual Models From Natural Language Supervision](https://arxiv.org/abs/2103.00020) \[Radford et al., ICML, 2021\]
- [Florence: A New Foundation Model for Computer Vision](https://arxiv.org/abs/2111.11432) \[Yuan et al., 2021\]
- [BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation](https://arxiv.org/abs/2201.12086v2) \[Li et al., ICML, 2022\]
- [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/abs/2301.12597v1) \[Li et al., 2023\]
- [Zero-Shot Text-to-Image Generation](https://arxiv.org/abs/2102.12092) \[Ramesh et al., ICML, 2021\]
- [CogView: Mastering Text-to-Image Generation via Transformers](https://arxiv.org/abs/2105.13290v2) \[Ding et al., NeurIPS, 2021\]
- [ERNIE-ViLG: Unified Generative Pre-training for Bidirectional Vision-Language Generation](https://arxiv.org/abs/2112.15283) \[Zhang et al., 2021\]
- [Make-A-Scene: Scene-Based Text-to-Image Generation with Human Priors](https://arxiv.org/abs/2203.13131) \[Gafni et al., ECCV, 2022\]
- [CogView2: Faster and Better Text-to-Image Generation via Hierarchical Transformers](https://arxiv.org/abs/2204.14217) \[Ding et al., 2022\]
- [Scaling Autoregressive Models for Content-Rich Text-to-Image Generation](https://arxiv.org/abs/2206.10789) \[Yu et al., 2022\]
- [Muse: Text-To-Image Generation via Masked Generative Transformers](https://arxiv.org/abs/2301.00704) \[Chang et al., 2023\]
- [CogVideo: Large-scale Pretraining for Text-to-Video Generation via Transformers](https://arxiv.org/abs/2205.15868) \[Hong et al., 2022\]
- [Phenaki: Variable Length Video Generation From Open Domain Textual Description](https://arxiv.org/abs/2210.02399) \[Villegas et al., 2022\]
- [Scalable Diffusion Models with Transformers](https://arxiv.org/abs/2212.09748) \[Peebles et al., 2022\]
- [wav2vec 2.0: A Framework for Self-Supervised Learning of Speech Representations](https://arxiv.org/abs/2006.11477) \[Baevski et al., NeurIPS, 2020\]
- [HuBERT: Self-Supervised Speech Representation Learning by Masked Prediction of Hidden Units](https://arxiv.org/abs/2106.07447) \[Hsu et al., IEEE/ACM Transactions on Audio, Speech, and Language Processing, 2021\]
- [Robust Speech Recognition via Large-Scale Weak Supervision](https://arxiv.org/abs/2212.04356) \[Radford et al., 2022\]
- [Music Transformer](https://arxiv.org/abs/1809.04281) \[Huang et al., 2018\]
- [AudioLM: a Language Modeling Approach to Audio Generation](https://arxiv.org/abs/2209.03143) \[Borsos et al., 2022\]
- [MuLan: A Joint Embedding of Music Audio and Natural Language](https://arxiv.org/abs/2208.12415) \[Huang et al., 2022\]
- [MusicLM: Generating Music From Text](https://arxiv.org/abs/2301.11325) \[Agostinelli et al., 2023\]

## 参考資料

## お知らせ

少しでも弊社にご興味を持っていただけた方は、お気軽にご連絡頂けますと幸いです。まずはカジュアルにお話を、という形でも、副業を検討したいという形でも歓迎しています。

脚注

941

123

[^1]: 記事執筆当時。

[^2]: [Kyo氏によるご指摘](https://twitter.com/kyo_takano/status/1629407473789710338) を踏まえて該当論文を再読し、記述を修正いたしました。初稿にて不正確な情報を提示していましたことをお詫びするとともに、ご指摘に感謝いたします。

---

# NotebookLM 要約



---