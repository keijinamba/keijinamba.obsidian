---
type: programming
date: 2025.12.18
links:
  - https://nankurunikki.com/データベース/postgresql/671/
  - https://dev.mysql.com/doc/refman/8.0/ja/pattern-matching.html
tags:
  - Programming
  - Database
  - PostgreSQL
  - Mysql
related:
 - "[[【PostgreSQL】WHERE句を使ったあいまい検索の方法を解説！]]"
in:
  - "[[MOC/Programming]]"
---

## LIKE検索の基本

`LIKE`演算子は、パターンマッチングによるあいまい検索に使用する。

### ワイルドカード

- `%`：0文字以上の任意の文字列
- `_`：任意の1文字

### エスケープ

`%`や`_`をリテラル文字として検索する場合は、エスケープ文字（デフォルトは`\`）を使用する。

#### PostgreSQL

```sql
-- 「%」を含む文字列を検索
SELECT * FROM users WHERE name LIKE '%\%%';

-- 「_」を含む文字列を検索
SELECT * FROM users WHERE name LIKE '%\_%';

-- ESCAPE句でエスケープ文字を指定
SELECT * FROM users WHERE name LIKE '%#%' ESCAPE '#';
```

#### MySQL

```sql
-- 「%」を含む文字列を検索
SELECT * FROM pet WHERE name LIKE '%\%%';

-- 「_」を含む文字列を検索
SELECT * FROM pet WHERE name LIKE '%\_%';

-- ESCAPE句でエスケープ文字を指定
SELECT * FROM pet WHERE name LIKE '%#%' ESCAPE '#';
```

**注意点：**
- デフォルトのエスケープ文字は`\`（バックスラッシュ）
- `ESCAPE`句で任意の文字をエスケープ文字として指定可能
- エスケープ文字自体を検索する場合は、エスケープ文字を2つ続けて記述（例：`\\`）


## PostgreSQL

### 基本構文

```sql
SELECT * FROM テーブル名 WHERE カラム名 LIKE 'パターン';
```

### 特徴

- `LIKE`：大文字小文字を**区別する**
- `ILIKE`：大文字小文字を**区別しない**（PostgreSQL独自）

### 使用例

```sql
-- 「山」を含む
SELECT * FROM users WHERE name LIKE '%山%';

-- 「山」で始まる
SELECT * FROM users WHERE name LIKE '山%';

-- 「山」で終わる
SELECT * FROM users WHERE name LIKE '%山';

-- 5文字の名前
SELECT * FROM users WHERE name LIKE '_____';
```

## MySQL

### 基本構文

```sql
SELECT * FROM テーブル名 WHERE カラム名 LIKE 'パターン';
```

### 特徴

- `LIKE`：デフォルトで大文字小文字を**区別しない**
- 大文字小文字を区別する場合は`BINARY`キーワードや照合順序を指定

### 使用例

```sql
-- 「b」で始まる
SELECT * FROM pet WHERE name LIKE 'b%';

-- 「fy」で終わる
SELECT * FROM pet WHERE name LIKE '%fy';

-- 「w」を含む
SELECT * FROM pet WHERE name LIKE '%w%';

-- 5文字の名前
SELECT * FROM pet WHERE name LIKE '_____';
```

### 正規表現検索

MySQLでは`REGEXP_LIKE()`関数も利用可能：

```sql
-- 「b」で始まる（正規表現）
SELECT * FROM pet WHERE REGEXP_LIKE(name, '^b');

-- 「fy」で終わる（正規表現）
SELECT * FROM pet WHERE REGEXP_LIKE(name, 'fy$');

-- 5文字の名前（正規表現）
SELECT * FROM pet WHERE REGEXP_LIKE(name, '^.{5}$');
```

## 主な違い

| 項目 | PostgreSQL | MySQL |
|------|-----------|-------|
| 大文字小文字の区別 | `LIKE`は区別、`ILIKE`は区別しない | `LIKE`はデフォルトで区別しない |
| 正規表現 | `~`演算子や`SIMILAR TO` | `REGEXP_LIKE()`関数 |

---
