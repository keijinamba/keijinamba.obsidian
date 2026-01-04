# Art カテゴリ ルール

## サブカテゴリ

### アーティスト（Artist）
**テンプレート**: `./Templates/Art_Artist_Template.md`
- **MOC**: `[[MOC/Art Artist]]`
- **タグ**: `Art`、`Art/Artist`
- **プロパティ**: `name`（英語名）、`name-ja`（日本語名）、`country`、`movement`（運動へのリンク）、`style`
- **用途**: 画家、彫刻家、建築家などのアーティスト
- **リンク形式**: `Art/Artists/XXX`

### アート作品（Asset）
**テンプレート**: `./Templates/Art_Asset_Template.md`
- **MOC**: `[[MOC/Art Asset]]`
- **タグ**: `Art`、`Art/Asset`
- **プロパティ**: `name`、`name-ja`、`artist`（アーティストへのリンク）、`year`、`style`、`painting-style`、`owner`
- **用途**: 絵画、彫刻、建築などの作品
- **リンク形式**: `Art/Asset/XXX/YYY`（アーティスト名/作品名）
- **画像**: 画像の幅を400pxに指定する場合がある（例: `![image|400](URL)`）

### 美術運動（Movement）
**テンプレート**: `./Templates/Art_Movement_Template.md`
- **MOC**: `[[MOC/Art Movement]]`
- **タグ**: `Art`、`Art/Movement`
- **プロパティ**: `name`（英語名）、`name-ja`（日本語名）、`period`
- **用途**: 印象派、シュルレアリスムなどの美術運動
- **リンク形式**: `Art/Movement/XXX`

## リンクの形式
- アーティスト: `Art/Artists/XXX`
- アート作品: `Art/Asset/XXX/YYY`
- 美術運動: `Art/Movement/XXX`
- MOCへのリンク: `[[MOC/Art]]`、`[[MOC/Art Artist]]`など（一部のMOCは`up:`プロパティを使用）

