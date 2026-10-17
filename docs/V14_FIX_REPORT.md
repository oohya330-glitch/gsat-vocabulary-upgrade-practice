# V14 修正紀錄與驗證狀態

本次依報告 A–D 與使用者修改授權執行；只供 v14-review-fixes／PR #1。main、合併及部署未執行。

## 結論

**尚未完成正式驗收，不建議取代 V13。** 60 組／120 題已整批校訂，240 個選項各有語境詞義、形式及專屬理由；程式模型已執行，但本版真實瀏覽器作答仍為 0/120。不得將模型結果視為 UI 通過。

## 修正與資料處理

- 第 2 組依已確認的報告 D 採 necessary → essential，兩題同步改寫；不再把 important 翻成不可或缺。
- 第 53 組重新建立 results → consequences，基本句不含目標答案；54 保留 poor sleep。
- 全部選項依句法槽位重做，同形同數；題目標準明示「最能保留基本句意思」，自然的其他意思不稱為錯誤英文。
- 原 V13／先前 V14 因選項全面變更，完整封存原始紀錄，顯示舊版完成／首次錯題摘要；新版進度另計。沒有把舊答案套入新選項重新評分。舊練寫、未知欄位、歷史均保存在封存及備份匯出。
- schema 3；每筆新紀錄有 questionId、contentRevision、選項 ID／文字快照，作答保留 optionId 及時間。
- 寫入前先另存原始資料備份；儲存失敗／衝突仍保留最新輸入。提供完整匯出、重試、損毀復原確認。
- 不再截斷 10000 字元練寫或 20 筆歷史；歷史保留 runId／時間，無時間的舊資料標未知。
- ①完成即解鎖①解析；②顯示／隱藏獨立。重練解析依本輪狀態解鎖，重新整理保留重練模式。
- 手機中文字級使用縮放變數；重繪保留詳解開闔、焦點及捲動位置。這些 UI 行為仍須瀏覽器確認。

## 問題處理表

| 編號 | 程式／教材處理 | 驗證狀態 |
|---|---|---|
| AUD-001 | 53 重建，60 題初始基本句無目標詞模型掃描通過 | 模型或來源檢查；真實 UI 未驗證 |
| AUD-002 | 02 必要性語境同步改寫，內容核對 | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-003 | 240 項加入獨立詞義／形式／理由，結構核對 | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-004 | 60 題按槽位重做干擾項；逐項內容核對 | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-005 | 中文先對應基本句，焦點差異在答案後說明 | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-006 | 120 句逐項補漏／移除額外程度，完整修訂見教材表 | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-007 | 補語義強度、正式度、搭配限制；保留自然的基本英文 | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-008 | 54 改 factor contributing to poor sleep | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-009 | 逐題詞義取代共用字典，19 展現、44 動詞、51／56 名詞 | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-010 | 24 旅遊、40 群眾恐慌、51 長者健康、56 口說學習 | 內容核對／結構模型；瀏覽器未驗證 |
| AUD-011 | 取消文字截斷，故障後繼續更新緩衝，模型含 12000 字與重試 | 模型或來源檢查；真實 UI 未驗證 |
| AUD-012 | 保留全部歷史，25 筆重新載入模型通過 | 模型或來源檢查；真實 UI 未驗證 |
| AUD-013 | 保存時間／runId，歷史清單提供完成、次數、最後結果 | 模型或來源檢查；真實 UI 未驗證 |
| AUD-014 | 封存舊版原始資料；新紀錄附內容 revision／選項快照 | 模型或來源檢查；真實 UI 未驗證 |
| AUD-015 | ①解析獨立解鎖，全部60組模型通過 | 模型或來源檢查；真實 UI 未驗證 |
| AUD-016 | 移除手機固定21px；CSS核對，視覺未驗證 | 模型或來源檢查；真實 UI 未驗證 |
| AUD-017 | 重繪恢復 details／焦點／捲動；來源核對，瀏覽器未驗證 | 模型或來源檢查；真實 UI 未驗證 |
| AUD-018 | 共用警告、匯出、重試及備份後復原；模型含取消／確認 | 模型或來源檢查；真實 UI 未驗證 |

## 測試證據與重現

執行 `node tests/regression.cjs`。測試使用 Node VM、最小 DOM／localStorage／confirm 替身，不載入真實瀏覽器。完整機器紀錄見 `tests/regression-result.json`。

- 60 組各測：首次答對、錯後答對、連錯兩次揭答、練寫解鎖／空白看答／隱藏／清字、進度、洗牌保存、重練防洩漏及首次紀錄不覆寫。
- 六回各測 0／3／10 錯題、20/20 進度、清除取消／確認。
- 額外測：12000 字、配額故障、25 筆歷史、舊版封存、損毀資料復原、兩頁模型衝突、新輪歷史、revision 變動、偏好及重練模式重新載入。

## 尚未驗證與限制

| 項目 | 狀態與原因 |
|---|---|
| 最新版本 Chrome 全120題實際操作 | 未驗證；沒有本版可用的 HTTP(S) 預覽。先前 file:// 被瀏覽器政策拒絕且禁止繞過，因此未用其他路徑規避。沒有建立部署。 |
| 原生確認、實際下載備份、焦點／閱讀位置 | 未驗證；模型只驗證分支與產出，不證明瀏覽器允許下載或視覺行為。 |
| 手機／iPad／桌面響應式視覺 | 未驗證；CSS 改動不能當截圖或真實裝置測試。 |
| Safari、觸控、軟鍵盤 | 未驗證；目前沒有真實 Safari／iPad 操作證據。 |
| 真實多分頁同步競爭 | 未驗證；模型確認一般衝突停寫與草稿保留，但 localStorage 比較後寫入不是跨頁原子交易。 |
| 儲存完全不可用、使用者關閉分頁 | 無法保證持久保存；會顯示警告、離頁提示與匯出途徑。必須實測瀏覽器權限與配額。 |
| 舊版進度 | 原成績保留為舊版摘要及原始封存，新題庫進度另計；不把未確認的舊題版本假定為新版本。 |
| 原 F01–F24 | 本次依可讀的四份 A–D、AUD-001–018 修正。沒有原 F01–F24 的完整原文可逐號重新認證，不捏造對應。 |

## 上線關卡

PR 保持草稿。完成最新 commit 的瀏覽器回歸、處理回歸發現的任何 Critical／Major，提交驗收結果後，仍須使用者明確說「同意正式上線 V14」才能合併與部署。

## 60 組／120 題修訂後教材與模型紀錄

下表的「模型通過」只代表本地事件與狀態模型；每一題的瀏覽器欄均為未驗證。參考英文是可接受答案之一，自由練寫不以字串相等評分。

### 第 01 組｜big → significant

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | This issue has a big effect on teenagers. |
| ①中文 | 這個問題對青少年有很大的影響。 |
| ①參考改寫 | This issue has a significant effect on teenagers. |
| ②中文 | 這項決定對我們的未來有重大影響。 |
| ②基本句（作答前不顯示） | This decision has a big impact on our future. |
| ②參考答案 | This decision has a significant impact on our future. |
| 用法與限制 | significant 強調影響、差異或重要性。 big effect 很自然；significant 在此強調影響程度或重要性，不表示所有 big 都能換用。 |
| 搭配 | a significant impact / a significant difference |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| significant（答案） | 重大的；形容詞 | significant 強調影響、差異或重要性。 big effect 很自然；significant 在此強調影響程度或重要性，不表示所有 big 都能換用。 本題搭配：a significant impact / a significant difference |
| minor | 輕微的；形容詞 | minor effect 指輕微影響，與 big 的程度相反。 |
| temporary | 暫時的；形容詞 | temporary 說明持續時間，沒有保留影響很大的程度。 |
| negative | 負面的；形容詞 | negative 指影響方向，big 並未限定負面。 |

### 第 02 組｜necessary → essential

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Getting enough sleep is necessary for good health. |
| ①中文 | 充足的睡眠對維持健康是必要的。 |
| ①參考改寫 | Getting enough sleep is essential for good health. |
| ②中文 | 規律練習對提升英語能力是必要的。 |
| ②基本句（作答前不顯示） | Regular practice is necessary for improving English skills. |
| ②參考答案 | Regular practice is essential for improving English skills. |
| 用法與限制 | necessary 與 essential 在此均表必要條件；essential 強調不可或缺。不要把 important（重要）一律改成 essential。 |
| 搭配 | essential to success / essential for health |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| essential（答案） | 必要的；形容詞 | necessary 與 essential 在此均表必要條件；essential 強調不可或缺。不要把 important（重要）一律改成 essential。 本題搭配：essential to success / essential for health |
| optional | 可選擇的；形容詞 | optional 表示可有可無，不保留 necessary 的必要性。 |
| irrelevant | 無關的；形容詞 | irrelevant 否定睡眠與健康的關聯。 |
| harmful | 有害的；形容詞 | harmful 表示造成傷害，與此處必要的正面條件不同。 |

### 第 03 組｜good → beneficial

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Regular exercise is good for mental health. |
| ①中文 | 規律運動對心理健康有益。 |
| ①參考改寫 | Regular exercise is beneficial to mental health. |
| ②中文 | 閱讀對學生的語言發展有益。 |
| ②基本句（作答前不顯示） | Reading is good for students’ language development. |
| ②參考答案 | Reading is beneficial to students’ language development. |
| 用法與限制 | beneficial 表示有益的，常搭配 be beneficial to。 good for 已是自然英文；beneficial to 聚焦助益，須一併調整介系詞。 |
| 搭配 | beneficial to health / beneficial effects |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| beneficial（答案） | 有益的；形容詞 | beneficial 表示有益的，常搭配 be beneficial to。 good for 已是自然英文；beneficial to 聚焦助益，須一併調整介系詞。 本題搭配：beneficial to health / beneficial effects |
| harmful | 有害的；形容詞 | harmful to 語法自然，但意思與 good for 相反。 |
| irrelevant | 無關的；形容詞 | irrelevant to 表示無關，不保留對健康有益。 |
| detrimental | 不利的；形容詞 | detrimental to 表示造成不良影響，與原句方向相反。 |

### 第 04 組｜bad → harmful

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Breathing polluted air is bad for our health. |
| ①中文 | 吸入污染空氣對健康有害。 |
| ①參考改寫 | Breathing polluted air is harmful to our health. |
| ②中文 | 長時間久坐對身體有害。 |
| ②基本句（作答前不顯示） | Sitting for long periods is bad for the body. |
| ②參考答案 | Sitting for long periods is harmful to the body. |
| 用法與限制 | harmful 強調造成傷害，常搭配 be harmful to。 bad for 在此指有害，不能把所有 bad（如品質差）都改為 harmful。 |
| 搭配 | harmful to the environment / harmful effects |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| harmful（答案） | 有害的；形容詞 | harmful 強調造成傷害，常搭配 be harmful to。 bad for 在此指有害，不能把所有 bad（如品質差）都改為 harmful。 本題搭配：harmful to the environment / harmful effects |
| beneficial | 有益的；形容詞 | beneficial to 搭配正確，但與 bad for 的負面影響相反。 |
| irrelevant | 無關的；形容詞 | irrelevant to 表示無關，原句則明說有害。 |
| vital | 至關重要的；形容詞 | vital to 表必要性，不表示吸入污染空氣造成傷害。 |

### 第 05 組｜many → numerous

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Students have many opportunities to learn new skills. |
| ①中文 | 學生有許多機會學習新技能。 |
| ①參考改寫 | Students have numerous opportunities to learn new skills. |
| ②中文 | 這個城市有許多公共圖書館。 |
| ②基本句（作答前不顯示） | This city has many public libraries. |
| ②參考答案 | This city has numerous public libraries. |
| 用法與限制 | numerous 修飾可數複數名詞。 numerous 表數量多，不適用不可數名詞；many 本身正確自然。 |
| 搭配 | numerous opportunities / numerous reasons |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| numerous（答案） | 許多的；形容詞 | numerous 修飾可數複數名詞。 numerous 表數量多，不適用不可數名詞；many 本身正確自然。 本題搭配：numerous opportunities / numerous reasons |
| limited | 有限的；形容詞 | limited opportunities 強調數量有限，不保留 many。 |
| rare | 罕見的；形容詞 | rare opportunities 強調機會少見，與 many 不同。 |
| valuable | 有價值的；形容詞 | valuable opportunities 很自然，但只談價值，不保留數量多。 |

### 第 06 組｜help → assist

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Teachers help students with problems. |
| ①中文 | 老師協助學生處理問題。 |
| ①參考改寫 | Teachers assist students with problems. |
| ②中文 | 志工協助長者進行日常活動。 |
| ②基本句（作答前不顯示） | Volunteers help older adults with daily activities. |
| ②參考答案 | Volunteers assist older adults with daily activities. |
| 用法與限制 | assist 為較正式的「協助」；assist someone with something，或 assist someone in doing something。 |
| 搭配 | Teachers assist students with problems. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| assist（答案） | 協助；原形動詞／動詞片語 | assist 為較正式的「協助」；assist someone with something，或 assist someone in doing something。 本題搭配：Teachers assist students with problems. |
| burden | 使負擔加重；原形動詞／動詞片語 | burden students with problems 表示把問題加諸學生，方向與協助相反。 |
| confront | 使面對；原形動詞／動詞片語 | confront students with problems 表使學生面對問題，不等於提供協助。 |
| provide | 提供；原形動詞／動詞片語 | provide students with problems 表提供問題，未表示協助處理。 |

### 第 07 組｜help → contribute to

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Exercise helps improve our health. |
| ①中文 | 運動有助於改善我們的健康。 |
| ①參考改寫 | Exercise contributes to improving our health. |
| ②中文 | 良好的溝通有助於建立信任。 |
| ②基本句（作答前不顯示） | Good communication helps build trust. |
| ②參考答案 | Good communication contributes to building trust. |
| 用法與限制 | contribute to 表示「有助於、促成」；to 是介系詞，後接名詞或 V-ing。 |
| 搭配 | Exercise contributes to good health. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| contributes to（答案） | 有助於；第三人稱單數動詞／動詞片語 | contribute to 表示「有助於、促成」；to 是介系詞，後接名詞或 V-ing。 本題搭配：Exercise contributes to good health. |
| interferes with | 干擾；第三人稱單數動詞／動詞片語 | interferes with good health 表負面干擾，不保留有助健康。 |
| detracts from | 減損；第三人稱單數動詞／動詞片語 | detracts from good health 表減損健康，方向相反。 |
| depends on | 依賴；第三人稱單數動詞／動詞片語 | depends on good health 表運動依賴健康，把助益關係改成前提。 |

### 第 08 組｜make → enable

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | The Internet makes it possible for us to learn anywhere. |
| ①中文 | 網路使我們能隨地學習。 |
| ①參考改寫 | The Internet enables us to learn anywhere. |
| ②中文 | 線上課程讓偏遠地區的學生能獲得教育資源。 |
| ②基本句（作答前不顯示） | Online courses make it possible for students in remote areas to access educational resources. |
| ②參考答案 | Online courses enable students in remote areas to access educational resources. |
| 用法與限制 | enable 表示「使……能夠」；常用 enable someone to do something。 |
| 搭配 | The Internet enables us to learn anywhere. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| enables（答案） | 使能夠；第三人稱單數動詞／動詞片語 | enable 表示「使……能夠」；常用 enable someone to do something。 本題搭配：The Internet enables us to learn anywhere. |
| forbids | 禁止；第三人稱單數動詞／動詞片語 | forbids us to learn 語法可行，卻是禁止而非使學習成為可能。 |
| requires | 要求；第三人稱單數動詞／動詞片語 | requires us to learn 表要求，可能但不等於提供能力或條件。 |
| persuades | 說服；第三人稱單數動詞／動詞片語 | persuades us to learn 表說服，加入原句沒有的意願改變。 |

### 第 09 組｜cause → result in

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | The storm caused serious damage. |
| ①中文 | 暴風雨造成嚴重損害。 |
| ①參考改寫 | The storm resulted in serious damage. |
| ②中文 | 缺乏睡眠可能導致注意力不集中。 |
| ②基本句（作答前不顯示） | A lack of sleep can cause poor concentration. |
| ②參考答案 | A lack of sleep can result in poor concentration. |
| 用法與限制 | result in 表示「導致、造成」；後接結果。注意 result from 表示「起因於」。 |
| 搭配 | The storm resulted in serious damage. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| resulted in（答案） | 導致；過去式動詞／動詞片語 | result in 表示「導致、造成」；後接結果。注意 result from 表示「起因於」。 本題搭配：The storm resulted in serious damage. |
| prevented | 阻止了；過去式動詞／動詞片語 | prevented serious damage 是防止損害，與 caused 相反。 |
| reduced | 減少了；過去式動詞／動詞片語 | reduced serious damage 表減少，不保留造成損害。 |
| revealed | 揭露了；過去式動詞／動詞片語 | revealed serious damage 表使損害被看見，不表示風暴造成損害。 |

### 第 10 組｜change greatly → transform

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Technology has greatly changed our lives. |
| ①中文 | 科技已大幅改變我們的生活。 |
| ①參考改寫 | Technology has transformed our lives. |
| ②中文 | 這次經驗大幅改變了她對失敗的看法。 |
| ②基本句（作答前不顯示） | The experience greatly changed her view of failure. |
| ②參考答案 | The experience transformed her view of failure. |
| 用法與限制 | transform 強調顯著改變；常見 transform lives / transform education。 transform 指顯著轉變，所以基本句加入 greatly；一般輕微 change 不宜替換。 |
| 搭配 | Technology has transformed our lives. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| transformed（答案） | 轉變了；過去分詞 | transform 強調顯著改變；常見 transform lives / transform education。 transform 指顯著轉變，所以基本句加入 greatly；一般輕微 change 不宜替換。 本題搭配：Technology has transformed our lives. |
| preserved | 保留了；過去分詞 | has preserved our lives 表保存或保全，不保留大幅改變。 |
| documented | 記錄了；過去分詞 | has documented our lives 表記錄生活，不保留改變。 |
| disrupted | 擾亂了；過去分詞 | has disrupted our lives 表負面干擾，原句大幅改變沒有指定負面。 |

### 第 11 組｜grow → increase

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | The population has grown rapidly. |
| ①中文 | 人口迅速增加。 |
| ①參考改寫 | The population has increased rapidly. |
| ②中文 | 近年來線上購物需求已有所增加。 |
| ②基本句（作答前不顯示） | Demand for online shopping has grown in recent years. |
| ②參考答案 | Demand for online shopping has increased in recent years. |
| 用法與限制 | increase 表示數量、程度增加；可作動詞或名詞。 |
| 搭配 | The population has increased rapidly. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| increased（答案） | 增加了；過去分詞 | increase 表示數量、程度增加；可作動詞或名詞。 本題搭配：The population has increased rapidly. |
| decreased | 減少了；過去分詞 | has decreased rapidly 表人口減少，與 grown 相反。 |
| stabilized | 趨於穩定；過去分詞 | has stabilized 表趨穩，不保留迅速增加。 |
| fluctuated | 波動了；過去分詞 | has fluctuated 表上下變動，不等於增加。 |

### 第 12 組｜try → attempt

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | He tried to solve the problem alone. |
| ①中文 | 他嘗試獨自解決問題。 |
| ①參考改寫 | He attempted to solve the problem alone. |
| ②中文 | 她試著在比賽前控制緊張情緒。 |
| ②基本句（作答前不顯示） | She tried to manage her nerves before the competition. |
| ②參考答案 | She attempted to manage her nerves before the competition. |
| 用法與限制 | attempt to do something 表示「嘗試做某事」，語氣比 try 稍正式。 attempt 與 try 都不保證成功；manage to 才表示成功做到。 |
| 搭配 | He attempted to solve the problem alone. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| attempted（答案） | 嘗試了；過去式動詞／動詞片語 | attempt to do something 表示「嘗試做某事」，語氣比 try 稍正式。 attempt 與 try 都不保證成功；manage to 才表示成功做到。 本題搭配：He attempted to solve the problem alone. |
| refused | 拒絕了；過去式動詞／動詞片語 | refused to solve 表不願做，不保留嘗試。 |
| managed | 設法做到了；過去式動詞／動詞片語 | managed to solve 表成功做到，tried 未保證成功。 |
| pretended | 假裝了；過去式動詞／動詞片語 | pretended to solve 表假裝解決，與認真嘗試不同。 |

### 第 13 組｜choose → select

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Students can choose courses based on their interests. |
| ①中文 | 學生可以根據自己的興趣選擇課程。 |
| ①參考改寫 | Students can select courses based on their interests. |
| ②中文 | 請從清單中選出三項活動。 |
| ②基本句（作答前不顯示） | Please choose three activities from the list. |
| ②參考答案 | Please select three activities from the list. |
| 用法與限制 | select 表示選擇、挑選；常用 select courses / select an option。 |
| 搭配 | Students can select courses based on their interests. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| select（答案） | 挑選；原形動詞／動詞片語 | select 表示選擇、挑選；常用 select courses / select an option。 本題搭配：Students can select courses based on their interests. |
| reject | 拒選；原形動詞／動詞片語 | reject courses 表排除課程，不是選取課程。 |
| design | 設計；原形動詞／動詞片語 | design courses 表設計內容，不是依興趣選課。 |
| advertise | 宣傳；原形動詞／動詞片語 | advertise courses 表推廣課程，不保留選擇行為。 |

### 第 14 組｜use → utilize

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | We should use available resources effectively. |
| ①中文 | 我們應有效運用現有資源。 |
| ①參考改寫 | We should utilize available resources effectively. |
| ②中文 | 我們可以利用科技提高學習效率。 |
| ②基本句（作答前不顯示） | We can use technology to improve learning efficiency. |
| ②參考答案 | We can utilize technology to improve learning efficiency. |
| 用法與限制 | utilize 表示運用資源或方法；不必在所有情況下取代 use。 use 已自然簡潔；utilize 在運用資源時可用，較正式不代表更好。 |
| 搭配 | We should utilize available resources effectively. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| utilize（答案） | 運用；原形動詞／動詞片語 | utilize 表示運用資源或方法；不必在所有情況下取代 use。 use 已自然簡潔；utilize 在運用資源時可用，較正式不代表更好。 本題搭配：We should utilize available resources effectively. |
| ignore | 忽略；原形動詞／動詞片語 | ignore resources 表不理會資源，不保留有效使用。 |
| distribute | 分配；原形動詞／動詞片語 | distribute resources 表分配資源，並未表示使用資源。 |
| evaluate | 評估；原形動詞／動詞片語 | evaluate resources 表評估，不保留 use 的實際運用。 |

### 第 15 組｜think → believe

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | I think effort leads to success. |
| ①中文 | 我認為努力能帶來成功。 |
| ①參考改寫 | I believe effort leads to success. |
| ②中文 | 我認為每個人都應該受到公平對待。 |
| ②基本句（作答前不顯示） | I think everyone deserves fair treatment. |
| ②參考答案 | I believe everyone deserves fair treatment. |
| 用法與限制 | believe that ... 可用於提出個人看法或信念。 believe 較突出信念，think 可較保留；本題保留認同立場但確信程度可能不同。 |
| 搭配 | I believe effort leads to success. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| believe（答案） | 相信；原形動詞／動詞片語 | believe that ... 可用於提出個人看法或信念。 believe 較突出信念，think 可較保留；本題保留認同立場但確信程度可能不同。 本題搭配：I believe effort leads to success. |
| doubt | 懷疑；原形動詞／動詞片語 | doubt effort leads to success 表懷疑此命題，與 think 在此表認同不同。 |
| deny | 否認；原形動詞／動詞片語 | deny effort leads to success 否定此命題，原句為正面看法。 |
| fear | 擔心；原形動詞／動詞片語 | fear effort leads to success 加入對成功的擔憂，不保留原句立場。 |

### 第 16 組｜think → argue

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Some people think school uniforms are necessary. |
| ①中文 | 有些人認為校服是必要的。 |
| ①參考改寫 | Some people argue that school uniforms are necessary. |
| ②中文 | 有些教育工作者認為學生需要更多休息時間。 |
| ②基本句（作答前不顯示） | Some educators think students need more time to rest. |
| ②參考答案 | Some educators argue that students need more time to rest. |
| 用法與限制 | argue that ... 用來提出論點或主張，不只是「想一想」。 think 只表看法；argue that 加入提出或支持論點的行為。本題練習表達主張，不宣稱完全等義。 |
| 搭配 | Some people argue that school uniforms are necessary. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| argue（答案） | 主張；爭論；原形動詞／動詞片語 | argue that ... 用來提出論點或主張，不只是「想一想」。 think 只表看法；argue that 加入提出或支持論點的行為。本題練習表達主張，不宣稱完全等義。 本題搭配：Some people argue that school uniforms are necessary. |
| deny | 否認；原形動詞／動詞片語 | deny that 子句否定必要性，與認為必要的立場相反。 |
| doubt | 懷疑；原形動詞／動詞片語 | doubt that 子句表不確定或不相信，不保留原句的認同立場。 |
| regret | 後悔；遺憾；原形動詞／動詞片語 | regret that 子句可自然成立，但加入遺憾，不是提出校服必要的論點。 |

### 第 17 組｜think → reflect on

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | We should think carefully about our choices. |
| ①中文 | 我們應仔細反思自己的選擇。 |
| ①參考改寫 | We should reflect on our choices carefully. |
| ②中文 | 寫日記讓她能反思自己的進步。 |
| ②基本句（作答前不顯示） | Keeping a journal helps her think about her progress. |
| ②參考答案 | Keeping a journal helps her reflect on her progress. |
| 用法與限制 | reflect on 表示回顧、反思；常用 reflect on choices / mistakes。 reflect on 重回顧與反思，比一般 think about 更深入；注意 on 後接名詞或動名詞。 |
| 搭配 | We should reflect on our choices carefully. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| reflect on（答案） | 反思；原形動詞／動詞片語 | reflect on 表示回顧、反思；常用 reflect on choices / mistakes。 reflect on 重回顧與反思，比一般 think about 更深入；注意 on 後接名詞或動名詞。 本題搭配：We should reflect on our choices carefully. |
| complain about | 抱怨；原形動詞／動詞片語 | complain about our choices 加入不滿，think carefully 不一定負面。 |
| boast about | 誇耀；原形動詞／動詞片語 | boast about our choices 加入自誇，不是仔細思考。 |
| forget about | 忘記；原形動詞／動詞片語 | forget about our choices 表忘記，不保留反思。 |

### 第 18 組｜know → be aware of

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | We should know about the risks. |
| ①中文 | 我們應了解風險。 |
| ①參考改寫 | We should be aware of the risks. |
| ②中文 | 離開前，我們必須了解天氣方面的風險。 |
| ②基本句（作答前不顯示） | We must know about the weather risks before leaving. |
| ②參考答案 | We must be aware of the weather risks before leaving. |
| 用法與限制 | be aware of 表示察覺或意識到；of 後接名詞或 V-ing。 |
| 搭配 | We should be aware of the risks. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| be aware of（答案） | 意識到；原形動詞／動詞片語 | be aware of 表示察覺或意識到；of 後接名詞或 V-ing。 本題搭配：We should be aware of the risks. |
| be ignorant of | 不知道；原形動詞／動詞片語 | be ignorant of the risks 表不了解，與 know about 相反。 |
| be indifferent to | 漠不關心；原形動詞／動詞片語 | be indifferent to the risks 表態度冷漠，不保留知悉風險。 |
| be responsible for | 對……負責；原形動詞／動詞片語 | be responsible for the risks 表責任歸屬，不等於知道風險。 |

### 第 19 組｜show → demonstrate

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | His actions showed a strong sense of responsibility. |
| ①中文 | 他的行動展現了強烈的責任感。 |
| ①參考改寫 | His actions demonstrated a strong sense of responsibility. |
| ②中文 | 研究結果顯示睡眠與記憶力之間的關聯。 |
| ②基本句（作答前不顯示） | The findings showed a link between sleep and memory. |
| ②參考答案 | The findings demonstrated a link between sleep and memory. |
| 用法與限制 | demonstrate 表示清楚展現或證明；常見 demonstrate responsibility。 此處 demonstrated 是「展現了」責任感；研究句的 demonstrated a link 表顯示關聯，不等於證明因果。 |
| 搭配 | His actions demonstrated a strong sense of responsibility. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| demonstrated（答案） | 展現了；過去式動詞／動詞片語 | demonstrate 表示清楚展現或證明；常見 demonstrate responsibility。 此處 demonstrated 是「展現了」責任感；研究句的 demonstrated a link 表顯示關聯，不等於證明因果。 本題搭配：His actions demonstrated a strong sense of responsibility. |
| concealed | 掩藏了；過去式動詞／動詞片語 | concealed a strong sense of responsibility 表掩藏，與 showed 相反。 |
| contradicted | 與……矛盾；過去式動詞／動詞片語 | contradicted a strong sense of responsibility 表行動與責任感相矛盾，不是展現。 |
| undermined | 削弱了；過去式動詞／動詞片語 | undermined a strong sense of responsibility 表削弱，不保留展現。 |

### 第 20 組｜chance → opportunity

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | She got the chance to study abroad. |
| ①中文 | 她獲得了出國留學的機會。 |
| ①參考改寫 | She got the opportunity to study abroad. |
| ②中文 | 這份實習讓她有機會學習新技能。 |
| ②基本句（作答前不顯示） | The internship gave her a chance to learn new skills. |
| ②參考答案 | The internship gave her an opportunity to learn new skills. |
| 用法與限制 | opportunity 表示機會；常用 an opportunity to do something。 |
| 搭配 | She got the opportunity to study abroad. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| opportunity（答案） | 機會；單數名詞 | opportunity 表示機會；常用 an opportunity to do something。 本題搭配：She got the opportunity to study abroad. |
| obligation | 義務；單數名詞 | obligation to study abroad 語法自然，但把機會改成必須做的義務。 |
| permission | 許可；單數名詞 | permission to study abroad 指獲准，不等於獲得實際機會。 |
| motivation | 動機；單數名詞 | motivation to study abroad 指想做的動力，不保留獲得機會。 |

### 第 21 組｜problem → issue

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Environmental problems are becoming serious. |
| ①中文 | 環境問題正變得嚴重。 |
| ①參考改寫 | Environmental issues are becoming serious. |
| ②中文 | 心理健康不佳是學校應關注的問題。 |
| ②基本句（作答前不顯示） | Poor mental health is a problem that schools should pay attention to. |
| ②參考答案 | Poor mental health is an issue that schools should pay attention to. |
| 用法與限制 | issue 可表示值得討論或處理的問題；常見 environmental issues。 |
| 搭配 | Environmental issues are becoming serious. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| issues（答案） | 議題；問題；複數名詞 | issue 可表示值得討論或處理的問題；常見 environmental issues。 本題搭配：Environmental issues are becoming serious. |
| benefits | 益處；複數名詞 | environmental benefits 指環境益處，與 problems 的負面問題不同。 |
| opportunities | 機會；複數名詞 | environmental opportunities 指環境相關機會，不保留問題。 |
| policies | 政策；複數名詞 | environmental policies 是政策，與待處理的問題不是同一概念。 |

### 第 22 組｜start → initiate

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | The school started a recycling program. |
| ①中文 | 學校啟動回收計畫。 |
| ①參考改寫 | The school initiated a recycling program. |
| ②中文 | 社區發起了一項節能活動。 |
| ②基本句（作答前不顯示） | The community started an energy-saving campaign. |
| ②參考答案 | The community initiated an energy-saving campaign. |
| 用法與限制 | initiate 表示發起、啟動；常見 initiate a program / campaign。 |
| 搭配 | The school initiated a recycling program. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| initiated（答案） | 發起了；過去式動詞／動詞片語 | initiate 表示發起、啟動；常見 initiate a program / campaign。 本題搭配：The school initiated a recycling program. |
| terminated | 終止了；過去式動詞／動詞片語 | terminated a recycling program 是結束，不是開始。 |
| evaluated | 評估了；過去式動詞／動詞片語 | evaluated a recycling program 是評估，未表示發起。 |
| financed | 資助了；過去式動詞／動詞片語 | financed a recycling program 是出錢支持，不必然是啟動者。 |

### 第 23 組｜do → perform

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Students do various tasks every day. |
| ①中文 | 學生每天執行各種任務。 |
| ①參考改寫 | Students perform various tasks every day. |
| ②中文 | 醫護人員在緊急情況下履行重要職責。 |
| ②基本句（作答前不顯示） | Medical workers carry out important duties during emergencies. |
| ②參考答案 | Medical workers perform important duties during emergencies. |
| 用法與限制 | perform 表示執行任務、履行職責；常見 perform tasks / duties。 |
| 搭配 | Students perform various tasks every day. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| perform（答案） | 執行；表演；原形動詞／動詞片語 | perform 表示執行任務、履行職責；常見 perform tasks / duties。 本題搭配：Students perform various tasks every day. |
| postpone | 延後；原形動詞／動詞片語 | postpone tasks 是延後任務，並非執行。 |
| assign | 指派；原形動詞／動詞片語 | assign tasks 是分派任務，未表示學生自己執行。 |
| avoid | 避開；原形動詞／動詞片語 | avoid tasks 是迴避任務，與 do 相反。 |

### 第 24 組｜say → remark

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | He said that the plan was excellent. |
| ①中文 | 他說這項計畫很棒。 |
| ①參考改寫 | He remarked that the plan was excellent. |
| ②中文 | 參觀結束後，這位遊客說博物館的安排很完善。 |
| ②基本句（作答前不顯示） | After the tour, the visitor said that the museum was well organized. |
| ②參考答案 | After the tour, the visitor remarked that the museum was well organized. |
| 用法與限制 | remark that ... 表示評論、提及；比 say 更強調發表意見。 said 只表說出；remarked 加入評論或提及的語氣。noticed / considered / discovered 接 that 子句可自然，但不保留發言。 |
| 搭配 | He remarked that the plan was excellent. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| remarked（答案） | 評論了；過去式動詞／動詞片語 | remark that ... 表示評論、提及；比 say 更強調發表意見。 said 只表說出；remarked 加入評論或提及的語氣。noticed / considered / discovered 接 that 子句可自然，但不保留發言。 本題搭配：He remarked that the plan was excellent. |
| noticed | 注意到了；過去式動詞／動詞片語 | noticed that 可自然接子句，但只表注意到，不保留 said 的發言行為。 |
| considered | 認為了；過去式動詞／動詞片語 | considered that 可表認為，但沒有保留說出口這件事。 |
| discovered | 發現了；過去式動詞／動詞片語 | discovered that 表發現事實，不表示發言，且加入獲知資訊。 |

### 第 25 組｜tell → inform

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Teachers tell students about the changes. |
| ①中文 | 老師告知學生這些變動。 |
| ①參考改寫 | Teachers inform students of the changes. |
| ②中文 | 學校將告知家長新的活動時程。 |
| ②基本句（作答前不顯示） | The school will tell parents about the new event schedule. |
| ②參考答案 | The school will inform parents of the new event schedule. |
| 用法與限制 | inform someone of/about something 表示通知某人某事。 inform 是告知；remind 預設對方先前知道，warn 帶有警告性，不能任意互換。 |
| 搭配 | Teachers inform students of the changes. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| inform（答案） | 告知；原形動詞／動詞片語 | inform someone of/about something 表示通知某人某事。 inform 是告知；remind 預設對方先前知道，warn 帶有警告性，不能任意互換。 本題搭配：Teachers inform students of the changes. |
| remind | 提醒；原形動詞／動詞片語 | remind students of changes 表喚回已知資訊；tell about 未預設學生先前知道。 |
| convince | 使信服；原形動詞／動詞片語 | convince students of changes 表使人相信，不是單純告知變動。 |
| warn | 警告；原形動詞／動詞片語 | warn students of changes 加入危險或不良後果的警告語氣，原句未交代。 |

### 第 26 組｜very important → crucial

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Education plays a very important role in our lives. |
| ①中文 | 教育在我們的生活中扮演非常重要的角色。 |
| ①參考改寫 | Education plays a crucial role in our lives. |
| ②中文 | 家長的支持在孩子的成長中扮演非常重要的角色。 |
| ②基本句（作答前不顯示） | Parental support plays a very important role in children's development. |
| ②參考答案 | Parental support plays a crucial role in children's development. |
| 用法與限制 | crucial 表示關鍵、極重要；常見 play a crucial role in。 crucial 帶關鍵性，比一般 important 強；此處 very important 提供高度重要語境。 |
| 搭配 | Education plays a crucial role in our lives. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| crucial（答案） | 關鍵的；形容詞 | crucial 表示關鍵、極重要；常見 play a crucial role in。 crucial 帶關鍵性，比一般 important 強；此處 very important 提供高度重要語境。 本題搭配：Education plays a crucial role in our lives. |
| minor | 次要的；形容詞 | minor role 與 very important 的重要程度相反。 |
| decorative | 裝飾性的；形容詞 | decorative role 表裝飾作用，不保留生活中的重要角色。 |
| temporary | 暫時的；形容詞 | temporary role 說明時間長短，不表示重要程度。 |

### 第 27 組｜serious → severe

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Air pollution has become a serious problem. |
| ①中文 | 空氣污染已成為嚴重的問題。 |
| ①參考改寫 | Air pollution has become a severe problem. |
| ②中文 | 颱風對沿海地區造成嚴重損害。 |
| ②基本句（作答前不顯示） | The typhoon caused serious damage to coastal areas. |
| ②參考答案 | The typhoon caused severe damage to coastal areas. |
| 用法與限制 | severe 表示嚴重的；常搭配 pollution、damage、shortages。 |
| 搭配 | Air pollution has become a severe problem. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| severe（答案） | 嚴重的；形容詞 | severe 表示嚴重的；常搭配 pollution、damage、shortages。 本題搭配：Air pollution has become a severe problem. |
| minor | 輕微的；形容詞 | minor problem 表問題不大，與 serious 相反。 |
| imaginary | 想像的；形容詞 | imaginary problem 表並非真實問題，不保留嚴重程度。 |
| temporary | 暫時的；形容詞 | temporary problem 表時間短暫，未保留嚴重性。 |

### 第 28 組｜difficult → challenging

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Learning a new language can be difficult. |
| ①中文 | 學習一門新語言可能很困難。 |
| ①參考改寫 | Learning a new language can be challenging. |
| ②中文 | 快速適應新環境可能很困難。 |
| ②基本句（作答前不顯示） | Adjusting to a new environment quickly can be difficult. |
| ②參考答案 | Adjusting to a new environment quickly can be challenging. |
| 用法與限制 | challenging 表示有挑戰性的；可形容任務或學習經驗。 challenging 帶需要努力、可接受挑戰的視角；difficult 只指困難，兩者評價色彩不完全相同。 |
| 搭配 | Learning a new language can be challenging. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| challenging（答案） | 具挑戰性的；形容詞 | challenging 表示有挑戰性的；可形容任務或學習經驗。 challenging 帶需要努力、可接受挑戰的視角；difficult 只指困難，兩者評價色彩不完全相同。 本題搭配：Learning a new language can be challenging. |
| effortless | 不費力的；形容詞 | effortless 表毫不費力，與 difficult 相反。 |
| pointless | 無意義的；形容詞 | pointless 評價是否有意義，不等於困難。 |
| routine | 例行的；形容詞 | routine 指常規，未保留學習難度。 |

### 第 29 組｜useful → valuable

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Volunteering provides useful experience. |
| ①中文 | 志工服務提供有用的經驗。 |
| ①參考改寫 | Volunteering provides valuable experience. |
| ②中文 | 老師的回饋提供了有用的指引。 |
| ②基本句（作答前不顯示） | The teacher's feedback offered useful guidance. |
| ②參考答案 | The teacher's feedback offered valuable guidance. |
| 用法與限制 | valuable 表示有價值的；常搭配 experience / advice。 useful 強調實際用途；valuable 強調價值。此處同指有幫助的經驗，但焦點不同。 |
| 搭配 | Volunteering provides valuable experience. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| valuable（答案） | 有價值的；形容詞 | valuable 表示有價值的；常搭配 experience / advice。 useful 強調實際用途；valuable 強調價值。此處同指有幫助的經驗，但焦點不同。 本題搭配：Volunteering provides valuable experience. |
| useless | 無用的；形容詞 | useless experience 與 useful 相反。 |
| unpleasant | 不愉快的；形容詞 | unpleasant experience 談感受，不保留實用價值。 |
| irrelevant | 不相關的；形容詞 | irrelevant experience 指不相關，不保留對人有用。 |

### 第 30 組｜enough → sufficient

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Students need enough sleep. |
| ①中文 | 學生需要充足睡眠。 |
| ①參考改寫 | Students need sufficient sleep. |
| ②中文 | 社區沒有足夠的乾淨飲用水。 |
| ②基本句（作答前不顯示） | The community does not have enough clean drinking water. |
| ②參考答案 | The community does not have sufficient clean drinking water. |
| 用法與限制 | sufficient 表示足夠的；常搭配 sleep、time、resources。 |
| 搭配 | Students need sufficient sleep. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| sufficient（答案） | 足夠的；形容詞 | sufficient 表示足夠的；常搭配 sleep、time、resources。 本題搭配：Students need sufficient sleep. |
| insufficient | 不足的；形容詞 | insufficient sleep 表睡眠不足，與 enough 相反。 |
| interrupted | 中斷的；形容詞 | interrupted sleep 談連續性，不是睡眠量足夠。 |
| restless | 不安穩的；形容詞 | restless sleep 談睡眠品質，不保留足量。 |

### 第 31 組｜clear → evident

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | The benefits of exercise are clear. |
| ①中文 | 運動的益處很明顯。 |
| ①參考改寫 | The benefits of exercise are evident. |
| ②中文 | 結果明顯顯示這項方法有效。 |
| ②基本句（作答前不顯示） | It is clear from the results that the method works. |
| ②參考答案 | It is evident from the results that the method works. |
| 用法與限制 | evident 表示明顯的；常見 be evident / become evident。 |
| 搭配 | The benefits of exercise are evident. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| evident（答案） | 明顯的；形容詞 | evident 表示明顯的；常見 be evident / become evident。 本題搭配：The benefits of exercise are evident. |
| hidden | 隱藏的；形容詞 | hidden benefits 指不易看見，與 clear 相反。 |
| uncertain | 不確定的；形容詞 | uncertain benefits 表益處不確定，與明確不同。 |
| temporary | 暫時的；形容詞 | temporary benefits 表持續時間短，未保留可清楚辨識。 |

### 第 32 組｜possible → feasible

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | It is possible to carry out this plan. |
| ①中文 | 這項計畫可以實行。 |
| ①參考改寫 | This plan is feasible. |
| ②中文 | 即使預算有限，這項提案仍可以實行。 |
| ②基本句（作答前不顯示） | It is still possible to carry out the proposal with a limited budget. |
| ②參考答案 | The proposal is still feasible with a limited budget. |
| 用法與限制 | feasible 表示實際可行；常用於計畫、方案。 feasible 指現實條件下可實行，不等於任何抽象的 possible。 |
| 搭配 | This plan is feasible. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| feasible（答案） | 可行的；形容詞 | feasible 表示實際可行；常用於計畫、方案。 feasible 指現實條件下可實行，不等於任何抽象的 possible。 本題搭配：This plan is feasible. |
| impractical | 不切實際的；形容詞 | impractical plan 表難以實行，與可執行相反。 |
| compulsory | 強制的；形容詞 | compulsory plan 談是否強制，不表示可行。 |
| controversial | 有爭議的；形容詞 | controversial plan 談爭議，不保留能否實施。 |

### 第 33 組｜common → widespread

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Smartphone use is common among teenagers. |
| ①中文 | 青少年使用智慧型手機很普遍。 |
| ①參考改寫 | Smartphone use is widespread among teenagers. |
| ②中文 | 網路霸凌在許多社群平台上相當普遍。 |
| ②基本句（作答前不顯示） | Cyberbullying is common on many social media platforms. |
| ②參考答案 | Cyberbullying is widespread on many social media platforms. |
| 用法與限制 | widespread 表示廣泛存在、普遍的；常搭配 use / concern。 |
| 搭配 | Smartphone use is widespread among teenagers. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| widespread（答案） | 廣泛的；形容詞 | widespread 表示廣泛存在、普遍的；常搭配 use / concern。 本題搭配：Smartphone use is widespread among teenagers. |
| rare | 罕見的；形容詞 | rare among teenagers 表少見，與 common 相反。 |
| restricted | 受限制的；形容詞 | restricted among teenagers 表使用受限，不是普遍。 |
| unpopular | 不受歡迎的；形容詞 | unpopular among teenagers 談喜好，不等同普及程度。 |

### 第 34 組｜different → diverse

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Students come from different backgrounds. |
| ①中文 | 學生來自不同背景。 |
| ①參考改寫 | Students come from diverse backgrounds. |
| ②中文 | 團隊成員有不同觀點。 |
| ②基本句（作答前不顯示） | Team members have different perspectives. |
| ②參考答案 | Team members have diverse perspectives. |
| 用法與限制 | diverse 表示多元、多樣；常見 diverse backgrounds。 different 只說彼此不同；diverse 強調種類多樣，屬焦點加強，不是所有不同都多元。 |
| 搭配 | Students come from diverse backgrounds. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| diverse（答案） | 多樣的；形容詞 | diverse 表示多元、多樣；常見 diverse backgrounds。 different 只說彼此不同；diverse 強調種類多樣，屬焦點加強，不是所有不同都多元。 本題搭配：Students come from diverse backgrounds. |
| identical | 相同的；形容詞 | identical backgrounds 與 different 相反。 |
| privileged | 優渥的；形容詞 | privileged backgrounds 談社經條件，不保留背景差異。 |
| academic | 學術的；形容詞 | academic backgrounds 談背景類型，未保留多樣程度。 |

### 第 35 組｜lasting → long-lasting

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Reading can have lasting benefits. |
| ①中文 | 閱讀可以帶來持久的益處。 |
| ①參考改寫 | Reading can have long-lasting benefits. |
| ②中文 | 這項計畫對社區帶來了持久的正面影響。 |
| ②基本句（作答前不顯示） | The project had a lasting positive effect on the community. |
| ②參考答案 | The project had a long-lasting positive effect on the community. |
| 用法與限制 | lasting 和 long-lasting 都是自然表達；long-lasting 強調持續很久，並非一定比 lasting 高級。 lasting 本身自然；long-lasting 同樣表持久，並非一定更高級。 |
| 搭配 | Reading can have long-lasting benefits. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| long-lasting（答案） | 持久的；形容詞 | lasting 和 long-lasting 都是自然表達；long-lasting 強調持續很久，並非一定比 lasting 高級。 lasting 本身自然；long-lasting 同樣表持久，並非一定更高級。 本題搭配：Reading can have long-lasting benefits. |
| short-lived | 短暫的；形容詞 | short-lived benefits 與 lasting 的持續性相反。 |
| immediate | 立即的；形容詞 | immediate benefits 說明何時開始，不保留持續很久。 |
| financial | 財務上的；形容詞 | financial benefits 限定金錢領域，原句未限定。 |

### 第 36 組｜gain → acquire

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Students can gain knowledge through reading. |
| ①中文 | 學生可以透過閱讀獲取知識。 |
| ①參考改寫 | Students can acquire knowledge through reading. |
| ②中文 | 孩子能透過遊戲獲得新的社交技能。 |
| ②基本句（作答前不顯示） | Children can get new social skills through play. |
| ②參考答案 | Children can acquire new social skills through play. |
| 用法與限制 | acquire 表示獲得知識或技能；常見 acquire knowledge / skills。 acquire knowledge / skills 常指逐步習得；不宜把每個 get 都換 acquire。 |
| 搭配 | Students can acquire knowledge through reading. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| acquire（答案） | 取得；習得；原形動詞／動詞片語 | acquire 表示獲得知識或技能；常見 acquire knowledge / skills。 acquire knowledge / skills 常指逐步習得；不宜把每個 get 都換 acquire。 本題搭配：Students can acquire knowledge through reading. |
| forget | 忘記；原形動詞／動詞片語 | forget knowledge 表遺忘，不保留獲取。 |
| test | 測試；原形動詞／動詞片語 | test knowledge 表檢驗已知內容，不是習得。 |
| display | 展現；原形動詞／動詞片語 | display knowledge 表展現所知，未表示增加知識。 |

### 第 37 組｜get → obtain

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | We can get information online. |
| ①中文 | 我們可以在線上取得資訊。 |
| ①參考改寫 | We can obtain information online. |
| ②中文 | 申請人必須先取得家長同意。 |
| ②基本句（作答前不顯示） | Applicants must get parental permission first. |
| ②參考答案 | Applicants must obtain parental permission first. |
| 用法與限制 | obtain 表示取得；常見 obtain information / permission。 obtain information / permission 是取得；observe 是觀察或遵守，並非 obtain 的同義詞。 |
| 搭配 | We can obtain information online. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| obtain（答案） | 取得；原形動詞／動詞片語 | obtain 表示取得；常見 obtain information / permission。 obtain information / permission 是取得；observe 是觀察或遵守，並非 obtain 的同義詞。 本題搭配：We can obtain information online. |
| conceal | 隱藏；原形動詞／動詞片語 | conceal information 表隱藏，不是取得。 |
| distort | 扭曲；原形動詞／動詞片語 | distort information 表改變真實性，不保留 get。 |
| verify | 查證；原形動詞／動詞片語 | verify information 表查核資訊，未保留取得資訊的行為。 |

### 第 38 組｜make better → enhance

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Exercise can make our quality of life better. |
| ①中文 | 運動能提升我們的生活品質。 |
| ①參考改寫 | Exercise can enhance our quality of life. |
| ②中文 | 閱讀不同觀點可以提升我們的理解。 |
| ②基本句（作答前不顯示） | Reading different viewpoints can improve our understanding. |
| ②參考答案 | Reading different viewpoints can enhance our understanding. |
| 用法與限制 | enhance 表示提升品質或能力；常見 enhance quality of life。 |
| 搭配 | Exercise can enhance our quality of life. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| enhance（答案） | 提升；原形動詞／動詞片語 | enhance 表示提升品質或能力；常見 enhance quality of life。 本題搭配：Exercise can enhance our quality of life. |
| diminish | 降低；原形動詞／動詞片語 | diminish quality of life 表降低，與 make better 相反。 |
| measure | 測量；原形動詞／動詞片語 | measure quality of life 表測量，未保留改善。 |
| predict | 預測；原形動詞／動詞片語 | predict quality of life 表預測，未保留提升。 |

### 第 39 組｜make worse → worsen

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Stress may make existing health problems worse. |
| ①中文 | 壓力可能使健康問題惡化。 |
| ①參考改寫 | Stress may worsen existing health problems. |
| ②中文 | 不實資訊可能使社會分歧惡化。 |
| ②基本句（作答前不顯示） | False information may make social divisions worse. |
| ②參考答案 | False information may worsen social divisions. |
| 用法與限制 | worsen 表示使情況惡化；常見 worsen a problem。 |
| 搭配 | Stress may worsen existing health problems. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| worsen（答案） | 惡化；原形動詞／動詞片語 | worsen 表示使情況惡化；常見 worsen a problem。 本題搭配：Stress may worsen existing health problems. |
| alleviate | 減輕；原形動詞／動詞片語 | alleviate health problems 表減輕，與 make worse 相反。 |
| conceal | 掩蓋；原形動詞／動詞片語 | conceal health problems 表隱藏，不表示更嚴重。 |
| detect | 偵測；原形動詞／動詞片語 | detect health problems 表發現問題，未表示惡化。 |

### 第 40 組｜cause → trigger

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | The news caused a heated discussion. |
| ①中文 | 這則新聞引發了激烈的討論。 |
| ①參考改寫 | The news triggered a heated discussion. |
| ②中文 | 突如其來的聲響在人群中造成恐慌。 |
| ②基本句（作答前不顯示） | The sudden noise caused panic among the crowd. |
| ②參考答案 | The sudden noise triggered panic among the crowd. |
| 用法與限制 | trigger 表示引發事件或反應；常見 trigger a discussion。 |
| 搭配 | The news triggered a heated discussion. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| triggered（答案） | 引發了；過去式動詞／動詞片語 | trigger 表示引發事件或反應；常見 trigger a discussion。 本題搭配：The news triggered a heated discussion. |
| prevented | 阻止了；過去式動詞／動詞片語 | prevented a discussion 表阻止，不是引起。 |
| summarized | 摘要了；過去式動詞／動詞片語 | summarized a discussion 表摘要既有討論，不表示造成討論。 |
| interrupted | 打斷了；過去式動詞／動詞片語 | interrupted a discussion 表中斷，不保留引起討論。 |

### 第 41 組｜reduce → minimize

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | We should reduce unnecessary waste as much as possible. |
| ①中文 | 我們應盡量減少不必要的浪費。 |
| ①參考改寫 | We should minimize unnecessary waste. |
| ②中文 | 提早規劃可以盡量減少不必要的開支。 |
| ②基本句（作答前不顯示） | Planning ahead can reduce unnecessary expenses as much as possible. |
| ②參考答案 | Planning ahead can minimize unnecessary expenses. |
| 用法與限制 | minimize 表示盡量降低；常見 minimize waste / risks。 |
| 搭配 | We should minimize unnecessary waste. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| minimize（答案） | 盡量減少；原形動詞／動詞片語 | minimize 表示盡量降低；常見 minimize waste / risks。 本題搭配：We should minimize unnecessary waste. |
| maximize | 使最大化；原形動詞／動詞片語 | maximize waste 與盡量減少相反。 |
| classify | 分類；原形動詞／動詞片語 | classify waste 是分類，不保留數量盡量降低。 |
| measure | 測量；原形動詞／動詞片語 | measure waste 是測量，未表示減少。 |

### 第 42 組｜follow → maintain

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | It is important to follow a healthy lifestyle. |
| ①中文 | 遵循健康的生活方式很重要。 |
| ①參考改寫 | It is important to maintain a healthy lifestyle. |
| ②中文 | 即使忙碌，我們也應保持規律的作息時間。 |
| ②基本句（作答前不顯示） | We should keep a regular sleep schedule even when we are busy. |
| ②參考答案 | We should maintain a regular sleep schedule even when we are busy. |
| 用法與限制 | maintain 表示維持；常見 maintain a healthy lifestyle。 follow 指遵循，maintain 指持續維持；本題比較健康生活方式的實行與延續，不是完全等義。 |
| 搭配 | It is important to maintain a healthy lifestyle. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| maintain（答案） | 維持；原形動詞／動詞片語 | maintain 表示維持；常見 maintain a healthy lifestyle。 follow 指遵循，maintain 指持續維持；本題比較健康生活方式的實行與延續，不是完全等義。 本題搭配：It is important to maintain a healthy lifestyle. |
| abandon | 放棄；原形動詞／動詞片語 | abandon a healthy lifestyle 是放棄，不保留遵循。 |
| describe | 描述；原形動詞／動詞片語 | describe a healthy lifestyle 是描述，未表示實行或維持。 |
| recommend | 推薦；原形動詞／動詞片語 | recommend a healthy lifestyle 是建議別人採行，不表示自己遵循。 |

### 第 43 組｜face → encounter

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Teenagers often face difficulties. |
| ①中文 | 青少年經常面對困難。 |
| ①參考改寫 | Teenagers often encounter difficulties. |
| ②中文 | 旅行時我們可能面對意想不到的挑戰。 |
| ②基本句（作答前不顯示） | We may face unexpected challenges while traveling. |
| ②參考答案 | We may encounter unexpected challenges while traveling. |
| 用法與限制 | encounter 表示遇到問題或困難；常見 encounter difficulties。 face 指面對，encounter 偏遭遇；主動面對與偶然遇到的焦點可不同。 |
| 搭配 | Teenagers often encounter difficulties. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| encounter（答案） | 遭遇；原形動詞／動詞片語 | encounter 表示遇到問題或困難；常見 encounter difficulties。 face 指面對，encounter 偏遭遇；主動面對與偶然遇到的焦點可不同。 本題搭配：Teenagers often encounter difficulties. |
| avoid | 避開；原形動詞／動詞片語 | avoid difficulties 表避開，face 則面對；不能保留事件關係。 |
| predict | 預測；原形動詞／動詞片語 | predict difficulties 是事前預測，不是實際遭遇。 |
| create | 造成；原形動詞／動詞片語 | create difficulties 指製造困難，不保留面對困難。 |

### 第 44 組｜deal with → address

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Schools should deal with this issue. |
| ①中文 | 學校應處理這個問題。 |
| ①參考改寫 | Schools should address this issue. |
| ②中文 | 政府必須處理公共運輸不足的問題。 |
| ②基本句（作答前不顯示） | The government must deal with the lack of public transportation. |
| ②參考答案 | The government must address the lack of public transportation. |
| 用法與限制 | address 表示著手處理問題；常見 address an issue。 address an issue 表著手處理，不保證問題已解決。 |
| 搭配 | Schools should address this issue. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| address（答案） | 處理（動詞）；原形動詞／動詞片語 | address 表示著手處理問題；常見 address an issue。 address an issue 表著手處理，不保證問題已解決。 本題搭配：Schools should address this issue. |
| ignore | 忽略；原形動詞／動詞片語 | ignore this issue 與 deal with 的積極處理相反。 |
| complicate | 使複雜化；原形動詞／動詞片語 | complicate this issue 是把問題變複雜，不表示處理。 |
| postpone | 延後；原形動詞／動詞片語 | postpone this issue 表把議題延後，沒有保留著手處理。 |

### 第 45 組｜solve → resolve

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Communication helps people solve problems. |
| ①中文 | 溝通有助於人們解決問題。 |
| ①參考改寫 | Communication helps people resolve problems. |
| ②中文 | 雙方透過討論化解了誤會。 |
| ②基本句（作答前不顯示） | The two sides cleared up the misunderstanding through discussion. |
| ②參考答案 | The two sides resolved the misunderstanding through discussion. |
| 用法與限制 | resolve 表示解決爭端或問題；常見 resolve conflicts。 resolve 在此指成功解決，不只是討論或辨識。 |
| 搭配 | resolve problems / resolve a misunderstanding |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| resolve（答案） | 解決；原形動詞／動詞片語 | resolve 表示解決爭端或問題；常見 resolve conflicts。 resolve 在此指成功解決，不只是討論或辨識。 本題搭配：resolve problems / resolve a misunderstanding |
| create | 造成；原形動詞／動詞片語 | create problems 表製造問題，與 solve 相反。 |
| identify | 辨識；原形動詞／動詞片語 | identify problems 表找出問題，不保證解決。 |
| discuss | 討論；原形動詞／動詞片語 | discuss problems 是討論，不必然已解決。 |

### 第 46 組｜build → establish

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | We should build good study habits. |
| ①中文 | 我們應建立良好讀書習慣。 |
| ①參考改寫 | We should establish good study habits. |
| ②中文 | 學校應建立明確的反霸凌規範。 |
| ②基本句（作答前不顯示） | Schools should set clear rules against bullying. |
| ②參考答案 | Schools should establish clear rules against bullying. |
| 用法與限制 | establish 表示建立制度或習慣；常見 establish good habits。 |
| 搭配 | We should establish good study habits. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| establish（答案） | 建立；原形動詞／動詞片語 | establish 表示建立制度或習慣；常見 establish good habits。 本題搭配：We should establish good study habits. |
| abandon | 放棄；原形動詞／動詞片語 | abandon habits 表放棄，與 build 相反。 |
| monitor | 監控；原形動詞／動詞片語 | monitor habits 表觀察追蹤，不表示建立。 |
| describe | 描述；原形動詞／動詞片語 | describe habits 表敘述習慣，沒有建立之意。 |

### 第 47 組｜encourage → promote

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Reading can encourage critical thinking. |
| ①中文 | 閱讀能促進批判性思考。 |
| ①參考改寫 | Reading can promote critical thinking. |
| ②中文 | 團隊活動能促進學生合作。 |
| ②基本句（作答前不顯示） | Group activities can encourage cooperation among students. |
| ②參考答案 | Group activities can promote cooperation among students. |
| 用法與限制 | promote 表示促進發展；常見 promote critical thinking。 |
| 搭配 | Reading can promote critical thinking. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| promote（答案） | 促進；推廣；原形動詞／動詞片語 | promote 表示促進發展；常見 promote critical thinking。 本題搭配：Reading can promote critical thinking. |
| discourage | 抑制；原形動詞／動詞片語 | discourage critical thinking 與 encourage 相反。 |
| measure | 測量；原形動詞／動詞片語 | measure critical thinking 表測量能力，不表示促進。 |
| require | 要求；原形動詞／動詞片語 | require critical thinking 表活動需要這種能力，不表示培養能力。 |

### 第 48 組｜protect → preserve

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | We must protect natural resources. |
| ①中文 | 我們必須保護自然資源。 |
| ①參考改寫 | We must preserve natural resources. |
| ②中文 | 我們應保護當地的文化傳統。 |
| ②基本句（作答前不顯示） | We should protect local cultural traditions. |
| ②參考答案 | We should preserve local cultural traditions. |
| 用法與限制 | preserve 表示保存、維護；常見 preserve natural resources。 preserve 強調使其免於損壞或消失；protect 更廣，可指當下防護。 |
| 搭配 | We must preserve natural resources. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| preserve（答案） | 保存；保護；原形動詞／動詞片語 | preserve 表示保存、維護；常見 preserve natural resources。 preserve 強調使其免於損壞或消失；protect 更廣，可指當下防護。 本題搭配：We must preserve natural resources. |
| exploit | 開採；利用；原形動詞／動詞片語 | exploit natural resources 表利用或開採，不保留保護。 |
| deplete | 耗盡；原形動詞／動詞片語 | deplete natural resources 與保護相反。 |
| catalogue | 編目；原形動詞／動詞片語 | catalogue natural resources 表盤點，不等於維護保存。 |

### 第 49 組｜understand → comprehend

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Students need to understand the main idea. |
| ①中文 | 學生需要理解主旨。 |
| ①參考改寫 | Students need to comprehend the main idea. |
| ②中文 | 文章核心概念不容易理解。 |
| ②基本句（作答前不顯示） | The main concept of the article is not easy to understand. |
| ②參考答案 | The main concept of the article is not easy to comprehend. |
| 用法與限制 | comprehend 表示理解；常見 comprehend the main idea。 understand 自然常用；comprehend 較正式，不能因較長便視為更好。 |
| 搭配 | Students need to comprehend the main idea. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| comprehend（答案） | 理解；原形動詞／動詞片語 | comprehend 表示理解；常見 comprehend the main idea。 understand 自然常用；comprehend 較正式，不能因較長便視為更好。 本題搭配：Students need to comprehend the main idea. |
| memorize | 背熟；原形動詞／動詞片語 | memorize the main idea 可以自然成立，但背熟不保證理解。 |
| reject | 否定；原形動詞／動詞片語 | reject the main idea 表不接受，不是理解其意。 |
| summarize | 摘要；原形動詞／動詞片語 | summarize the main idea 表濃縮轉述，並不等於理解這一心智狀態。 |

### 第 50 組｜improve → strengthen

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Teamwork can improve relationships. |
| ①中文 | 團隊合作可以改善人際關係。 |
| ①參考改寫 | Teamwork can strengthen relationships. |
| ②中文 | 共同克服挑戰可以增進彼此的信任。 |
| ②基本句（作答前不顯示） | Overcoming challenges together can improve mutual trust. |
| ②參考答案 | Overcoming challenges together can strengthen mutual trust. |
| 用法與限制 | strengthen 表示強化；常見 strengthen relationships。 improve 指改善，strengthen 強調關係或信任更強，屬方向一致的焦點變化。 |
| 搭配 | Teamwork can strengthen relationships. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| strengthen（答案） | 強化；原形動詞／動詞片語 | strengthen 表示強化；常見 strengthen relationships。 improve 指改善，strengthen 強調關係或信任更強，屬方向一致的焦點變化。 本題搭配：Teamwork can strengthen relationships. |
| weaken | 削弱；原形動詞／動詞片語 | weaken relationships 與 improve 相反。 |
| describe | 描述；原形動詞／動詞片語 | describe relationships 只敘述關係，沒有改善。 |
| complicate | 使複雜化；原形動詞／動詞片語 | complicate relationships 表使關係複雜，未保留正向改善。 |

### 第 51 組｜advantage → benefit

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Online learning offers many advantages. |
| ①中文 | 線上學習有許多益處。 |
| ①參考改寫 | Online learning offers many benefits. |
| ②中文 | 規律運動對長者有許多好處。 |
| ②基本句（作答前不顯示） | Regular exercise offers many advantages for older adults. |
| ②參考答案 | Regular exercise offers many benefits for older adults. |
| 用法與限制 | benefit 表示益處；常見 the benefits of online learning。 advantages 指有利之處；benefits 強調帶來的好處，兩者在本題可合理對應。 |
| 搭配 | Online learning offers many benefits. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| benefits（答案） | 益處（複數名詞）；複數名詞 | benefit 表示益處；常見 the benefits of online learning。 advantages 指有利之處；benefits 強調帶來的好處，兩者在本題可合理對應。 本題搭配：Online learning offers many benefits. |
| drawbacks | 缺點；複數名詞 | drawbacks 表缺點，與 advantages 相反。 |
| restrictions | 限制；複數名詞 | restrictions 表限制條件，不保留優點。 |
| obligations | 義務；複數名詞 | obligations 表必須承擔的責任，不是益處。 |

### 第 52 組｜disadvantage → drawback

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | One major disadvantage is the high cost. |
| ①中文 | 一項主要缺點是成本高。 |
| ①參考改寫 | One major drawback is the high cost. |
| ②中文 | 有限的面對面互動是遠距學習的一項缺點。 |
| ②基本句（作答前不顯示） | Limited face-to-face interaction is a disadvantage of distance learning. |
| ②參考答案 | Limited face-to-face interaction is a drawback of distance learning. |
| 用法與限制 | drawback 表示缺點、不利之處；常見 a major drawback。 |
| 搭配 | One major drawback is the high cost. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| drawback（答案） | 缺點；單數名詞 | drawback 表示缺點、不利之處；常見 a major drawback。 本題搭配：One major drawback is the high cost. |
| benefit | 益處；單數名詞 | benefit 表優點，與 disadvantage 相反。 |
| requirement | 要求；單數名詞 | requirement 表必須符合的條件，不等於缺點。 |
| objective | 目標；單數名詞 | objective 表企圖達成的目的，不是成本高這項不利之處。 |

### 第 53 組｜results → consequence

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | The decision had unexpected results. |
| ①中文 | 這項決定帶來了意料之外的結果。 |
| ①參考改寫 | The decision had unexpected consequences. |
| ②中文 | 忽略小問題可能造成嚴重的影響。 |
| ②基本句（作答前不顯示） | Ignoring small problems can lead to serious effects. |
| ②參考答案 | Ignoring small problems can have serious consequences. |
| 用法與限制 | consequences 指行為或事件造成的結果，常帶負面語氣，但不必然都是壞結果。results 較中性；此題保留「意料之外的結果」。 |
| 搭配 | unexpected consequences / serious consequences |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| consequences（答案） | 後果；複數名詞 | consequences 指行為或事件造成的結果，常帶負面語氣，但不必然都是壞結果。results 較中性；此題保留「意料之外的結果」。 本題搭配：unexpected consequences / serious consequences |
| intentions | 意圖；複數名詞 | intentions 是決策者的意圖，不是決策產生的結果。 |
| requirements | 要求；複數名詞 | requirements 是須符合的條件，不是決策之後產生的結果。 |
| alternatives | 替代選擇；複數名詞 | alternatives 是可改選的方案，與 results 的結果不同。 |

### 第 54 組｜reason → factor

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Stress is a major reason for poor sleep. |
| ①中文 | 壓力是造成睡眠不佳的一個主要原因。 |
| ①參考改寫 | Stress is a major factor contributing to poor sleep. |
| ②中文 | 家庭支持是青少年變得更有自信的一個重要原因。 |
| ②基本句（作答前不顯示） | Family support is an important reason why teenagers become more confident. |
| ②參考答案 | Family support is an important factor in helping teenagers become more confident. |
| 用法與限制 | reason 指原因；factor 指促成結果的因素，通常只是多個因素之一。contributing to poor sleep 保留睡眠不佳的負面結果，不把因素說成唯一原因。 factor 不表示唯一或充分原因；保留 poor sleep，不把影響睡眠籠統視為等義。 |
| 搭配 | a factor contributing to poor sleep / a factor in helping someone do something |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| factor（答案） | 因素；單數名詞 | reason 指原因；factor 指促成結果的因素，通常只是多個因素之一。contributing to poor sleep 保留睡眠不佳的負面結果，不把因素說成唯一原因。 factor 不表示唯一或充分原因；保留 poor sleep，不把影響睡眠籠統視為等義。 本題搭配：a factor contributing to poor sleep / a factor in helping someone do something |
| barrier | 阻礙；單數名詞 | barrier contributing to poor sleep 把壓力說成阻擋物，未準確保留造成睡不好的原因角色。 |
| remedy | 療法；單數名詞 | remedy 是改善問題的方法，不是造成睡不好的原因。 |
| symptom | 症狀；單數名詞 | symptom 是表現出的徵象，不保留原句的成因方向。 |

### 第 55 組｜way → approach

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | We need a different way to deal with this problem. |
| ①中文 | 我們需要不同方式處理問題。 |
| ①參考改寫 | We need a different approach for dealing with this problem. |
| ②中文 | 合作是解決社區問題的有效方式。 |
| ②基本句（作答前不顯示） | Working together is an effective way to solve community problems. |
| ②參考答案 | Working together is an effective approach to solving community problems. |
| 用法與限制 | approach 表示處理問題的方法；常見 an approach to a problem。 approach to 後接名詞或動名詞，指處理方法；solution 指問題的解答，可能是方法的成果。 |
| 搭配 | an approach for dealing with a problem / an approach to solving problems |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| approach（答案） | 方法；途徑；單數名詞 | approach 表示處理問題的方法；常見 an approach to a problem。 approach to 後接名詞或動名詞，指處理方法；solution 指問題的解答，可能是方法的成果。 本題搭配：an approach for dealing with a problem / an approach to solving problems |
| budget | 預算；單數名詞 | a different budget for dealing with this problem 是處理問題的另一份預算，不保留 way 的方法。 |
| deadline | 期限；單數名詞 | a different deadline for dealing with this problem 指處理期限，未說處理方法不同。 |
| location | 地點；單數名詞 | a different location for dealing with this problem 指處理地點，未保留方法的差異。 |

### 第 56 組｜goal → objective

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Our main goal is to reduce waste. |
| ①中文 | 我們的主要目標是減少浪費。 |
| ①參考改寫 | Our main objective is to reduce waste. |
| ②中文 | 她的主要目標是提升口說能力。 |
| ②基本句（作答前不顯示） | Her main goal is to improve her speaking skills. |
| ②參考答案 | Her main objective is to improve her speaking skills. |
| 用法與限制 | objective 表示目標；常見 the main objective is to ...。 |
| 搭配 | Our main objective is to reduce waste. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| objective（答案） | 目標（名詞）；單數名詞 | objective 表示目標；常見 the main objective is to ...。 本題搭配：Our main objective is to reduce waste. |
| obstacle | 障礙；單數名詞 | main obstacle 是主要阻礙，不保留想達成的目標。 |
| obligation | 義務；單數名詞 | obligation 為必須承擔的責任；goal 只表希望達成的目標，未加入義務。 |
| resource | 資源；單數名詞 | main resource 是主要資源，不是減少浪費這個目的。 |

### 第 57 組｜effect → impact

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Technology has a profound effect on society. |
| ①中文 | 科技對社會有深遠影響。 |
| ①參考改寫 | Technology has a profound impact on society. |
| ②中文 | 社群媒體對青少年自我形象有很大影響。 |
| ②基本句（作答前不顯示） | Social media has a strong effect on teenagers' self-image. |
| ②參考答案 | Social media has a strong impact on teenagers' self-image. |
| 用法與限制 | impact 表示影響；常見 have an impact on。 |
| 搭配 | Technology has a profound impact on society. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| impact（答案） | 影響；單數名詞 | impact 表示影響；常見 have an impact on。 本題搭配：Technology has a profound impact on society. |
| reliance | 依賴；單數名詞 | reliance on society 表依賴社會，把原句對社會產生影響的方向改變。 |
| burden | 負擔；單數名詞 | burden on society 加入負面負擔，effect 未限定負面。 |
| claim | 主張；單數名詞 | claim on society 不表示科技造成的影響。 |

### 第 58 組｜barrier → obstacle

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Fear can be a barrier on the path to success. |
| ①中文 | 恐懼可能是通往成功之路上的阻礙。 |
| ①參考改寫 | Fear can be an obstacle on the path to success. |
| ②中文 | 缺乏自信不應成為追求夢想的阻礙。 |
| ②基本句（作答前不顯示） | A lack of confidence should not be a barrier to pursuing dreams. |
| ②參考答案 | A lack of confidence should not be an obstacle to pursuing dreams. |
| 用法與限制 | obstacle 表示障礙；常見 an obstacle to success。 |
| 搭配 | Fear can become an obstacle to success. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| obstacle（答案） | 障礙；單數名詞 | obstacle 表示障礙；常見 an obstacle to success。 本題搭配：Fear can become an obstacle to success. |
| asset | 助力；單數名詞 | an asset on the path to success 表有助成功的資產或優勢，與 barrier 相反。 |
| advantage | 優勢；單數名詞 | an advantage on the path to success 表有利條件，並非阻礙。 |
| incentive | 誘因；單數名詞 | an incentive on the path to success 表促使前進的動力，不保留障礙。 |

### 第 59 組｜ability → capability

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Technology can improve students' learning abilities. |
| ①中文 | 科技可以提升學生的學習能力。 |
| ①參考改寫 | Technology can improve students' learning capabilities. |
| ②中文 | 這項訓練可以提升團隊解決問題的能力。 |
| ②基本句（作答前不顯示） | The training can improve the team's ability to solve problems. |
| ②參考答案 | The training can improve the team's capability to solve problems. |
| 用法與限制 | capability 表示能力；常見 learning capabilities。 ability 常用且自然；capability 偏具有完成任務的能力或潛能，本題不再同時改 improve 為 strengthen。 |
| 搭配 | Technology can improve students' learning capabilities. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| capabilities（答案） | 能力；複數名詞 | capability 表示能力；常見 learning capabilities。 ability 常用且自然；capability 偏具有完成任務的能力或潛能，本題不再同時改 improve 為 strengthen。 本題搭配：Technology can improve students' learning capabilities. |
| difficulties | 困難；複數名詞 | learning difficulties 指學習困難，與 abilities 的能力不同。 |
| preferences | 偏好；複數名詞 | learning preferences 指學習偏好，不是能做什麼的能力。 |
| routines | 常規；複數名詞 | learning routines 指學習慣例，不等於能力。 |

### 第 60 組｜choice → alternative

| 項目 | 修訂內容 |
|---|---|
| ①基本句 | Public transportation is another option that can replace driving. |
| ①中文 | 大眾運輸是另一種可以代替開車的選擇。 |
| ①參考改寫 | Public transportation is an alternative that can replace driving. |
| ②中文 | 騎自行車是短程出行時開車以外的另一種選擇。 |
| ②基本句（作答前不顯示） | Cycling is another option instead of driving for short trips. |
| ②參考答案 | Cycling is an alternative to driving for short trips. |
| 用法與限制 | alternative 表示另一種選擇；常見 an alternative to driving。 alternative to driving 是開車以外的替代；approach to driving 是開車的方法。 |
| 搭配 | Public transportation is an alternative to driving. |
| ①測試 | 三條作答路徑、重練／重設：模型通過；瀏覽器未驗證 |
| ②測試 | 空白看答、隱藏、輸入、清字、完成保存：模型通過；瀏覽器未驗證 |

| 選項 | 本題義與形式 | 理由 |
|---|---|---|
| alternative（答案） | 替代方案；單數名詞 | alternative 表示另一種選擇；常見 an alternative to driving。 alternative to driving 是開車以外的替代；approach to driving 是開車的方法。 本題搭配：Public transportation is an alternative to driving. |
| obligation | 義務；單數名詞 | an obligation 是必須履行的責任，不保留 another option 所指的選擇。 |
| achievement | 成就；單數名詞 | an achievement 是達成的成果，不是供人選用的交通替代方案。 |
| obstacle | 障礙；單數名詞 | an obstacle 是阻礙，並非能取代開車的選擇。 |
