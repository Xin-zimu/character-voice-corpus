# character-voice-corpus

主项目 [Xin-zimu/character-voice](https://github.com/Xin-zimu/character-voice) 的**私有语料备份**。

字幕原文来自受版权保护的动画作品，所以不进公开仓库。本仓库**仅作个人备份与跨机器同步，不对外分发**，也不要改成公开。

## 为什么单独一个仓库

主项目是公开的，`.gitignore` 排除了 `data/parsed/`、`data/index/`、`data/scenes/`、`data/_deprecated/`。语料要跟着走，就放到这里。主项目克隆后即使没有语料也能跑（检索为空，降级运行），把这里的目录拷回去即可恢复。

## 内容

| 路径 | 内容 | 来源 |
|---|---|---|
| `parsed/*.dialogue.jsonl` | 13070 条字幕逐条解析，29 个片源 | 华盟字幕社中文字幕，由 `scripts/parse_ass.py` 解析 |
| `scenes/*.scenes.txt` | 164 片段 / 4316 行，「长门 / 有希」称呼锚点 ±8 条上下文 | L1 定位层 |
| `_deprecated/candidates_heuristic/` | 1118 条废弃候选池，**禁用**，精度约 15% | 字面特征筛选的失败路径，保留仅作记录 |
| `index/` | BM25 索引（`docs.jsonl` 等） | `py build_corpus.py` 生成 |
| `seed_nagato_lines.md` | 10 条人工核定台词（L2） | 逐句核对上下文确认 |
| `DATA-RULES.md` | 分级与准入规则（主项目 `data/README.md` 的副本） | 链接相对主项目，副本内不保证可跳转 |

## 还原到主项目

```bash
cd <主项目>
cp -r <本仓库>/parsed       data/
cp -r <本仓库>/index        data/
cp -r <本仓库>/scenes       data/
cp -r <本仓库>/_deprecated  data/
cp <本仓库>/seed_nagato_lines.md data/
```

不放索引也可以，跑一次 `py build_corpus.py` 会重建。

## 纪律

- **索引和切分规则绑定**。当前《消失》切点是 `0:38:13`（`movie_cut_sec` 2293.79）。改了 `build_corpus.py` 的切分逻辑就要重新生成索引，不要沿用旧的那份。
- 语料分级、两种准入（剧情参考 / 角色台词依据）、世界归属的已知问题，全部以 `DATA-RULES.md` 为准。
- 不要因为「反正是私有仓库」就把未核定的片段当作该世界的经历——归属问题跟仓库可见性无关。
