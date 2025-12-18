---
type: "web-clipping"
title: "パターンマッチングを行う(LIKE演算子)"
source: "https://www.javadrive.jp/mysql/select/index7.html"
author: "Tatsuo Ikura"
site: "Buzzword Inc."
domain: "javadrive.jp"
published: 2022-04-05
created: 2025-12-18T12:57:14+09:00
description: "WHERE 句で条件を指定するときに LIKE 演算子を使用すると、カラムの値と文字列が一致するかどうかを比較する時に特殊な文字 '%' と '_' を使ってパターンマッチングを行うことができます。ここでは MySQL で LIKE 演算子を使ってカラムの値をパターンマッチングする方法について解説します。"
favicon: "https://www.javadrive.jp/favicon.ico"
image: "https://www.javadrive.jp/mysql/select/img/p7-0.png"
summarized: false
tags:
  - "WebClipping"
related:
in:
  - "[[MOC/WebClipping]]"
---

- [Home](https://www.javadrive.jp/) ›
- [MySQLの使い方](https://www.javadrive.jp/mysql/) ›
- [データの取得](https://www.javadrive.jp/mysql/select/)

WHERE 句で条件を指定するときに LIKE 演算子を使用すると、カラムの値と文字列が一致するかどうかを比較する時に特殊な文字 '%' と '\_' を使ってパターンマッチングを行うことができます。ここでは MySQL で LIKE 演算子を使ってカラムの値をパターンマッチングする方法について解説します。

(Last modified: )

目次

1. [パターンマッチングを行う](https://www.javadrive.jp/mysql/select/#section1)
2. [特殊文字をエスケープする](https://www.javadrive.jp/mysql/select/#section2)

## パターンマッチングを行う

WHERE 句で条件を指定する場合に、 LIKE 演算子を使用することでカラムの値に対して特殊な文字を使ったパターンマッチングを行うことができます。使い方は次の通りです。

```
SELECT col_name1 [, col_name2 ...] FROM table_name
  WHERE col_name LIKE pattern
```

パターンは( pattern )は特殊な文字であるパーセント(%)とアンダーバー(\_)を文字列と組み合わせた値として記述します。カラムの値全体がパターンとマッチした場合に条件式は TRUE となります。( LIKE 演算子の場合は カラムの値全体がマッチするようにパターンを指定する必要がある点に注意してください。 [REGEXP 演算子](https://www.javadrive.jp/mysql/select/index8.html) a>の場合はカラムの値のいずれかにマッチすれば TRUE となります)。

% と \_ の意味は次の通りです。

```
%   任意の0文字以上の文字列
_   任意の1文字
```

% は 0 文字以上の任意の文字列にマッチします。例えばパターンとして a%b が記述されていた場合、 a で始まり 0 個以上の任意の文字が間に入り最後に b で終わるような次の文字列にマッチします。

```
ab
axb
aonb
aoneb
atreeb
```

\_ は任意の 1 文字にマッチします。例えばパターンとして a\_b が記述されていた場合、 a で始まり 1 つの任意の文字が間に入り最後に b で終わるような次の文字列にマッチします。

```
axb
aob
agb
```

\-- --

それでは実際に試してみます。次のようなテーブルを作成しました。

create table user (name varchar(10), car varchar(15));

![パターンマッチングを行う(1)](https://www.javadrive.jp/mysql/select/img/p7-1.png)

テーブルに次のようなデータを追加しました。

insert into user values ('Yamada', 'Toyota Prius');  
insert into user values ('Suzuki', 'Nissan Serena');  
insert into user values ('Nishi', 'Honda Civic');  
insert into user values ('Takada', 'Toyota Crown');  
insert into user values ('Watanabe', 'Suzuki Alto');

![パターンマッチングを行う(2)](https://www.javadrive.jp/mysql/select/img/p7-2.png)

それではデータを取得します。条件式を記述しなかった場合はすべてのデータを取得します。

select \* from user;

![パターンマッチングを行う(3)](https://www.javadrive.jp/mysql/select/img/p7-3.png)

次に LIKE 演算子を使ってパターンマッチングを行ってみます。まず car カラムの値が Toyota で始まる任意の文字列と一致するデータを取得します。

select \* from user where car like 'Toyota%';

![パターンマッチングを行う(4)](https://www.javadrive.jp/mysql/select/img/p7-4.png)

次は name カラムの値が a が 2 回含まれるデータを取得します。

select \* from user where name like '%a%a%';

![パターンマッチングを行う(5)](https://www.javadrive.jp/mysql/select/img/p7-5.png)

最後に name カラムの値が 5 文字のデータを取得します。(パターンとして任意の一文字にマッチする \_ を 5 個記述しています)。

select \* from user where name like '\_\_\_\_\_';

![パターンマッチングを行う(6)](https://www.javadrive.jp/mysql/select/img/p7-6.png)

意図した通りのパターンを定義するのは少し慣れが必要ですが、便利な機能ですので色々試してみてください。

## 特殊文字をエスケープする

LIKE 演算子の中で % と \_ は特別な意味を持ちますが、特殊な文字ではなく文字の一つとしてこの2つの文字を使用したい場合には \\ 文字を使って次のようにエスケープ処理をする必要があります。また \\ も文字の一つして使用する場合もエスケープ処理をしてください。

\\%  
\\\_  
\\\\

次のように使います。

\[例\] address カラムの値が '\_back' で終わるデータを取得  
SELECT \* FROM staff WHERE address like '%\\\_back';

エスケープ処理をするために使われる文字はデフォルトでは \\ ですが別の文字に変更することもできます。次のように使います。

```
SELECT col_name1 [, col_name2 ...] FROM table_name
  WHERE col_name LIKE pattern ESCAPE 'escape_char'
```

次のように使います。

\[例\] address カラムの値が '\_back' で終わるデータを取得  
SELECT \* FROM staff WHERE address like '%#\_back' ESCAPE '#';

\-- --

それでは実際に試してみます。次のようなテーブルを作成しました。

create table user (name varchar(20));

![特殊文字をエスケープする(1)](https://www.javadrive.jp/mysql/select/img/p7-7.png)

テーブルに次のようなデータを追加しました。

insert into user values ('Yamada\_Takashi');  
insert into user values ('Suzuki-Hanako');  
insert into user values ('Nishi-Ichirou');  
insert into user values ('Takada\_Ken');  
insert into user values ('Watanabe\_Minami');

![特殊文字をエスケープする(2)](https://www.javadrive.jp/mysql/select/img/p7-8.png)

それでは LIKE 演算子を使ってパターンマッチングを行ってみます。 name カラムの値に '\_' という文字が含まれているデータを取得します。この時エスケープ処理をせずに次のように記述してしまうと '\_' は任意の 1 文字にマッチする特殊な文字なのですべてのデータがマッチしてしまいます。

select \* from user where name like '%\_%';

![特殊文字をエスケープする(3)](https://www.javadrive.jp/mysql/select/img/p7-9.png)

'\_' を単なる文字としてマッチさせたい場合はエスケープ処理をして次のように実行します。

select \* from user where name like '%¥\_%';

![特殊文字をエスケープする(4)](https://www.javadrive.jp/mysql/select/img/p7-10.png)

希望した結果を取得することができました。

またエスケープする時に使用する文字を \\ から例えば # に変更する場合は次のように実行します。

select \* from user where name like '%#\_%' escape '#';

![特殊文字をエスケープする(5)](https://www.javadrive.jp/mysql/select/img/p7-11.png)

エスケープする文字を変更することができました。

\-- --

LIKE 演算子を使ってカラムの値をパターンマッチングする方法について解説しました。

![プロフィール画像](https://www.javadrive.jp/img/facex.png)

著者 / [TATSUO IKURA](http://www.buzzword.co.jp/profile/)

これから IT 関連の知識を学ばれる方を対象に、色々な言語でのプログラミング方法や関連する技術、開発環境構築などに関する解説サイトを運営しています。

---

# NotebookLM 要約



---