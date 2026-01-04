# AI Agent 指示書

## 全般的なルール

### ノート作成の基本原則
- **テンプレートの使用**: すべてのノートは`./Templates/`ディレクトリ内の適切なテンプレートから作成する
- **簡潔性**: テンプレートに書かれていること以外は、*指定がない限り*余計に書かず簡潔に要点をまとめる
- **MOCへの自動紐付け**: すべてのノートはfrontmatterの`in:`プロパティで適切なMOCファイルにリンクする。これによりMOCファイルのdataviewクエリが自動的にノートを表示する
- **画像の扱い**: 画像挿入が想定されている部分は`<!-- upload image here -->`のコメントを残して空けておく
- **リンクの形式**: properties部分で他ノートにリンクできるものは、適切なパス形式（例: `Wine/Regions/XXX/YYY`、`Biology/XXX`）で参照させる

### Frontmatterの構造
- 各テンプレートには`type`、`tags`、`in:`などのfrontmatterが定義されている
- `in:`プロパティには`[[MOC/XXX]]`形式でMOCファイルへのリンクを記述する
- `related:`プロパティには関連するノートへのリンクを配列形式で記述する
- `links:`プロパティには外部リンク（URL）を記述する

---

## カテゴリ別テンプレート

### 生物学（Biology）
**テンプレート**: `./Templates/Biology_Template.md`
- **MOC**: `[[MOC/Biology]]`
- **タグ**: `Biology`、必要に応じてサブタグ（例: `細胞生物学`）
- **用途**: 生物学の概念、細胞構造、進化、遺伝など

### 物理学（Physics）
**テンプレート**: `./Templates/Physics_Template.md`
- **MOC**: `[[MOC/Physics]]`
- **タグ**: `Physics`
- **用途**: 物理学の概念、素粒子、天体物理学など

### 化学（Chemistry）
**テンプレート**: `./Templates/Chemistry_Template.md`
- **MOC**: `[[MOC/Chemistry]]`
- **タグ**: `Chemistry`
- **用途**: 化学の概念、物質の性質など

### 地理学（Geography）
**テンプレート**: `./Templates/Geography_Template.md`
- **MOC**: `[[MOC/Geography]]`
- **タグ**: `Geography`
- **用途**: 地理学的な現象、気候、地形など

### 哲学（Philosophy）
**テンプレート**: `./Templates/Philosophy_Concept_Template.md`
- **MOC**: `[[MOC/Philosophy]]`、`[[MOC/Philosophy Concept]]`
- **タグ**: `Philosophy`、`Philosophy/Concept`
- **用途**: 哲学的概念、思想など

### アート（Art）

#### アーティスト（Artist）
**テンプレート**: `./Templates/Art_Artist_Template.md`
- **MOC**: `[[MOC/Art Artist]]`
- **タグ**: `Art`、`Art/Artist`
- **プロパティ**: `name`（英語名）、`name-ja`（日本語名）、`country`、`movement`（運動へのリンク）、`style`
- **用途**: 画家、彫刻家、建築家などのアーティスト

#### アート作品（Asset）
**テンプレート**: `./Templates/Art_Asset_Template.md`
- **MOC**: `[[MOC/Art Asset]]`
- **タグ**: `Art`、`Art/Asset`
- **プロパティ**: `name`、`name-ja`、`artist`（アーティストへのリンク）、`year`、`style`、`painting-style`、`owner`
- **用途**: 絵画、彫刻、建築などの作品

#### 美術運動（Movement）
**テンプレート**: `./Templates/Art_Movement_Template.md`
- **MOC**: `[[MOC/Art Movement]]`
- **タグ**: `Art`、`Art/Movement`
- **プロパティ**: `name`（英語名）、`name-ja`（日本語名）、`period`
- **用途**: 印象派、シュルレアリスムなどの美術運動

### クラシック音楽（Classical Music）

#### 作曲家（Composer）
**テンプレート**: `./Templates/Classical_Music_Composer_Template.md`
- **MOC**: `[[MOC/ClassicalMusic Composer]]`
- **タグ**: `ClassicalMusic`、`ClassicalMusic/Composer`
- **プロパティ**: `name`、`name-ja`、`country`、`birth-date`、`death-date`、`era`（時代へのリンク）、`style`
- **用途**: クラシック音楽の作曲家

#### 音楽時代（Era）
**テンプレート**: `./Templates/Classical_Music_Era_Template.md`（存在する場合）
- **MOC**: `[[MOC/ClassicalMusic Era]]`
- **用途**: バロック、古典派、ロマン派などの音楽時代

### 文学（Literature）

#### 著者（Author）
**テンプレート**: `./Templates/Literature_Author_Template.md`
- **MOC**: `[[MOC/Literature Author]]`
- **タグ**: `Literature`、`Literature/Author`
- **プロパティ**: `name`、`name-ja`、`country`、`birth-date`、`death-date`、`movement`、`style`
- **用途**: 小説家、詩人、劇作家などの著者

### ワイン（Wine）

#### ブドウ品種（Grape）
**テンプレート**: `./Templates/Wine_Grape_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Grape]]`
- **タグ**: `Wine`、`Wine/Grape`
- **プロパティ**: `grape-en`、`grape-ja`、`color`、`style`、`acidity`、`tannin`、`aroma`（配列）、`aging-potential`、`major-regions`（配列、`Wine/Regions/XXX/YYY`形式でリンク）
- **用途**: カベルネ・ソーヴィニヨン、ピノ・ノワールなどのブドウ品種

#### ワイン産地（Region）
**テンプレート**: `./Templates/Wine_Region_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Region]]`
- **タグ**: `Wine`、`Wine/Region`
- **プロパティ**: `country`、`region`、`region-ja`、`sub-region`（配列）、`red-grapes`（配列）、`white-grapes`（配列）、`style`、`climate`、`soil`
- **用途**: ボルドー、ブルゴーニュなどのワイン産地

#### ワインメーカー（Maker）
**テンプレート**: `./Templates/Wine_Maker_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Maker]]`
- **タグ**: `Wine`、`Wine/Maker`
- **プロパティ**: `name`、`name-ja`、`country`、`region`（`Wine/Regions/XXX/YYY`形式でリンク）、`sub-region`、`location`（lat, lng）
- **用途**: シャトー・マルゴーなどのワイナリー

#### ワインショップ（Shop）
**テンプレート**: `./Templates/Wine_Shop_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Shop]]`
- **タグ**: `Wine`、`Wine/Shop`
- **プロパティ**: `name`、`location`
- **用途**: ワインショップ、ワイン販売店

#### ワインテイスティング記録（Tasting）
**テンプレート**: `./Templates/Wine_Tasting_Template.md`
- **MOC**: `[[MOC/Wine]]`（テンプレートに`in:`がない場合は追加を検討）
- **タグ**: `Wine`
- **プロパティ**: `name`、`maker`、`vintage`、`country`、`region`、`grape`（配列）、`price`、`shop`、`rating`（0-5）、`review`
- **用途**: 実際にテイスティングしたワインの記録

#### ワイン用語（Word）
**テンプレート**: `./Templates/Wine_Word_Template.md`
- **MOC**: `[[MOC/Wine]]`、`[[MOC/Wine Word]]`
- **タグ**: `Wine`、`Wine/Word`
- **用途**: タンニン、オークなどのワイン用語や知識

### プログラミング（Programming）
**テンプレート**: `./Templates/Programming_Template.md`
- **MOC**: `[[MOC/Programming]]`
- **タグ**: `Programming`
- **プロパティ**: `date`
- **用途**: プログラミングの技術、ツール、手法など

### 場所（POI: Point of Interest）
**テンプレート**: `./Templates/POI_Template.md`
- **MOC**: `[[MOC/POI]]`
- **タグ**: `POI`
- **プロパティ**: `date`、`location`
- **用途**: 訪れた場所、観光地、名所など

### 日記（Diary）
**テンプレート**: `./Templates/Diary_Template.md`
- **MOC**: `[[MOC/Diary]]`
- **タグ**: `Diary`
- **用途**: 日記、個人的な記録

### ランダム（Random）
**テンプレート**: `./Templates/Random_Template.md`
- **MOC**: `[[MOC/Random]]`
- **用途**: 上記のカテゴリに分類されない雑多なノート

### Webクリッピング（Web Clipper）
**テンプレート**: `./Templates/Web_Clipper_Template.md`
- **MOC**: `[[MOC/WebClipping]]`
- **タグ**: `Web`
- **用途**: Webページのクリッピング、保存した記事など

---

## 重要な注意事項

### リンクの形式
- **ワイン関連**: `Wine/Regions/XXX/YYY`、`Wine/Grapes/XXX`、`Wine/Makers/XXX/YYY`などの形式を使用
- **その他**: `Biology/XXX`、`Art/Artists/XXX`、`Physics/XXX`などの形式を使用
- **MOCへのリンク**: `[[MOC/XXX]]`形式を使用（スペースを含む場合は`[[MOC/XXX YYY]]`）

### 画像の扱い
- 画像を挿入する予定の場所には`<!-- upload image here -->`のコメントを残す
- 既存の画像URLがある場合は、そのまま使用する
- アート作品のAssetノートでは、画像の幅を400pxに指定する場合がある（例: `![image|400](URL)`）

### MOCファイルとの連携
- MOCファイルはdataviewクエリを使用して、`in:`プロパティにそのMOCへのリンクを含むノートを自動的に表示する
- 新しいノートを作成する際は、必ず適切なMOCへのリンクを`in:`プロパティに追加する
- 一部のMOCは`up:`プロパティを使用している場合がある（例: `MOC/Art.md`）

### テンプレートの選択
- ユーザーが明示的にテンプレートを指定した場合は、そのテンプレートを使用する
- テンプレートが指定されていない場合は、ノートの内容やカテゴリから適切なテンプレートを推測して使用する
- 不明な場合は、ユーザーに確認するか、最も近いテンプレートを使用する

---

## 作業フロー

1. **テンプレートの確認**: 作成するノートの種類に応じて適切なテンプレートを`./Templates/`ディレクトリから選択
2. **テンプレートの読み込み**: 選択したテンプレートの内容を確認
3. **ノートの作成**: テンプレートをベースに、ユーザーが提供した情報を元にノートを作成
4. **MOCへのリンク確認**: frontmatterの`in:`プロパティに適切なMOCファイルへのリンクが含まれていることを確認
5. **リンクの形式確認**: 他のノートへのリンクが適切なパス形式になっていることを確認
6. **画像プレースホルダーの確認**: 画像が必要な場合は`<!-- upload image here -->`コメントを残す
