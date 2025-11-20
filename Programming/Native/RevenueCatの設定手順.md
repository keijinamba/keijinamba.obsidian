#Expo #RevenueCat #ReactNative #AppStoreConnect

Expo（React Native）アプリでRevenueCatを利用する場合、App Store ConnectやRevenueCat Dashboardでの事前設定が重要です。以下の手順は実機テストおよび本番リリースでRevenueCatを正しく動作させるためのチェックリストです。

## 1. App Store Connectでの準備

1. **課金契約の確認**
    
    - **Paid Applications Agreementの締結と銀行・税務情報の登録**：App Store ConnectでPaid Applications Agreementに同意するだけではなく、税務情報と銀行口座情報の登録が必要です[community.revenuecat.com](https://community.revenuecat.com/sdks-51/offerings-are-empty-in-developer-build-but-all-new-identifiers-match-up-2599#:~:text=Oh%20my%20gosh%2C%20I%20finally,really%20lacking%20here%3B%20it%20says)。これらが承認されるまでSandboxテストでもIAPが取得できません。
    - 契約ステータスが“アクティブ”になっているか確認します。
      
2. **アプリ内課金商品の作成**
    
    - **製品IDの作成**：アプリ内課金の「サブスクリプション」や「非消耗型アイテム」をApp Store Connectで作成し、製品IDを決めます。IDはRevenueCat Dashboardで設定する際に完全一致している必要があります[medium.com](https://medium.com/@talhanoman61/revenuecat-offerings-not-loading-on-ios-heres-what-actually-fixed-it-bb45c0a15aed#:~:text=Turns%20out%2C%20the%20issue%20wasn%E2%80%99t,want%20to%20fix%20this%20quickly)。
        
    - **サブスクリプショングループ**：サブスクリプションは必ずグループに属している必要があり、グループ名やローカリゼーション（表示名・説明文・スクリーンショット）を入力します[medium.com](https://medium.com/@talhanoman61/revenuecat-offerings-not-loading-on-ios-heres-what-actually-fixed-it-bb45c0a15aed#:~:text=3,Subscription%20Group)。
        
    - **ステータス確認**：商品のステータスが「提出準備中（Prepare for Submission）」になっている場合、スクリーンショットや説明文が未入力の可能性があります。すべての情報を入力して「送信準備完了（Ready to Submit）」にしておきます[capgo.app](https://capgo.app/docs/plugins/native-purchases/ios-sandbox-testing/#:~:text=Products%20must%20be%20in%20at,Submit%E2%80%9D%20status%20for%20sandbox%20testing)。
      
3. **App‑specific Shared Secretの取得**
    
    - App Store Connectの「App 情報」→「App 特有の共有シークレット」で生成したShared SecretをRevenueCatのiOS設定に貼り付けます[medium.com](https://medium.com/@talhanoman61/revenuecat-offerings-not-loading-on-ios-heres-what-actually-fixed-it-bb45c0a15aed#:~:text=5.%20Set%20the%20App,Secret%20in%20RevenueCat)。

4. **test**
	- aaa

5. aaa
    - asss


## 2. RevenueCat Dashboardでの設定

1. **プロジェクトとアプリの作成**
    
    - RevenueCat Dashboardで新しいプロジェクトを作成し、アプリ（iOSアプリ）を追加します。
        
    - Bundle IDがApp Store Connectで登録したものと一致しているか確認します。
        
2. **製品（Product）の登録**
    
    - Dashboardの「Products」で、App Store Connectの製品IDと同じIDで製品を登録します。
        
    - 購読の場合は `Product Type` を Subscription にします。
        
3. **エンタイトルメント（Entitlements）の作成**
    
    - 有料ユーザーに解放する機能名などでEntitlementを作成します（例：Pro）。
        
    - 作成したEntitlementに先ほど登録した製品を紐付けます。
        
4. **オファリング（Offerings）の作成**
    
    - DashboardのOfferingsで `default` Offering などを作成し、表示したいパッケージ（製品）を登録します。
        
    - 複数のオファリングを用意すると、期間限定割引など状況に応じてアプリ側で切り替え可能です[community.revenuecat.com](https://community.revenuecat.com/general-questions-7/need-help-with-implementing-promo-codes-for-subscription-discounts-2962#:~:text=I%20would%20recommend%20using%20multiple,back%20to%20the%20old%20one)。
        
5. **APIキーの取得**
    
    - RevenueCatのプロジェクト設定からiOS用の公開鍵（API Key）を取得し、アプリの初期化に使用します。
        

## 3. テスト時の注意点

- **プロダクトの伝播時間**：App Store Connectで新規作成したサブスクリプションやIntroオファーは反映に数時間〜24時間かかることがあります。設定後すぐに取得できない場合は時間を置いてから試すと良いでしょう。
    
- **デバッグビルドのバンドルID**：Expo run:ios で生成されるデバッグビルドは別のBundle IDになります。テスト用に同じIDでアプリを作成し、RevenueCatで別アプリとして管理するか、EAS buildで正しいバンドルIDを設定します。
    

これらの手順に従うことで、ExpoアプリでRevenueCatを利用したサブスクリプションの実装とテストがスムーズに進められます。必要な時にこのガイドを見直して、設定漏れがないか確認してください。
