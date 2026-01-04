# Literature カテゴリ ルール

## カテゴリの定義

**このプロジェクトにおいて、Literatureは書物全般を指す。**
- 小説、詩、戯曲などの文学作品
- 学術書、ノンフィクション、エッセイなどの非文学作品
- すべての書物を含む

## サブカテゴリ

### 著者（Author）
**テンプレート**: `./Templates/Literature_Author_Template.md`
- **MOC**: `[[MOC/Literature Author]]`
- **タグ**: `Literature`、`Literature/Author`
- **プロパティ**: `name`、`name-ja`、`country`、`birth-date`、`death-date`、`movement`、`style`
- **用途**: 小説家、詩人、劇作家、学術書の著者など、すべての書物の著者
- **リンク形式**: `Literature/Authors/XXX`

### 書物（Book）
**テンプレート**: `./Templates/Literature_Book_Template.md`
- **MOC**: `[[MOC/Literature Book]]`
- **タグ**: `Literature`、`Literature/Book`
- **プロパティ**: `title`、`title-ja`、`author`（著者へのリンク）、`year`、`genre`、`category`（Fiction/Non-fiction）
- **用途**: 小説、詩集、戯曲、学術書、ノンフィクションなど、すべての書物
- **リンク形式**: `Literature/Books/XXX`

## リンクの形式
- 著者: `Literature/Authors/XXX`
- 書物: `Literature/Books/XXX`
- MOCへのリンク: `[[MOC/Literature Author]]`、`[[MOC/Literature Book]]`
