# Wine カテゴリ ルール

## サブカテゴリ

### ブドウ品種（Grape）
**テンプレート**: `./Templates/Wine_Grape_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Grape]]`
- **タグ**: `Wine`、`Wine/Grape`
- **プロパティ**: `grape-en`、`grape-ja`、`color`、`style`、`acidity`、`tannin`、`aroma`（配列）、`aging-potential`、`major-regions`（配列、`Wine/Regions/XXX/YYY`形式でリンク）
- **用途**: カベルネ・ソーヴィニヨン、ピノ・ノワールなどのブドウ品種
- **リンク形式**: `Wine/Grapes/XXX`

### ワイン産地（Region）
**テンプレート**: `./Templates/Wine_Region_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Region]]`
- **タグ**: `Wine`、`Wine/Region`
- **プロパティ**: `country`、`region`、`region-ja`、`sub-region`（配列）、`red-grapes`（配列）、`white-grapes`（配列）、`style`、`climate`、`soil`
- **用途**: ボルドー、ブルゴーニュなどのワイン産地
- **リンク形式**: `Wine/Regions/XXX/YYY`（国/地域）

### ワインメーカー（Maker）
**テンプレート**: `./Templates/Wine_Maker_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Maker]]`
- **タグ**: `Wine`、`Wine/Maker`
- **プロパティ**: `name`、`name-ja`、`country`、`region`（`Wine/Regions/XXX/YYY`形式でリンク）、`sub-region`、`location`（lat, lng）
- **用途**: シャトー・マルゴーなどのワイナリー
- **リンク形式**: `Wine/Makers/XXX/YYY`（国/ワイナリー名）

### ワインショップ（Shop）
**テンプレート**: `./Templates/Wine_Shop_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Shop]]`
- **タグ**: `Wine`、`Wine/Shop`
- **プロパティ**: `name`、`location`
- **用途**: ワインショップ、ワイン販売店
- **リンク形式**: `Wine/Shops/XXX`

### ワインテイスティング記録（Tasting）
**テンプレート**: `./Templates/Wine_Tasting_Template.md`
- **MOC**: `[[MOC/Wine]]`
- **タグ**: `Wine`
- **プロパティ**: `name`、`maker`、`vintage`、`country`、`region`、`grape`（配列）、`price`、`shop`、`rating`（0-5）、`review`
- **用途**: 実際にテイスティングしたワインの記録
- **リンク形式**: `Wine/Tasting/XXX`

### ワイン用語（Word）
**テンプレート**: `./Templates/Wine_Word_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Word]]`
- **タグ**: `Wine`、`Wine/Word`
- **用途**: タンニン、オークなどのワイン用語や知識
- **リンク形式**: `Wine/Word/XXX`

## リンクの形式
- ブドウ品種: `Wine/Grapes/XXX`
- ワイン産地: `Wine/Regions/XXX/YYY`（国/地域）
- ワインメーカー: `Wine/Makers/XXX/YYY`（国/ワイナリー名）
- ワインショップ: `Wine/Shops/XXX`
- ワインテイスティング: `Wine/Tasting/XXX`
- ワイン用語: `Wine/Word/XXX`
- MOCへのリンク: `[[MOC/Wine]]`、`[[MOC/Wine Grape]]`など

## 特別な注意事項
- properties部分で`regions`、`grapes`、`maker`など他ノートにリンクできるものは、必ず`Wine/Regions/XXX/YYY`などの形式で参照させる

