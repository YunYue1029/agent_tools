# Good and Bad PR Descriptions

One test decides every line: **could the reviewer get this from the diff or the file tree?** If yes, delete it. What's left is the description's actual job — the effect, and the why.

Both examples describe the same PR. The prose is 繁體中文; prefixes, headings, and identifiers stay English.

## Bad

```
feat: add CPE search v2

## Summary
- 新增 `nist_cpe_handle_v2` management command
- 在 `cpe_search_v2.py` 加入 `CPEQuery` class
- 把 `normalize_cpe_string()` 抽到 `normalize_data_v2.py`
- 相似度 threshold 從 0.7 調到 0.8
- 新增 `v2.md` 文件

## Behavior / Compatibility
- v1 沒有變動
- env vars N/A
- 無 breaking change
```

Every bullet is a filename or a class name with a verb in front of it — that's the file tree, retyped. None of it says **why** the threshold moved, **why** v2 exists at all, or **what a caller actually experiences differently**. A reviewer who reads this knows exactly as much as `git diff --stat` told them, and has to open the diff anyway to learn anything real. And `N/A` is a heading answering a question nobody asked — if there's nothing to say about env vars, don't raise the topic.

## Good

```
feat: CPE search v2 — 比對 v1 會默默丟掉的 URI-encoded 2.3 字串

## Summary
- v1 會丟掉所有 URI-encoded 的 CPE 2.3 字串，所以用這種格式發佈的廠商
  （Cisco、Oracle）從來沒被比對到，大約佔整個 corpus 的 12%。v2 能處理
  這些字串。
- 修好之後，舊的比對 threshold 開始出現 false positive，所以評分也必須
  一起重新加權。
- 預設仍然走 v1，v2 採 opt-in，讓兩者能先在真實資料上對照過再決定要不要
  切換。

## Behavior / Compatibility
- 同一筆輸入在 v2 的分數和 v1 不能直接比較 — 這是刻意的，不是 regression。
  目前下游沒有任何地方在比較這兩組分數，但在 v2 變成預設之前值得再確認一次。
```

- **Every line states an effect a reviewer couldn't get elsewhere.** "12% of the corpus never matched" is a fact about the world, not about the code — no diff shows it.
- **The causal chain is explicit.** The threshold moved *because* fixing the parser changed what matches. That link is the one thing a diff structurally cannot express, and it's the first question a future reader will have.
- **No class names, no filenames.** Nothing here would change if the implementation were rewritten with different names tomorrow — which is the point: the description outlives the code that made it true.
- **Behavior/Compatibility only exists because there's a real caveat.** A PR with no caller-visible change would drop this section rather than write "no breaking changes" into it.

A one-line fix needs a title and nothing else. Sections earn their place by carrying something the file tree can't — otherwise cut the heading, don't fill it.
