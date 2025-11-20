#Expo #RevenueCat #ReactNative #AppStoreConnect

> [!summary] 概要
> Expo（React Native）アプリでRevenueCatを正しく動作させるための前提チェックと設定手順。App Store ConnectとRevenueCat Dashboard双方での準備を完了させた後、テスト時の注意点を確認します。

## ✅ 事前チェックリスト

- [ ] Paid Applications Agreementに同意し、**税務・銀行情報**まで承認済み
- [ ] Sandboxテスターの情報を最新化
- [ ] デバッグ用／本番用 Bundle ID、証明書、プロビジョニングを整理
- [ ] RevenueCat API Key & Shared Secretを安全な場所に保管

---

## 1. App Store Connectでの準備

> [!important] 契約・財務情報
> 承認が済むまではSandboxでもIAPを取得できません。契約ステータスが「アクティブ」であることを必ず確認してください。

### 1-1. 課金契約の確認
- Paid Applications Agreementに同意し、**税務情報・銀行口座**を登録する  
  [community.revenuecat.com](https://community.revenuecat.com/sdks-51/offerings-are-empty-in-developer-build-but-all-new-identifiers-match-up-2599#:~:text=Oh%20my%20gosh%2C%20I%20finally,really%20lacking%20here%3B%20it%20says)
- 契約ステータスが「アクティブ」であることをダッシュボードで確認

### 1-2. アプリ内課金商品の作成
- **製品ID**：App Store ConnectでSubscription／非消耗型などの製品を作成し、RevenueCatと完全一致させる  
  [medium.com](https://medium.com/@talhanoman61/revenuecat-offerings-not-loading-on-ios-heres-what-actually-fixed-it-bb45c0a15aed#:~:text=Turns%20out%2C%20the%20issue%20wasn%E2%80%99t,want%20to%20fix%20this%20quickly)
- **サブスクリプショングループ**：必ずグループを作成し、名称・ローカリゼーションを入力  
  [medium.com](https://medium.com/@talhanoman61/revenuecat-offerings-not-loading-on-ios-heres-what-actually-fixed-it-bb45c0a15aed#:~:text=3,Subscription%20Group)
- **ステータス**：「Prepare for Submission」のままにならないよう、スクリーンショットや説明文を埋めて「Ready to Submit」に変更  
  [capgo.app](https://capgo.app/docs/plugins/native-purchases/ios-sandbox-testing/#:~:text=Products%20must%20be%20in%20at,Submit%E2%80%9D%20status%20for%20sandbox%20testing)

### 1-3. App-specific Shared Secretの取得
- App Store Connect → App情報 → 「App特有の共有シークレット」から生成し、RevenueCat iOS設定に貼り付け  
  [medium.com](https://medium.com/@talhanoman61/revenuecat-offerings-not-loading-on-ios-heres-what-actually-fixed-it-bb45c0a15aed#:~:text=5.%20Set%20the%20App,Secret%20in%20RevenueCat)

---

## 2. RevenueCat Dashboardでの設定

> [!tip] 命名・IDはApp Store Connectと揃える
> Bundle ID、製品ID、Entitlement名の整合性が最も多いトラブル原因。ダッシュボード側で誤字がないか再チェック。

### 2-1. プロジェクトとアプリの作成
- 新しいプロジェクトを作成し、iOSアプリを追加
- Bundle IDがApp Store Connectと一致しているか確認

### 2-2. 製品（Products）の登録
- 「Products」でApp Store Connectの製品IDと同じIDを登録
- 購読の場合は `Product Type` を `Subscription` に設定

### 2-3. エンタイトルメント（Entitlements）の作成
- 有料ユーザーに解放する機能名（例：Pro）でEntitlementを作成
- 先ほど登録した製品を紐付ける

### 2-4. オファリング（Offerings）の作成
- `default` Offeringなどを作成し、表示したいパッケージを割り当て
- 複数オファリングで期間限定割引などを柔軟に切り替え  
  [community.revenuecat.com](https://community.revenuecat.com/general-questions-7/need-help-with-implementing-promo-codes-for-subscription-discounts-2962#:~:text=I%20would%20recommend%20using%20multiple,back%20to%20the%20old%20one)

### 2-5. APIキーの取得
- プロジェクト設定からiOS用公開鍵（API Key）を取得し、アプリ初期化コードへ設定

---

## 3. テスト時の注意点

- **伝播時間**：新しいサブスクリプションやIntroオファーは反映に数時間〜24時間かかる場合あり。反映待ちを想定したスケジュールを組む。
- **デバッグビルドのBundle ID**：`expo run:ios` では別Bundle IDになる。  
  - RevenueCat側でテスト用アプリを追加する  
  - もしくは `eas build` で本番と同じBundle IDを使用

---

これらのチェックを完了させれば、ExpoアプリでRevenueCatを使ったサブスクリプション実装・テストがスムーズになります。設定変更時は再度このリストを確認し、抜け漏れを防いでください。
