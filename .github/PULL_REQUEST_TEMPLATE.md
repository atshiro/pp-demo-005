# Solution 変更内容

<!-- Solution の変更内容を簡潔に。何が増えた／変わったか -->

## 変更種別

- [ ] 新規 Component 追加（Canvas App / Cloud Flow / Table / 等）
- [ ] 既存 Component の修正
- [ ] Connection Reference の追加・変更（PRD env で要手動 Connection 作成）
- [ ] settings ファイル変更（CoE レビュー必須）
- [ ] その他

## STG 動作確認

- [ ] STG にデプロイされた後、Make.powerautomate.com で動作確認済み
- [ ] 確認手順は `docs/RUNBOOK.md` を参照

## PRD 反映時の追加作業（該当する場合）

- [ ] PRD env で Connection 作成が必要（誰が・いつ）
- [ ] `src/Solutions/<solution_name>/settings/deployment-settings.prd.json` への Connection Reference GUID 追記が必要

## 関連 Issue / Ticket

<!-- もしあれば -->

---

> v0 では CODEOWNERS 未配置のため、全 PR は CI green 後に auto-merge されます（範囲限定の CoE レビューは Phase 2 バックログ）。
