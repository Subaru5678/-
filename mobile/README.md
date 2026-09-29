# mobile (Capacitor)

`../diy-calc` の資材計算アプリをiOS/Androidにパッケージするための Capacitor プロジェクトです。
Web側のコード自体はここには無く、`webDir` として `../diy-calc` を直接参照しています
(`capacitor.config.ts`)。`diy-calc/index.html` を編集すれば、次の `npx cap sync` で
そのままアプリ側にも反映されます。

- アプリID: `com.diycalc.reformtools`(本番公開前に自分のドメインの逆引きIDに変更推奨)
- 広告: `@capacitor-community/admob` を導入済み。現在は **Googleの公開テスト広告ID** で
  動作するように配線されています(実際の広告は表示されず課金も発生しません)

## セットアップ済みのもの

- `npx cap add android` / `npx cap add ios` 済み(`android/`, `ios/` ディレクトリ)
- `@capacitor/assets` でアイコン・スプラッシュ画面を自動生成済み(`assets/` の
  プレースホルダー画像から生成。本番前に差し替え推奨)
- AndroidManifest.xml / Info.plist に AdMob の App ID(テストID)を設定済み

## ビルド方法(あなたのPC/Macで)

このクラウドセッションのサンドボックスは `dl.google.com`(Android Gradle Plugin や
Google Play Servicesの取得元)へのネットワークアクセスが制限されているため、実機ビルドの
確認はできていません。以下は手元の環境での手順です。

### Android

1. [Android Studio](https://developer.android.com/studio) をインストール
2. このリポジトリをクローンし、`mobile` フォルダを Android Studio で開く
   (または `cd mobile && npx cap open android`)
3. 初回はAndroid SDKのダウンロードが走ります。ビルド → 実機/エミュレータで実行

### iOS

1. Xcode(Macが必要)と [CocoaPods](https://cocoapods.org/) をインストール
2. `cd mobile/ios/App && pod install`
3. `cd mobile && npx cap open ios` で Xcode を開き、実機/シミュレータで実行

## 本番リリース前にやること

1. **AdMobの本番ID化**
   - [AdMobコンソール](https://admob.google.com/)でアプリを登録し、
     バナー広告ユニットを作成
   - `android/app/src/main/res/values/strings.xml` の `admob_app_id` を本番の
     App IDに変更
   - `ios/App/App/Info.plist` の `GADApplicationIdentifier` を本番の App IDに変更
   - `../diy-calc/index.html` の `initAds()` 内の `TEST_BANNER_ID` を本番の
     広告ユニットIDに変更
   - iOS: `NSUserTrackingUsageDescription` の文言を確認・調整
     (ATT許諾プロンプトに表示される文言です)
2. **アプリID変更**(任意、推奨): `capacitor.config.ts` の `appId` を
   `com.あなたのドメイン.diycalc` のような自分の逆引きドメインに変更してから
   `npx cap sync` を再実行
3. **アイコン差し替え**: `assets/icon.png`(1024x1024)と `assets/splash.png` を
   本番デザインに差し替えて `npx capacitor-assets generate --android --ios` を再実行
4. **開発者アカウント登録**: Apple Developer Program($99/年) /
   Google Play Developer(初回$25)
5. Android Studio / Xcode でリリースビルドを作成し、各ストアコンソールから申請
