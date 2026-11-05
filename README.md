# DSH Model Senses · 模型感官

幫每個 Agent 加一對眼同一把搜尋器。`model_senses` 工具可以睇圖、上網搜尋，跟住你而家嘅部署預設 API；你切換模型，佢會自動跟。

Gives every DSH agent vision and web search through the live deployment-default API. Switch the default model and the tools follow on the next call.

## 做啲咩

`model_senses` 有三個動作：

| 動作 | 做啲咩 |
|---|---|
| `see` | 睇一張圖：本地 `png`／`jpeg`／`webp`／`gif`，或者 https 圖 URL，加一條問題 |
| `search` | 搜尋公開資料；xAI 預設用原生 web search（`includeX: true` 加 X 搜尋），其他 API 自動用 DSH 嘅 web provider |
| `status` | 顯示而家 route 去邊個 provider／model／backend；`probe: true` 做即時檢查 |

### 睇圖路由

1. 先用同一個 API 入面有視覺嘅 model。
2. 唔得就用部署預設。
3. 再唔得就用其他已連線、有視覺嘅 API。
4. 回覆會講明邊個 route 答（`selected`／`same-api`／`default`／`cross-api`）。

所以就算你揀咗純文字模型，都唔會冇咗睇圖能力。

### 搜尋

- 預設 API 係 xAI：用 xAI 原生 `web_search`；`includeX: true` 會加 `x_search`。
- 其他 API：用 DSH `ctx.web`，回傳最多 8 個來源 URL。
- 原生搜尋失敗會自動 fallback 去 harness web，唔會靜靜死。

### 憑證

唔使額外 plugin，直接用你 DSH 已經設定好嘅 provider：

- 非 xAI：交畀 `ctx.llm` adapter，用你原本嘅 provider 憑證。
- xAI：可以用 `XAI_API_KEY`，或者 DSH 入面已儲存嘅 xAI OAuth grant（會自動 refresh，唔會喺工具結果度顯示 token）。

## 限制

- 圖片上限 8 MiB；答案最多 12,000 字。
- `see` timeout 120 秒，`search` 180 秒。
- 圖片會送去被路由嘅 API；plugin 唔會額外儲存。
- 本 plugin 唔會自己加 API key，亦唔會改你嘅 model 設定。

## 同系列 DSH 插件

| 插件 / Plugin | 功能 / What it does | 要唔要裝埋 / Also install | Repo |
|---|---|---|---|
| **Attention Center** | 側欄通知收件箱：任務完成、待核准、新問題會閃，系統允許時出桌面通知<br>Sidebar inbox with a flashing icon and optional desktop notifications | 唔使 / None | [dsh-attention-center](https://github.com/Kinfxhk/dsh-attention-center) |
| **Computer Use** | 側欄「電腦」頁：Agent 可以睇窗、click、打字、按鍵、捲動（隨時可以熄）<br>Computer page where the agent observes and operates windows | 要：Windows + Cua driver<br>Yes: Windows + Cua driver | [dsh-computer-use](https://github.com/Kinfxhk/dsh-computer-use) |
| **Model Senses** | `model_senses`：睇圖＋上網搜尋，跟住即時預設 API<br>Vision and web-search tools routed through the live default API | 唔使（用已連線 API）<br>None (uses configured APIs) | [dsh-model-senses](https://github.com/Kinfxhk/dsh-model-senses) |
| **Model Shelf** | 側欄模型架：按 API 篩選、收藏、一撳套用<br>Sidebar shelf to filter, bookmark, and apply models | 唔使 / None | [dsh-model-shelf](https://github.com/Kinfxhk/dsh-model-shelf) |
| **Perplexity Guard** | 修正 Perplexity 嘅 Opus／Sonnet 5.5、GPT-6.1 Sol 請求同 provider 設定<br>Fixes Perplexity requests and provider settings | 要：Perplexity API key<br>Yes: Perplexity API key | [dsh-perplexity-api-fix-plugin](https://github.com/Kinfxhk/dsh-perplexity-api-fix-plugin) |

## 安裝

需要 DSH `0.2.1-rc.5` 或以上。

1. 下載呢個 repo（`git clone`，或者 **Code → Download ZIP**）。
2. 喺 DSH 用 `plugin_manager` 安裝，`target` 填資料夾嘅絕對路徑：

   ```text
   plugin_manager(action: "install_bundle", target: "C:\\path\\to\\dsh-model-senses")
   ```

3. 重啟 DSH。之後 Agent 就有 `model_senses` 工具，唔使設定。

## English

`model_senses` gives an agent two capabilities that DSH chat does not forward on its own: image understanding and web search through the live deployment-default API.

- `see` — local `png`/`jpeg`/`webp`/`gif` or an https image URL plus a question. If the selected model cannot see, it picks a vision model on the same API, then the deployment default, then another configured API, and reports which route answered.
- `search` — xAI defaults use native `web_search` (and `x_search` with `includeX: true`), with automatic fallback to the harness web provider. Other APIs use the harness provider directly.
- `status` — the live route, credentials, and backends; `probe: true` performs a live check.

No extra plugin or configuration is needed: it uses the provider credentials already stored in DSH. For xAI it accepts `XAI_API_KEY` or a stored OAuth grant (refreshed automatically; tokens never appear in tool results).

Limits: images up to 8 MiB, answers capped at 12,000 characters, 120 s see timeout, 180 s search timeout.

Install: clone and run `plugin_manager` with `action: "install_bundle"` and the absolute folder path, then restart DSH.

## License

MIT
