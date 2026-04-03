# オフラインパッケージ結合手順書

## 概要
`offline_packages_windows.tar.gz` を 500MB ずつに分割しました。
社内ダウンロード時はこの手順に従ってファイルを結合してください。

## 分割ファイル一覧
- `part_aa` ～ `part_aw` (合計 23 ファイル、最後のファイルのみ 72MB)
- `offline_packages_windows.tar.gz.sha256` (整合性検証用ハッシュ)

## ダウンロード手順

### 1. すべての `part_*` ファイルをダウンロード
```bash
# GitHub から part_aa, part_ab, part_ac ... part_aw を社内サーバーにダウンロード
# 例：wget, curl, またはブラウザで以下のファイルを全てダウンロード
part_aa
part_ab
part_ac
part_ad
part_ae
part_af
part_ag
part_ah
part_ai
part_aj
part_ak
part_al
part_am
part_an
part_ao
part_ap
part_aq
part_ar
part_as
part_at
part_au
part_av
part_aw
```

### 2. ハッシュ検証ファイルもダウンロード
```bash
# 整合性検証用のハッシュファイルをダウンロード
offline_packages_windows.tar.gz.sha256
```

## 結合手順

### ステップ 1: 社内サーバーでファイルを結合
```bash
cat part_a* > offline_packages_windows.tar.gz
```

### ステップ 2: ハッシュ値の検証（推奨）
```bash
sha256sum -c offline_packages_windows.tar.gz.sha256
```

成功時の出力：
```
offline_packages_windows.tar.gz: OK
```

### ステップ 3: アーカイブを展開
```bash
tar -xzf offline_packages_windows.tar.gz
```

これで `offline_packages_windows/` ディレクトリが作成され、
すべての `.whl` ファイルが展開されます。

## トラブルシューティング

### ハッシュが一致しない場合
```bash
sha256sum offline_packages_windows.tar.gz
```
で生成された値を、`offline_packages_windows.tar.gz.sha256` の値と比較してください。
一致しない場合は、**ファイル破損の可能性があります**。
すべての `part_*` ファイルを再度ダウンロードしてください。

### 展開に失敗する場合
```bash
tar -tzf offline_packages_windows.tar.gz | head
```
でアーカイブが正常か確認してください。

## 注意事項
- `part_*` ファイルの順序が重要です（`part_aa` → `part_ab` → ... → `part_aw`）
- すべての `part_*` ファイルが揃っていることを確認してください（全 23 ファイル）
- ディスク空き容量が充分にあることを確認してください（ダウンロード時に追加で 11GB、結合時に余裕を持たせて 12GB 推奨）
