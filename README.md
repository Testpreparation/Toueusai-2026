# 桐榮祭2026 打ち上げ管理アプリ

出欠・メンバー名簿・登録申請・買い物リスト・予算/会計・タスクを一元管理するWebアプリです。

## ファイル
- `index.html`: アプリ画面
- `database.rules.json`: Firebase Realtime Databaseのアクセス制御ルール案

## Firebase接続設定
Realtime Database URL:
`https://toueusai-utiage2026-default-rtdb.firebaseio.com/`

メイン管理者メール:
`dkrg26nu@uni.mejiro.ac.jp`

### 初回設定
1. Firebase Console → プロジェクトの設定 → マイアプリから実際の `apiKey` と `appId` を取得し、`index.html` の `firebaseConfig` に設定します。
2. Firebase Authenticationでメール認証を有効にし、**メールリンク（パスワードなしのログイン）**を有効にします。
3. Authenticationの設定で、公開サイトのドメインを承認済みドメインに追加します。メール内のリンクは、アプリを開いた同じブラウザで完了してください。別の端末で開いた場合は、メールアドレスの再入力が必要になることがあります。
4. Realtime Databaseのルール画面に `database.rules.json` のルール案を適用し、Firebase Emulator Suiteでテストしてください。
5. 管理者権限はメールアドレスの比較で付与しないでください。Firebase Admin SDKを使う信頼されたサーバー環境から、確認済みのUIDにカスタムクレーム `admin: true` を付与します。メイン管理者には `mainAdmin: true` も付与します。
6. メンバー名簿はUIDをキーとして管理者が登録します。利用者は本人のUIDに紐づく出欠だけを登録できます。

## 重要なセキュリティ注意
- このリポジトリは公開リポジトリです。サービスアカウント秘密鍵、Admin SDK秘密鍵、認証トークンなどを絶対にコミットしないでください。
- Firebaseのクライアント設定は秘密鍵ではありませんが、アクセス制御は必ずSecurity Rulesで行ってください。
- このルール案は初期構成です。本番運用前にFirebase Emulator Suiteで全アクセス経路をテストし、必要な検証を追加してください。
- 会計の購入処理と台帳更新は別々の書き込みになるため、実運用ではCloud Functions等のサーバー側トランザクションで二重計上防止を実装してください。
- メイン管理者メールアドレスだけでは権限を付与しません。本人確認済みUIDを管理者が確認したうえで、サーバー側からカスタムクレームを付与してください。
- このアプリは静的HTMLだけで完結する安全な管理システムではありません。Security Rules・管理者権限設定・テストが完了するまでは本番データを登録しないでください。
