Below is a slide-by-slide outline, suitable for page-by-page spoken narration in a 5–10 minute rehearsal (formulas are not covered in detail):

> 以下是每頁投影片 (slide) 的繁體中文概述 (outline)，適合 5–10 分鐘中文彩排 (rehearsal) 時逐頁口頭說明 (spoken narration)（不含公式 (formula) 細節 (detail)）：

---

## Slide 1 — 標題 (Title)
Self-introduction at the start (Che Chon Hong, University of Macau); state the topic — deterministic, synchronous, self-stabilizing BFS construction with constant space — and announce that the algorithm will be explained rule by rule.

> 開場自我介紹 (self-introduction)（澳門大學 Che Chon Hong），說明報告主題 (topic)：具常數空間 (constant space) 的確定性 (deterministic)、同步 (synchronous)、自穩定 (self-stabilizing) BFS 建構 (BFS construction)，並預告會逐條規則 (rule by rule) 說明演算法 (algorithm)。

---

## Slide 2 — Part 1：模型與問題 (model and problem)
Before: a connected, undirected, semi-uniform and anonymous network. Every vertex sees only its own fixed ports and its neighbours, and knows nothing global — not the number of vertices, not the maximum degree, not the root eccentricity. After: BFS mod 3, constant-space, deterministic, self-stabilizing. Every vertex knows its distance to the root modulo three, stored in a constant number of bits, whatever the size, the degree and the root eccentricity. The left picture is the input, an undirected graph with extra links; the right picture is the same graph once the protocol has stabilised, where blue, green and orange stand for distance modulo three equal to 0, 1 and 2.

> 之前 (before)：一個連通 (connected)、無向 (undirected)、半均勻 (semi-uniform) 且匿名 (anonymous) 的網路 (network)。每個頂點 (vertex) 只看得到自己固定的埠 (port) 與鄰居 (neighbour)，完全不知道全域 (global) 資訊——不知道頂點總數 (number of vertices)、最大度數 (maximum degree)，也不知道根離心率 (root eccentricity)。之後 (after)：BFS 模三 (modulo three)，常數空間 (constant space)、確定性 (deterministic)、自穩定 (self-stabilizing)。每個頂點知道到根 (root) 的距離 (distance) 取模三，並以常數個位元 (constant bits) 儲存 (store)，與網路規模 (size)、度數 (degree)、根離心率 (eccentricity) 無關 (independent)。左圖是輸入 (input)，一個帶額外連線 (extra link) 的無向圖；右圖是同一張圖在協定 (protocol) 穩定 (stabilise) 後的樣子，其中藍 (blue)、綠 (green)、橘 (orange) 分別代表距離模三為 0、1、2。

---

## Slide 3 — Part 2：演算法總覽 (algorithm overview)
Two programs: tokbinR runs at the root and tokbinV at every other vertex; the root releases tokens, everybody else handles them. Tokens arriving from several parents are merged, never multiplied. Each vertex keeps a single bit and flips it every time a token arrives. A vertex may pass a token on only when its neighbour's bit permits it, and that one condition caps the lifespan of every token — so no cycle can keep a token alive forever. The network never falls silent either, and it settles in big-O of two to the power epsilon rounds, where epsilon is the root eccentricity. In the picture, Round 1 to Round 5 run top to bottom, the yellow TOKEN tag shows who holds the token in each round, and the triples that changed since the previous round are in red.

> 兩支程式 (two programs)：tokbinR 在根 (root) 執行，tokbinV 在其他所有頂點執行；根釋出 (release) token，其他所有頂點負責處理 (handle)。從多個父節點 (parent) 同時到達的 token 會被合併 (merged)，而不是增殖 (multiplied)。每個頂點只保留單一個位元 (single bit)，並且每收到一個 token 就翻轉 (flip) 它。頂點只有在鄰居 (neighbour) 的位元允許 (permit) 時，才可以把 token 轉送 (pass on) 出去；就是這一個條件 (condition) 限制了每個 token 的壽命 (lifespan)——所以沒有任何環 (cycle) 能讓 token 永遠存活 (alive)。網路也從不會靜默 (fall silent)，而且它會在 $O(2^{\varepsilon})$ 個回合 (round) 內穩定 (settle)，其中 epsilon 是根 (root) 的離心率 (eccentricity)。圖中 Round 1 到 Round 5 由上而下排列，黃色 TOKEN 標籤 (tag) 顯示每一回合誰持有 (hold) token，而與上一回合相比有變化 (changed) 的三元組 (triple) 標為紅色 (red)。

---

## Slide 4 — Part 3：本地變數 (local variables)
Every vertex stores three fields: k (rank, taking 0 / 1 / 2 / bottom, equal to the distance modulo three at stabilization, bottom meaning reset), t (false / true / wait, cycling), b (0 / 1 / top, 0 and 1 alternating, top flagging an error). The state space is 4×3×3 = 36 states and 6 bits, with a worked example showing how the six-bit string is composed. The graph on the right is an example with ten vertices and root eccentricity three.

> 每個頂點存三個欄位 (field)：k（rank，取值 0/1/2/bottom，穩定時為距離模三，bottom 代表重置 (reset)）、t（false/true/wait 循環 (cycle)）、b（0/1/top，0 與 1 交替 (alternate)，top 標記 (flag) 錯誤 (error)）。狀態空間 (state space) 為 4×3×3 = 36 狀態、6 位元，並舉例 (worked example) 說明六位元字串 (bit string) 的組成。右圖為根離心率 (eccentricity) 三、十個頂點的範例 (example)。

---

## Slide 5 — Part 4：親屬結構 (kinship structure)
Three concepts, defined in order:

1. **Kinship**: parent, child and sibling are defined by the rank difference modulo three; only the two adjacent levels are meaningful.
2. **The reset state**: k = bottom, t = false, b = 0; a reset vertex cuts every family relation.
3. **Parent selection**: an anonymous vertex takes as parent the neighbour behind the smallest port number — computed on the fly, never stored.

> 依序定義三組概念 (concept)：
>
> 1. **親屬關係 (kinship)**：依 rank 差值 (difference)（模三）定義父 (parent)、子 (child)、兄弟 (sibling)；只有相鄰兩層 (adjacent levels) 有意義。
> 2. **重置狀態 (reset state)**：k=bottom、t=false、b=0，重置頂點斷開 (cut) 所有家庭關係 (family relation)。
> 3. **父節點選擇 (parent selection)**：匿名 (anonymous) 頂點以埠號 (port number) 最小者選父，即時計算 (computed on the fly)、不儲存 (never stored)。

---

## Slide 6 — Part 5：根演算法 TokBinR (root algorithm TokBinR)
Three predicates of the root algorithm: N-Synch (the whole neighbourhood is at rank 1 and agrees on the bit and on the flag), N-Reset (every neighbour is reset), N-Er (every neighbour carries the error bit). Two rules: R-er-root (when none of the three predicates holds, raise the error flag), R-go (with the error flag set and N-Er holding, restart).

> 根演算法 (root algorithm) 的三個述詞 (predicate)：N-Synch（整個鄰域 (neighbourhood) 都在 rank 1 且位元與旗標 (flag) 一致）、N-Reset（所有鄰居皆重置）、N-Er（所有鄰居都帶錯誤位元 (error bit)）。兩條規則 (rule)：R-er-root（三個述詞皆不成立時舉起 (raise) 錯誤旗標）、R-go（錯誤旗標且 N-Er 成立時重新啟動 (restart)）。

---

## Slide 7 — Part 6：token 接收述詞 (token reception predicates)
Before the seven rules of a non-root vertex, six token-related predicates are defined: Tok (the set of neighbours offering a token), TakeP (the token sources are exactly the set of parents), TakeO (the sources are not the set of parents), Tok-rk (the minimum rank among the sources), TakeR (the sources are exactly the neighbours of minimum rank), k-t (the rank finally adopted = Tok-rk + 1 modulo three).

> 在非根 (non-root) 頂點七條規則之前，先定義六個 token 相關述詞：Tok（提供 token 的鄰居集合 (set)）、TakeP（token 來源 (source) 恰為父節點集合）、TakeO（來源非父節點集合）、Tok-rk（來源中最小 rank）、TakeR（來源恰為最小 rank 的鄰居集合）、k-t（最後採用 (adopt) 的 rank = Tok-rk + 1 模三）。

---

## Slide 8 — Part 7：非根演算法 TokBinV (non-root algorithm TokBinV)
The seven rules of a non-root vertex, one by one: R-er (declare an error), R-reset (reset), R-er-rank (a stabilized vertex that receives a non-parent token reports an error), R-join (rejoin from the reset state), R-tok (accept a parent's token), R-add (flip the bit), R-ready (end the wait). The picture on the right is the state automaton: white for unknown, red for error, grey for reset; solid arrows are the normal cycle, the red dotted ones lead to the error, the blue dashed ones are the reset and the rejoin.

> 逐條說明 (one by one) 非根頂點的七條規則：R-er（宣告 (declare) 錯誤）、R-reset（重置）、R-er-rank（已穩定 (stabilized) 頂點收到非父 token 即報錯 (report an error)）、R-join（從重置重新加入 (rejoin)）、R-tok（接受 (accept) 父節點 token）、R-add（翻轉位元）、R-ready（結束等待 (wait)）。右圖為狀態自動機 (state automaton)：白色未知 (unknown)、紅色錯誤、灰色重置；實線 (solid arrow) 為正常循環 (normal cycle)，紅虛點 (red dotted) 指向錯誤，藍虛線 (blue dashed) 為重置與重新加入。

---

## Slide 9 — Part 8：錯誤偵測述詞 I (error detection predicates I)
Five error predicates about internal state and token flow: Er-var (bottom, but not a legal reset), Er-root (a neighbour of rank 0, yet more than one parent), Er-noP (not bottom, yet no parent), Er-tp (a child sends a token but v is not waiting for it), Er-wait (waiting for a reset neighbour, which will never answer).

> 五個與內部狀態 (internal state)、token 流向 (token flow) 有關的錯誤述詞：Er-var（bottom 但非合法 (legal) 重置）、Er-root（鄰居 rank 0 卻有多個父節點）、Er-noP（非 bottom 卻無父節點）、Er-tp（子節點送 token 但自己未在等待 (waiting)）、Er-wait（等待一個重置鄰居，永遠不會有回應 (answer)）。

---

## Slide 10 — Part 9：錯誤偵測述詞 II (error detection predicates II)
Seven predicates in the second half: Er-P / Er-C / Er-S (parents, children or siblings disagree on the bit or on the flag), Er-Re (a vertex still owns a child while an error comes from below), Er-N (the combined test for disagreeing neighbours), Er-pi (a reset vertex observes neighbours of equal rank that disagree), Er-prg (a neighbour carries top, so the error propagates). Finally the global predicate Er is defined as the logical OR of the seven predicates. In short: these thirteen predicates give the protocol its self-stabilizing power — raise, propagate, reset, rebuild.

> 下半部 (second half) 七個述詞：Er-P／Er-C／Er-S（父、子、兄弟之間在位元或旗標上不一致 (disagree)）、Er-Re（仍有子節點卻從下方傳來錯誤）、Er-N（鄰居不一致的綜合判斷 (combined test)）、Er-pi（重置頂點觀察到 (observe) 同 rank 鄰居不一致）、Er-prg（鄰居帶 top，錯誤傳播 (propagate)）。最後定義全域述詞 (global predicate) Er 為七個述詞的邏輯或 (logical OR)。總結 (in short)：這十三個述詞賦予 (give) 協定自穩定能力 (self-stabilizing power)——舉錯、傳播、重置、重建 (rebuild)。

---

## Slide 11 — Part 10：JavaScript 模擬 (JavaScript simulation)
From the paper to an implementation: a complete JavaScript simulator running the same rule set. The colours match the slides, and k, t and b can be read beside every vertex. A ready-made or a random connected graph can be chosen; execution is synchronous (all vertices evaluate their guards at the same time and then apply their assignments together), and the log records which rule fired where. Demo flow: play and watch the tokens move towards the root, the bits flip and the flags cycle, until after N rounds the ranks converge; then inject an error to show the red vertices appearing, the error propagating, the reset, the rejoin and the convergence again — validating self-stabilization.

> 從論文 (paper) 走到實作 (implementation)：完整的 JavaScript 模擬器 (simulator)，執行同一套規則 (rule set)。顏色 (colour) 與投影片一致，每個頂點旁可讀 k、t、b 三欄位。可選擇現成 (ready-made) 或隨機 (random) 連通圖 (connected graph)，同步執行 (synchronous execution)（所有頂點同時評估 (evaluate) 守衛 (guard) 再一起賦值 (assignment)），log 記錄哪條規則在何處觸發 (fire)。Demo 流程 (flow)：播放 (play) 觀察 token 朝根移動、位元翻轉、旗標循環，經過 N 回合 rank 收斂 (converge)；接著注入 (inject) 錯誤，展示紅色頂點出現、錯誤傳播、重置、重新加入、再次收斂——驗證 (validate) 自穩定。

---

## Slide 12 — Part 11：結論 (conclusion)
Five closing points:

1. The first constant-space, deterministic, self-stabilizing BFS construction for semi-uniform synchronous networks of arbitrary topology; the vertices need no global parameter.
2. Because point-to-neighbour and distance-to-root are unavailable, an infinite token stream is introduced to break cycles; it may be useful for other tasks.
3. A by-product: an optimal two-colouring of bipartite graphs in constant memory (earlier solutions need Omega of log n bits).
4. Open problem one: the method assumes full synchrony; whether it extends to an asynchronous setting in constant space is still unclear.
5. Open problem two: space is constant but time is exponential (big-O of two to the power epsilon); constant space together with polynomial time is left for future research.

> 五點總結 (closing points)：
>
> 1. 首個 (first) 適用於任意拓樸 (arbitrary topology) 半均勻 (semi-uniform) 同步網路 (synchronous network) 的常數空間 (constant space)、確定性 (deterministic)、自穩定 (self-stabilizing) BFS 建構 (construction)，頂點不需任何全域參數 (global parameter)。
> 2. 因 point-to-neighbour 與 distance-to-root 不可用 (unavailable)，引入無限 (infinite) token 流 (token stream) 以打破環 (break cycles)，可能對其他任務 (task) 有用。
> 3. 副產品 (by-product)：常數記憶體 (constant memory) 下可得二分圖 (bipartite graph) 的最佳二著色 (optimal two-colouring)（先前解法 (solution) 需 Omega of log n 位元）。
> 4. 開放問題一 (open problem)：方法假設完全同步 (full synchrony)，能否以常數空間推廣 (extend) 到非同步環境 (asynchronous setting) 仍不清楚。
> 5. 開放問題二：空間常數但時間指數 (exponential)（$O(2^{\varepsilon})$），常數空間加多項式時間 (polynomial time) 留待未來研究 (future work)。

---

## Slide 13 — 結尾 (closing)
In one sentence: a deterministic, synchronous, self-stabilizing protocol that builds a BFS tree within big-O of two to the power epsilon rounds with only six bits per vertex, independently of the size, the maximum degree and the root eccentricity of the network. Thank you — questions are welcome.

> 一句話總結 (summary in one sentence)：確定性 (deterministic)、同步 (synchronous)、自穩定 (self-stabilizing) 的協定 (protocol)，在 $O(2^{\varepsilon})$ 回合 (round) 內建立 BFS 樹 (BFS tree)，每頂點僅六位元 (six bits)，且與網路規模 (size)、最大度數 (maximum degree)、根離心率 (eccentricity) 無關 (independent)。謝謝，歡迎提問 (questions)。

