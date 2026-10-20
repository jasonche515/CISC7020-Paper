Below is a slide-by-slide outline, suitable for page-by-page spoken narration in a 5–10 minute rehearsal (formulas are not covered in detail):

> **繁中：** 以下是每頁投影片 (slide) 的繁體中文概述 (outline)，適合 5–10 分鐘中文彩排 (rehearsal) 時逐頁口頭說明 (spoken narration)（不含公式 (formula) 細節 (detail)）：

---

## Slide 1 — 標題 (Title)
Self-introduction at the start (Che Chon Hong, University of Macau); state the topic — deterministic, synchronous, self-stabilizing BFS construction with constant space — and announce that the algorithm will be explained rule by rule.

> **繁中：** 開場自我介紹 (self-introduction)（澳門大學 Che Chon Hong），說明報告主題 (topic)：具常數空間 (constant space) 的確定性 (deterministic)、同步 (synchronous)、自穩定 (self-stabilizing) BFS 建構 (BFS construction)，並預告會逐條規則 (rule by rule) 說明演算法 (algorithm)。

---

## Slide 2 — Part 1：模型與問題 (model and problem)
Define the network model: a connected undirected graph, vertices communicating through locally numbered, fixed ports, semi-uniform and anonymous (only the root is special). The goal is "BFS modulo three": every vertex stores the distance to the root modulo three in a constant number of bits, without knowing n, the maximum degree or the depth. In the picture, blue, green and orange stand for distance modulo three equal to 0, 1 and 2.

> **繁中：** 定義網路模型 (network model)：連通 (connected) 無向 (undirected) 圖 (graph)，頂點 (vertex) 透過固定編號 (fixed numbering) 的本地埠 (local port) 溝通 (communicate)，半均勻 (semi-uniform) 且匿名 (anonymous)（只有根 (root) 特別）。目標是「BFS 模三 (modulo three)」：每個頂點用常數位元 (constant bits) 儲存到根的距離 (distance) 取模三，且不需知道 n、最大度數 (maximum degree) 或深度 (depth)。圖中以藍 (blue)、綠 (green)、橘 (orange) 分別代表距離模三為 0、1、2。

---

## Slide 3 — Part 2：演算法總覽 (algorithm overview)
The root runs TokBinR and injects tokens, while the other vertices run TokBinV. Tokens arriving from several parents are merged rather than multiplied; every vertex keeps a single bit b and flips it at each reception. A token is forwarded only when the neighbour's bit is 1, and this condition bounds the lifetime of a token, so no cycle can sustain one forever. The system stabilizes within big-O of two to the power epsilon rounds, with 36 local states and 6 bits, and it is a non-silent protocol.

> **繁中：** 根 (root) 執行 TokBinR 注入 (inject) token，其他頂點執行 TokBinV。多個父節點 (parent) 送來的 token 會合併 (merge) 而非增殖 (multiply)；每個頂點只保留一個位元 (bit) b，每次收到 token 就翻轉 (flip)。只有當鄰居 (neighbour) 位元為 1 時才轉送 (forward) token，這個條件 (condition) 限制了 token 壽命 (lifetime)，使環 (cycle) 無法永遠維持 token。系統在 big-O of two to the power epsilon 回合 (round) 內穩定 (stabilize)，共 36 個本地狀態 (local state)、6 個位元，且為非靜默 (non-silent) 協定 (protocol)。

---

## Slide 4 — Part 3：本地變數與狀態空間 (local variables and state space)
Every vertex stores three fields: k (rank, taking 0 / 1 / 2 / bottom, equal to the distance modulo three at stabilization, bottom meaning reset), t (false / true / wait, cycling), b (0 / 1 / top, 0 and 1 alternating, top flagging an error). The state space is 4×3×3 = 36 states and 6 bits, with a worked example showing how the six-bit string is composed. The graph on the right is an example of depth three with ten vertices.

> **繁中：** 每個頂點存三個欄位 (field)：k（rank，取值 0/1/2/bottom，穩定時為距離模三，bottom 代表重置 (reset)）、t（false/true/wait 循環 (cycle)）、b（0/1/top，0 與 1 交替 (alternate)，top 標記 (flag) 錯誤 (error)）。狀態空間 (state space) 為 4×3×3 = 36 狀態、6 位元，並舉例 (worked example) 說明六位元字串 (bit string) 的組成。右圖為深度 (depth) 三、十個頂點的範例 (example)。

---

## Slide 5 — Part 4：名詞定義（親屬結構與重置狀態）(terms: kinship and reset state)
Four concepts, defined in order:

1. **Kinship**: parent, child and sibling are defined by the rank difference modulo three; only the two adjacent levels are meaningful.
2. **The reset state**: k = bottom, t = false, b = 0; a reset vertex cuts every family relation.
3. **Parent selection**: an anonymous vertex takes as parent the neighbour behind the smallest port number — computed on the fly, never stored.
4. **The oriented BFS tree**: root at rank 0, every arrow pointing to its parent.

> **繁中：** 依序定義四組概念 (concept)：
>
> 1. **親屬關係 (kinship)**：依 rank 差值 (difference)（模三）定義父 (parent)、子 (child)、兄弟 (sibling)；只有相鄰兩層 (adjacent levels) 有意義。
> 2. **重置狀態 (reset state)**：k=bottom、t=false、b=0，重置頂點斷開 (cut) 所有家庭關係 (family relation)。
> 3. **父節點選擇 (parent selection)**：匿名 (anonymous) 頂點以埠號 (port number) 最小者選父，即時計算 (computed on the fly)、不儲存 (never stored)。
> 4. **定向 BFS 樹 (oriented BFS tree)**：根 rank 0，箭頭 (arrow) 指向父節點。

---

## Slide 6 — Part 5：TokBinR — 三個述詞、兩條規則 (three predicates, two rules)
Three predicates of the root algorithm: N-Synch (the whole neighbourhood is at rank 1 and agrees on the bit and on the flag), N-Reset (every neighbour is reset), N-Er (every neighbour carries the error bit). Two rules: R-er-root (when none of the three predicates holds, raise the error flag), R-go (with the error flag set and N-Er holding, restart).

> **繁中：** 根演算法 (root algorithm) 的三個述詞 (predicate)：N-Synch（整個鄰域 (neighbourhood) 都在 rank 1 且位元與旗標 (flag) 一致）、N-Reset（所有鄰居皆重置）、N-Er（所有鄰居都帶錯誤位元 (error bit)）。兩條規則 (rule)：R-er-root（三個述詞皆不成立時舉起 (raise) 錯誤旗標）、R-go（錯誤旗標且 N-Er 成立時重新啟動 (restart)）。

---

## Slide 7 — Part 6：token 接收述詞 (token reception predicates)
Before the seven rules of a non-root vertex, six token-related predicates are defined: Tok (the set of neighbours offering a token), TakeP (the token sources are exactly the set of parents), TakeO (the sources are not the set of parents), Tok-rk (the minimum rank among the sources), TakeR (the sources are exactly the neighbours of minimum rank), k-t (the rank finally adopted = Tok-rk + 1 modulo three).

> **繁中：** 在非根 (non-root) 頂點七條規則之前，先定義六個 token 相關述詞：Tok（提供 token 的鄰居集合 (set)）、TakeP（token 來源 (source) 恰為父節點集合）、TakeO（來源非父節點集合）、Tok-rk（來源中最小 rank）、TakeR（來源恰為最小 rank 的鄰居集合）、k-t（最後採用 (adopt) 的 rank = Tok-rk + 1 模三）。

---

## Slide 8 — Part 7：TokBinV — 七條規則 (the seven rules)
The seven rules of a non-root vertex, one by one: R-er (declare an error), R-reset (reset), R-er-rank (a stabilized vertex that receives a non-parent token reports an error), R-join (rejoin from the reset state), R-tok (accept a parent's token), R-add (flip the bit), R-ready (end the wait). The picture on the right is the state automaton: white for unknown, red for error, grey for reset; solid arrows are the normal cycle, the red dotted ones lead to the error, the blue dashed ones are the reset and the rejoin.

> **繁中：** 逐條說明 (one by one) 非根頂點的七條規則：R-er（宣告 (declare) 錯誤）、R-reset（重置）、R-er-rank（已穩定 (stabilized) 頂點收到非父 token 即報錯 (report an error)）、R-join（從重置重新加入 (rejoin)）、R-tok（接受 (accept) 父節點 token）、R-add（翻轉位元）、R-ready（結束等待 (wait)）。右圖為狀態自動機 (state automaton)：白色未知 (unknown)、紅色錯誤、灰色重置；實線 (solid arrow) 為正常循環 (normal cycle)，紅虛點 (red dotted) 指向錯誤，藍虛線 (blue dashed) 為重置與重新加入。

---

## Slide 9 — Part 8：錯誤偵測 I — 內部狀態與 token 流向 (error detection I: internal state and token flow)
Five error predicates about internal state and token flow: Er-var (bottom, but not a legal reset), Er-root (a neighbour of rank 0, yet more than one parent), Er-noP (not bottom, yet no parent), Er-tp (a child sends a token but v is not waiting for it), Er-wait (waiting for a reset neighbour, which will never answer).

> **繁中：** 五個與內部狀態 (internal state)、token 流向 (token flow) 有關的錯誤述詞：Er-var（bottom 但非合法 (legal) 重置）、Er-root（鄰居 rank 0 卻有多個父節點）、Er-noP（非 bottom 卻無父節點）、Er-tp（子節點送 token 但自己未在等待 (waiting)）、Er-wait（等待一個重置鄰居，永遠不會有回應 (answer)）。

---

## Slide 10 — Part 9：錯誤偵測 II — 同步性與傳播性 (error detection II: synchronization and propagation)
Six predicates in the second half: Er-P / Er-C / Er-S (parents, children or siblings disagree on the bit or on the flag), Er-Re (a vertex still owns a child while an error comes from below), Er-N (the combined test for disagreeing neighbours), Er-pi (a reset vertex observes neighbours of equal rank that disagree), Er-prg (a neighbour carries top, so the error propagates). Finally the global predicate Er is defined as the logical OR of the seven predicates. In short: these thirteen predicates give the protocol its self-stabilizing power — raise, propagate, reset, rebuild.

> **繁中：** 下半部 (second half) 六個述詞：Er-P／Er-C／Er-S（父、子、兄弟之間在位元或旗標上不一致 (disagree)）、Er-Re（仍有子節點卻從下方傳來錯誤）、Er-N（鄰居不一致的綜合判斷 (combined test)）、Er-pi（重置頂點觀察到 (observe) 同 rank 鄰居不一致）、Er-prg（鄰居帶 top，錯誤傳播 (propagate)）。最後定義全域述詞 (global predicate) Er 為七個述詞的邏輯或 (logical OR)。總結 (in short)：這十三個述詞賦予 (give) 協定自穩定能力 (self-stabilizing power)——舉錯、傳播、重置、重建 (rebuild)。

---

## Slide 11 — Part 10：JavaScript 即時 Demo (live demo)
From the paper to an implementation: a complete JavaScript simulator running the same rule set. The colours match the slides, and k, t and b can be read beside every vertex. A ready-made or a random connected graph can be chosen; execution is synchronous (all vertices evaluate their guards at the same time and then apply their assignments together), and the log records which rule fired where. Demo flow: play and watch the tokens move towards the root, the bits flip and the flags cycle, until after N rounds the ranks converge; then inject an error to show the red vertices appearing, the error propagating, the reset, the rejoin and the convergence again — validating self-stabilization.

> **繁中：** 從論文 (paper) 走到實作 (implementation)：完整的 JavaScript 模擬器 (simulator)，執行同一套規則 (rule set)。顏色 (colour) 與投影片一致，每個頂點旁可讀 k、t、b 三欄位。可選擇現成 (ready-made) 或隨機 (random) 連通圖 (connected graph)，同步執行 (synchronous execution)（所有頂點同時評估 (evaluate) 守衛 (guard) 再一起賦值 (assignment)），log 記錄哪條規則在何處觸發 (fire)。Demo 流程 (flow)：播放 (play) 觀察 token 朝根移動、位元翻轉、旗標循環，經過 N 回合 rank 收斂 (converge)；接著注入 (inject) 錯誤，展示紅色頂點出現、錯誤傳播、重置、重新加入、再次收斂——驗證 (validate) 自穩定。

---

## Slide 12 — Part 11：結論 (conclusion)
Five closing points:

1. The first constant-space, deterministic, self-stabilizing BFS construction for semi-uniform synchronous networks of arbitrary topology; the vertices need no global parameter.
2. Because point-to-neighbour and distance-to-root are unavailable, an infinite token stream is introduced to break cycles; it may be useful for other tasks.
3. A by-product: an optimal two-colouring of bipartite graphs in constant memory (earlier solutions need Omega of log n bits).
4. Open problem one: the method assumes full synchrony; whether it extends to an asynchronous setting in constant space is still unclear.
5. Open problem two: space is constant but time is exponential (big-O of two to the power epsilon); constant space together with polynomial time is left for future research.

> **繁中：** 五點總結 (closing points)：
>
> 1. 首個 (first) 適用於任意拓樸 (arbitrary topology) 半均勻 (semi-uniform) 同步網路 (synchronous network) 的常數空間 (constant space)、確定性 (deterministic)、自穩定 (self-stabilizing) BFS 建構 (construction)，頂點不需任何全域參數 (global parameter)。
> 2. 因 point-to-neighbour 與 distance-to-root 不可用 (unavailable)，引入無限 (infinite) token 流 (token stream) 以打破環 (break cycles)，可能對其他任務 (task) 有用。
> 3. 副產品 (by-product)：常數記憶體 (constant memory) 下可得二分圖 (bipartite graph) 的最佳二著色 (optimal two-colouring)（先前解法 (solution) 需 Omega of log n 位元）。
> 4. 開放問題一 (open problem)：方法假設完全同步 (full synchrony)，能否以常數空間推廣 (extend) 到非同步環境 (asynchronous setting) 仍不清楚。
> 5. 開放問題二：空間常數但時間指數 (exponential)（big-O of two to the power epsilon），常數空間加多項式時間 (polynomial time) 留待未來研究 (future work)。

---

## Slide 13 — 結尾 (closing)
In one sentence: a deterministic, synchronous, self-stabilizing protocol that builds a BFS tree within big-O of two to the power epsilon rounds with only six bits per vertex, independently of the size, the maximum degree and the depth of the network. Thank you — questions are welcome.

> **繁中：** 一句話總結 (summary in one sentence)：確定性 (deterministic)、同步 (synchronous)、自穩定 (self-stabilizing) 的協定 (protocol)，在 big-O of two to the power epsilon 回合 (round) 內建立 BFS 樹 (BFS tree)，每頂點僅六位元 (six bits)，且與網路規模 (size)、最大度數 (maximum degree)、深度 (depth) 無關 (independent)。謝謝，歡迎提問 (questions)。

