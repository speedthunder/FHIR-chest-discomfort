# 胸部不舒服 FHIR Observation 記錄工具

**網頁網址：https://speedthunder.github.io/FHIR-chest-discomfort/**

---

## 簡介

本工具讓民眾、家屬、照護人員或社福人員，以**日常語言**描述胸部不舒服的狀況，並自動產生符合 **HL7 FHIR R4 Observation** 標準的 JSON 紀錄。

無需醫護專業背景，點選即可完成記錄。

---

## 功能

| 功能 | 說明 |
|------|------|
| 疼痛部位選擇 | 11 個身體部位（左胸、右胸、心窩、肩膀等），對應 SNOMED CT 代碼 |
| 感覺類型 | 7 種不適感（壓迫、緊縮、刺痛、燒灼等），可複選 |
| 嚴重程度滑桿 | 0–10 分 NRS 量表，轉為 FHIR `valueQuantity`，LOINC 72514-3 |
| 伴隨症狀 | 12 種症狀（喘、冷汗、心悸、暈眩等），轉為 FHIR component `valueBoolean` |
| 日常語言描述 | 自由文字，存入 FHIR `note` 欄位 |
| 中華民國身分證檢核 | 依內政部戶政司演算法即時驗證身分證字號 |
| JSON 輸出 | 語法高亮顯示，支援複製與下載 `.json` |

---

## FHIR 資源結構

```
Observation
├── status: preliminary
├── category: survey（主訴調查）
├── code: SNOMED 29857009 / LOINC 75325-1（胸部不舒服）
├── subject: Patient（含身分證號）
├── effectiveDateTime
├── note: 民眾日常語言描述
├── bodySite: SNOMED CT 身體部位代碼
└── component[]
    ├── [0] 疼痛程度 — LOINC 72514-3 → valueQuantity (0–10 {score})
    ├── [1..n] 感覺類型 — SNOMED CT → valueBoolean: true
    └── [n+1..] 伴隨症狀 — SNOMED CT → valueBoolean: true
```

---

## 隨附檔案

| 檔案 | 說明 |
|------|------|
| `index.html` | 網頁主程式（HTML + JS 單檔） |
| `chest_discomfort_fhir_example.json` | 完整 FHIR Observation 範例（55 歲男性案例） |
| `fhir_observation_fields.csv` | 28 個 FHIR 欄位定義（中文說明 + 日常語言對應） |
| `bodysite_valueset.csv` | 疼痛部位 Value Set（SNOMED CT） |
| `component_valueset.csv` | 組件 Value Set（嚴重程度 + 感覺類型 + 伴隨症狀） |

---

## 中華民國身分證檢核說明

輸入身分證號時自動驗證：

- 格式：第 1 碼英文字母 + 第 2 碼 1（男）或 2（女）+ 後 8 碼數字
- 演算法：字母轉換值加權總和 mod 10 = 0（符合內政部戶政司規範）
- 即時顯示 ✅ 格式正確 / ❌ 檢核碼不符 / ⚠️ 非身分證格式

---

## 使用流程

1. 開啟網頁：https://speedthunder.github.io/FHIR-chest-discomfort/
2. 填入基本資料（選填）
3. 選擇不舒服部位
4. 勾選感覺類型
5. 拖曳嚴重程度滑桿（0–10）
6. 勾選伴隨症狀
7. 輸入日常語言描述（選填）
8. 按「產生 FHIR Observation 記錄」
9. 複製或下載 JSON

---

## 注意事項

- 本工具產生的記錄狀態為 `preliminary`（初步），供記錄與溝通使用
- SNOMED CT 代碼為建議值，正式臨床部署前請由臨床資訊學專家驗證
- 本工具不儲存任何資料，所有操作均在瀏覽器本地執行

---

## 授權

MIT License
