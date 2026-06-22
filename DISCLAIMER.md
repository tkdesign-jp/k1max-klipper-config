# Disclaimer

## English

This repository contains a Klipper `printer.cfg` for a **Creality K1 MAX 3D printer**. Read this before using anything in this repository.

### Warranty

Replacing the `printer.cfg` may **void your manufacturer warranty**, especially if combined with firmware modifications. Creality has not authorized this configuration. If your printer is under warranty and you may need to claim it, do not apply this.

### Risk

A wrong Klipper config can cause:

- **Steppers skipping, stalling, or overheating** (too-aggressive currents or speeds).
- **Hotend or bed temperatures exceeding safe limits** (wrong thermal protection settings).
- **Print head collisions with the build plate** (wrong Z offset, wrong endstop, wrong soft limits).
- **Damage to the build plate, hotend, or PEI/glass surface.**
- **In the worst case, fire risk** if MAX_TEMP / `verify_heater` are misconfigured.

These are not theoretical. Misconfigured Klipper installs have caused all of the above in the community.

### What's specific to *this* printer

- Input Shaper values (X / Y resonance frequencies) are **measured on one specific K1 MAX**. They will not be optimal on yours.
- Pressure Advance values are **specific to certain filaments tested by the author**. They will not be optimal on yours.
- PID values for hotend and bed are **specific to this unit's heaters and thermistors**. Re-tune on yours.

Treat this config as a *reference*, not a drop-in solution.

### Responsibility

By using anything in this repository, you accept that:

- **You are solely responsible** for any damage to your printer, loss of warranty, fire, injury, lost prints, or any consequential damages.
- The author (**T.K DΞSIGN**) provides this material **as-is**, with **no warranty** of any kind, express or implied.
- The author has **no obligation** to provide support, fixes, or compensation if something goes wrong.
- This is a personal project shared as a reference, not a commercial product.

### Before applying

Recommended precautions:

1. Back up your current `printer.cfg`:
   ```
   cp printer.cfg printer.cfg.bak.$(date +%Y%m%d)
   ```
2. Read every line of the new config. Search for any setting you don't understand.
3. Re-run Klipper's Input Shaper measurement on your printer.
4. Re-run Pressure Advance calibration with your filament.
5. Re-run PID calibration for both hotend and bed.
6. Verify MAX_TEMP, min_temp, min_extrude_temp, and any `verify_heater` settings.
7. Do a short low-stakes test print first (e.g., 3DBenchy at moderate temps).
8. Do not leave the first prints unattended.

---

## 日本語

このリポジトリは **Creality K1 MAX 3Dプリンタ** 用の Klipper `printer.cfg` を含む。使用前に必ず読むこと。

### 保証

`printer.cfg` を置き換えると、特にファームウェア改造と併用した場合、**メーカー保証が無効になる可能性**がある。Creality はこの設定を承認していない。保証期間中で保証を使う可能性があるなら、適用しないこと。

### リスク

不正な Klipper 設定は以下を引き起こす可能性がある:

- **ステッパの脱調・停止・過熱**(電流や速度の過剰設定)
- **ホットエンド・ベッド温度の安全限界超過**(熱保護設定ミス)
- **プリンタヘッドとビルドプレートの衝突**(Z オフセット、エンドストップ、ソフトリミット ミス)
- **ビルドプレート、ホットエンド、PEI/ガラス面の損傷**
- **最悪のケースで火災リスク**(MAX_TEMP / `verify_heater` 設定ミス)

これらは理論上の話ではない。コミュニティで設定ミスの Klipper が実際にすべて引き起こした事例がある。

### この個体固有のもの

- Input Shaper の値(X / Y 共振周波数)は **特定の K1 MAX 1台で測定** したもの。あなたの機材で最適とは限らない
- Pressure Advance 値は **著者がテストした特定のフィラメント** に固有。あなたの機材で最適とは限らない
- ホットエンドとベッドの PID 値は **この個体のヒータとサーミスタ** に固有。自分の機材で再校正すること

この設定を*ドロップイン*ソリューションではなく*参考*として扱うこと。

### 責任

このリポジトリの内容を使うことで、以下に同意したものとする:

- プリンタの故障、保証喪失、火災、怪我、印刷ロス、その他派生する損害について **すべて自己責任**
- 著者(**T.K DΞSIGN**)は本素材を **現状のまま** 提供し、明示・黙示問わず**いかなる保証もしない**
- 何か起きても著者にはサポート・修正・補償の**義務はない**
- これは商用製品ではなく、参考として共有された個人プロジェクト

### 適用前の推奨手順

1. 現在の `printer.cfg` をバックアップ:
   ```
   cp printer.cfg printer.cfg.bak.$(date +%Y%m%d)
   ```
2. 新設定の全行を読む。理解できない設定は検索すること
3. Klipper の Input Shaper 測定を自分のプリンタで再実行
4. 使用フィラメントで Pressure Advance 校正を再実行
5. ホットエンドとベッド両方の PID 校正を再実行
6. MAX_TEMP、min_temp、min_extrude_temp、`verify_heater` 設定を確認
7. 短いリスクの低いテスト印刷を先に行う(3DBenchy を中程度温度で 等)
8. 最初の印刷は無人で放置しない
