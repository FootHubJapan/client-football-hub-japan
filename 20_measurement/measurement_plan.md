# Measurement Plan

## 1. Overview
- Client: Football Hub Japan
- Project: CL-001
- Main Goal: Web改善＋計測整備を先行し、後続の広告テストを小さく開始できる状態を作る
- Primary CV: generate_lead（問い合わせ送信完了）
- Secondary CVs:
  - form_start（フォーム到達）
  - cta_click（CTAクリック）
  - phone_tap（電話タップ）
  - download_brochure（資料DL ※ある場合）
- Last Updated: 2026-03-14
- Owner: Tracking & Measurement Specialist
- Approver (Production publish): Yuki Isomura

---

## 2. Primary Conversion

### generate_lead
- Purpose: 問い合わせ送信完了（本命CV）
- Trigger (preferred): Success UI表示（同一ページ内の成功メッセージ表示）
- Trigger (fallback): N/A（現状はThank you pageなし）
- Page URL:
  - Main LP: https://football-hub-japan-ubzb.onrender.com/
  - Form page: https://football-hub-japan-ubzb.onrender.com/contact.html
  - Thank you: N/A
- Success condition:
  - contact.html 上で送信後に成功メッセージ `#successMessage` が表示された時点で発火

---

## 3. Secondary Conversions

### form_start
- Purpose: フォーム到達
- Trigger (preferred): フォーム要素の初回表示
- Trigger (fallback): フォームページのpage_view
- Page URL:
  - TBD
- Parameters:
  - page_path: {{Page Path}}
  - form_id: contact_main

### cta_click
- Purpose: CTAクリック
- Trigger: CTAクリック
- Target elements:
  - selector: TBD
- Parameters:
  - cta_text: {{Click Text}}
  - cta_location: unknown
  - page_path: {{Page Path}}

### phone_tap
- Purpose: 電話タップ
- Trigger: tel: リンククリック
- Parameters:
  - phone_number: {{Click URL}}
  - page_path: {{Page Path}}

### download_brochure
- Purpose: 資料DL（ある場合のみ）
- Trigger: 資料DLリンククリック
- Status: N/A until confirmed

---

## 4. URLs
- Main LP: https://football-hub-japan-ubzb.onrender.com/
- Form page: https://football-hub-japan-ubzb.onrender.com/contact.html
- Thank you page: TBD
- Download page: TBD
- generate_lead は同一ページ内の成功メッセージ `#successMessage` を Element Visibility で検知する方針に確定。
Trigger は Once per page + Observe DOM changes を採用。
generate_lead の初回実装完了。
contact.html の送信成功時に GA4 へ直接 `generate_lead` を送信する方式で実装。
送信成功UI表示と同時に以下パラメータで発火確認：
- form_id = contact_main
- lead_type = inquiry
- page_path = window.location.pathname
- cta_click の初回実装を追加。
[data-cta] 属性を持つ主要CTAクリック時に GA4 へ `cta_click` を直接送信。
送信パラメータ：
- cta_name
- cta_location
- cta_text
- page_path
---

## 5. Event Table
| Event Name | Type | Trigger (preferred / fallback) | URL/Page | Parameters | Priority | Status |
|---|---|---|---|---|---|---|
| generate_lead | Primary CV | success UI / thankyou page_view | Form/Thankyou | form_id, page_path, lead_type | High | Planned |
| form_start | Secondary CV | form visible / form page_view | Form | form_id, page_path | Medium | Planned |
| cta_click | Secondary CV | CTA click | Any | cta_text, cta_location, page_path | Medium | Planned |
| phone_tap | Secondary CV | tel: click | Any | phone_number, page_path | Medium | Planned |
| download_brochure | Secondary CV | download click / download page_view | Download | asset_name, page_path | Medium | N/A |

Status definitions:
- Planned
- Implemented
- Verified
- N/A

---

## 6. Tracking Architecture
- GA4 Property: TBD
- GTM Container: TBD
- Search Console: TBD
- CMS: Render / app site
- Source Repository: client-football-hub-japan
- Production publish owner: Yuki Isomura

---

## 7. UTM Rules
- utm_source: platform
- utm_medium: channel
- utm_campaign: fhj_<yyyymm>_<objective>_<offer>
- utm_content: creative variation
- utm_term: keyword（検索広告のみ）

### Rules
- 自社内リンクにはUTMを付けない
- 外部配布資料/メール/広告リンクにはUTMを付ける

---

## 8. Data Quality Checks
- Internal traffic exclusion: TBD
- Cross-domain needed?: TBD
- DebugView check: required
- Production verification method: 本番導線テスト + 24h確認
- Source/medium validation: required

---

## 9. Verification Checklist
- [ ] GTM Previewで generate_lead を確認
- [ ] GA4 DebugViewで generate_lead を確認
- [ ] form_start を確認
- [ ] cta_click を確認
- [ ] phone_tap を確認（ある場合）
- [ ] download_brochure を確認 or N/A
- [ ] 24時間以内に本番イベント反映を確認

---

## 10. Risks / Open Questions
- 問い合わせフォームのURL / 完了地点が未確定
- GTM / GA4 の現状未確認
- Success DOMの判定方法未確認

---

## 11. Change Log
- 2026-03-14: created
