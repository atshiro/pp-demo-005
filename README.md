# pp-app-template

Power Platform 案件用 GitHub リポジトリのテンプレート。
1 案件 = 1 Division/Work Environment Group = 1 リポジトリ。DEV / STG / PRD の 3 環境を 1 つの repo で内包する。

このテンプレートは CoE（[atshiro](https://github.com/atshiro) Org）が中央管理し、各案件チームへ Repository Template として配布する。

> **このリポジトリは サンプル v0**（WS 1 デモ用、2026-06-01 時点）です。サンプル v0 → v0.5 → v1.0 の段階管理については [計画書 v2](../Documentリポジトリ/PowerPlatform-Azure連携基盤設計/CoE自動化/CoE自動化検討ワークショップ計画_PPリポ-CICDひな型配布_v2_デモ駆動.md) を参照。

---

## ディレクトリ構成

```
pp-app-template/
├─ .github/
│  ├─ workflows/                    # 5 本の Workflow（PPT スライド40 が正）
│  │  ├─ export-from-dev.yml        # ① Export/Commit（Maker の "export 依頼" 起動）
│  │  ├─ build-pack.yml             # ② Build/Pack（PR 時 lint / Solution Checker / managed Solution ビルド）
│  │  ├─ deploy-to-stg.yml          # ③ STG Deploy（main マージ時、managed Import）
│  │  ├─ promote.yml                # ④ Promote（WS 2 合意モード = (γ-1) 自動 or (γ-2) 手動 dispatch）
│  │  └─ deploy-to-prd.yml          # ⑤ PRD Deploy（承認後、managed Import、pre-flight チェック付き）
│  └─ PULL_REQUEST_TEMPLATE.md     # CODEOWNERS は Phase 2 バックログのため未配置
├─ src/
│  └─ Solutions/SampleSolution/    # pac solution unpack 済みの Solution ソース
│     ├─ src/                       # Canvas App / Cloud Flow / Table 定義など
│     ├─ Other/                     # Solution.xml / Customizations.xml など
│     └─ settings/                  # ★ Solution 単位の deployment-settings (maker-00 で自動生成)
│        ├─ deployment-settings.dev.json
│        ├─ deployment-settings.stg.json
│        └─ deployment-settings.prd.json
├─ scripts/
│  ├─ export.ps1
│  └─ import.ps1
├─ .editorconfig
└─ .gitignore
```

## このテンプレで動く CI/CD（全体像）

```
Maker (Make.powerautomate.com)
  │ ① "export 依頼" ボタン押下（UX (i) モック）
  ▼
[export-from-dev.yml]  Export from DEV  → unpack → feature branch に commit → PR 起票
  │ PR
  ▼
[build-pack.yml]  PR 時バリデート（lint / Solution Checker / managed Build）
  │ PR レビュー → main マージ
  ▼
[deploy-to-stg.yml]  STG へ managed Import
  │ STG 動作確認 OK
  ▼
[promote.yml]  (γ-1) 自動 / (γ-2) 手動 dispatch のどちらかで PRD へ promote
  │ pre-flight チェック（PRD env で Connection Reference が解決可能か）
  ▼
[deploy-to-prd.yml]  GitHub Environments の承認待ち → CoE 承認 → PRD へ managed Import
```

## 認証

- **OIDC + Federated Credentials**（GitHub Actions ↔ Entra App Registration ↔ Dataverse Application User）
- **2 SPN**：DEV/STG 共通 + PRD 分離

## 関連

- 計画書 v2：[CoE自動化検討ワークショップ計画_PPリポ-CICDひな型配布_v2_デモ駆動.md](../Documentリポジトリ/CoE自動化/CoE自動化検討ワークショップ計画_PPリポ-CICDひな型配布_v2_デモ駆動.md)
- 設計項目一覧（進行管理板）：[サンプルv0_設計項目一覧.md](../Documentリポジトリ/CoE自動化/サンプルv0_設計項目一覧.md)
- STEP-OWNER / Maker 向けビュー / シーケンス図：[pp-app-template-docs/](../Documentリポジトリ/CoE自動化/pp-app-template-docs/)（設計ドキュメントは中部設計リポジトリ側で管理）
