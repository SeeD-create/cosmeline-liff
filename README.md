# LIFF サロン登録ページ

店頭QR → このページ → サロン自動紐づけ＋友だち追加（お客様の送信操作なし）。

## 構成

```
店頭QR（https://liff.line.me/{LIFF_ID}?salon=Sxxx）
  → LIFFがこの index.html を開く
  → liff.init → IDトークン取得
  → 本部GAS ?path=liff-register に {idToken, salonId} をPOST
  → GASが12_liff.js handleLiffRegister_ でIDトークン検証→顧客行にサロンID記録
  → 友だち追加オプション(On) によりBOT友だち追加 → follow → あいさつ
```

## デプロイ（GitHub Pages）

1. GitHubで新規リポジトリを作成（例: `cosmeline-liff`、Public）
2. `index.html` をアップロード（コミット）
3. リポジトリ Settings → Pages → Branch を `main` / `(root)` に設定して保存
4. 数分後 `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開される

## 設定値（index.html 内の書き換え箇所）

| 変数 | 値 |
|---|---|
| `LIFF_ID` | LINE Developers の LIFF タブで発行されたID |
| `GAS_ENDPOINT` | 本部GAS WebアプリのデプロイURL（`/exec` まで・`?secret=`なし） |

## LINE Developers 側の設定

- LIFFアプリの「エンドポイントURL」= 上記 GitHub Pages のURL
- サイズ: Tall / Scope: `profile` `openid` / 友だち追加オプション: On (aggressive)

## GAS 側の設定

- Script Property `LIFF_ID` に LIFF ID を登録
  （`07_salon.js buildSalonFollowUrl_` がこれを読みQRをLIFF URLにする）
- 登録後 `regenerateAllSalonQrCodes()` を実行し全サロンのQRを作り直す
