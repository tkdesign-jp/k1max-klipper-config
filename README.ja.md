# K1 MAX Klipper 設定

**Creality K1 MAX** 用の Klipper `printer.cfg` とチューニングノート。**大型 ABS 印刷**に最適化済み。

> 🇬🇧 English version: [README.md](README.md)
> ⚠️ 使用前に [DISCLAIMER.md](DISCLAIMER.md) を読むこと。

---

## これは何か

K1 MAX 用の個人 Klipper 設定。実際の印刷を通じてチューニング済み。何度かの失敗を経て、最終的に最大ビルドサイズの ABS T-Rex 頭蓋骨が綺麗に出力できた構成。

含まれるもの:

- K1 MAX の CoreXY 形状に対するステッパ / kinematics
- この個体で測定した Input Shaper 値
- 典型的なフィラメント向けの Pressure Advance チューニング
- ヒートベッド PID キャリブレーション
- ABS ワークフロー向けのSTART_PRINT / END_PRINT マクロ
- 安定した大型造形のための調整済み印刷速度

これは「**動いている1つの設定**」であって、決定版ではない。個体差で結果は変わる。

---

## 関連リポジトリ

安定した **有線 Ethernet 専用** 動作(Klipper ストリーミングと Moonraker の安定運用の前提)は別リポ:

➡️ [k1max-wired-network](https://github.com/tkdesign-jp/k1max-wired-network)

---

## これは何で、何ではないか

- K1 MAX 用の**ドロップイン Klipper 設定**。スタート地点として使い、自分の個体向けに調整すること
- **ファームウェア mod ではない**。すでに Klipper(純正または Helper-Script 版)が K1 MAX で動いている前提
- Input Shaper / Pressure Advance の値は**この個体と妥当なフィラメント**に固有。自分の機材で再測定すること
- **Creality 公認設定ではない**。Creality はこの設定を承認していない

---

## ⚠️ 先に読むこと

不正な `printer.cfg` は以下を引き起こす可能性がある:

- ステッパドライバの脱調や過熱
- ホットエンドやベッドの安全限界超過
- ビルドプレートやホットエンドの損傷
- 極端なケースでは、熱保護設定ミスで火災リスク

**自己責任で適用すること。** [DISCLAIMER.md](DISCLAIMER.md) 参照。

使用前に必ず:

- 現在の `printer.cfg` をバックアップ
- 不明な行は調べてから使う
- 自分の個体で Input Shaper 測定を再実行
- 使用フィラメントで Pressure Advance 校正を再実行
- 初回印刷前に熱限界を確認

---

## 動作環境

- Creality K1 MAX(このモデル限定)
- Klipper ファームウェア(純正 K1 MAX Klipper または Helper-Script 版)
- Mainsail または Fluidd がネットワーク経由でアクセス可能
- プリンタへの SSH アクセス

ネットワーク基盤の安定化には併用推奨: [k1max-wired-network](https://github.com/tkdesign-jp/k1max-wired-network)

---

## ⚠ 想定するハードウェア構成

この `printer.cfg` は **純正 K1 MAX 用ではない**。個別に改造された個体向けにチューニングされている。純正 K1 MAX を使っているなら、このまま読み込まないこと。少なくとも温度、PID、max_temp、Pressure Advance の値は、純正ハードウェアに対して誤った値になる。

参考個体の改造内容:

| 項目 | パーツ | 備考 |
|---|---|---|
| ホットエンドキット | Trianglelab **CHCB-OT** ホットエンドアップグレードキット(K1 / K1 Max / CR-M4 Sprite Extruder 用) | 純正 K1 系ホットエンドのドロップイン置換 |
| ノズル | [Trianglelab ZS V6](https://trianglelab.net/products/m6-zs-nozzle-hardened-steel-copper-alloy)(焼入れ鋼 + 銅合金、M6 ネジ) | CHCB-OT が M6/V6 規格のため装着可能、**純正 Creality Unicorn ではない** |
| ヒーター | セラミックヒーターコイル(CHC シリーズ、約 60W) | 純正カートリッジヒーターより昇温が速い |
| ヒートブレイク | バイメタル(チタン + 銅合金) | CHCB-OT キット同梱 |
| サーミスタ | PT1000 | **純正 NTC100K ではない** — `printer.cfg` の `sensor_type` と `pullup_resistor` を合わせる必要 |

設定値への影響:

- **センサータイプ変更**: 純正は NTC100K、この構成は PT1000。`sensor_type` を間違えると温度が完全にずれ、`verify_heater` が発動するか、もっと悪い結果に
- **セラミックヒーター(約60W) + 銅合金ノズル** = 昇温が速く熱応答が異なる。PID 値が純正と違う
- **焼入れ鋼ノズル先端**で、カーボンファイバ系フィラメントや高温も扱える。ただし `max_temp` は安全側で抑える
- **Pressure Advance** はこのノズル / ホットエンドの組み合わせで実測した値。自分の機材で再校正すること
- ホットエンドの物理寸法(メルトゾーン長)が純正と異なるため、リトラクト設定の調整が必要かも

ハードウェアが上記と異なる場合、**実運用前に自分の機材で実測すること**。

---

## このリポジトリのファイル

```
printer.cfg          Klipper メイン設定
(他のファイル追加予定 — マクロ、センサー設定 等)
```

> 🚧 このリポジトリは整備中。実際の `printer.cfg` はサニタイズ完了次第アップロードする(デバイス固有の UUID / パスを確認・必要に応じて削除)。

---

## インストール(プレビュー)

`printer.cfg` 公開後の流れ:

```bash
# プリンタ上(root で SSH)
cd /usr/data/printer_data/config/
# 現設定をバックアップ!
cp printer.cfg printer.cfg.bak.$(date +%Y%m%d)

# 新設定を取得
wget https://raw.githubusercontent.com/tkdesign-jp/k1max-klipper-config/main/printer.cfg

# Mainsail UI から Klipper 再起動、または:
sudo systemctl restart klipper
```

再起動後:

1. Mainsail Console で Klipper エラーが無いか確認
2. ホットエンドとベッドの温度が妥当か確認
3. 短い試し印刷(3DBenchy を低めのベッド温度で等)を実行

---

## チューニングノート

実設定公開後、このセクションで以下を記述予定:

- Input Shaper 値の測定方法(X / Y の周波数)
- フィラメント別 Pressure Advance テスト結果
- ホットエンドとベッドの PID チューニング値
- 大型 ABS 印刷で特定の速度を選んだ理由
- 既知の問題と回避策

---

## ライセンス

[MIT](LICENSE) — 自由に使ってよい、無保証。[DISCLAIMER.md](DISCLAIMER.md) も参照。
