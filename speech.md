# Speech script — `main-ppt.tex`

Fully English. Every formula is written **exactly as it is spoken**: mathematical symbols are spelled out as English words.
Target: **under 15:00** — 2:00 for Parts 1–3, 10:00 for Parts 4–9, 3:00 for Part 10, 1:00 for Part 11.

Lines in *[square brackets and italics]* are stage directions — do **not** read them aloud.

Each English part is followed by a `>` block giving the **繁體中文對照**. The Chinese is a reading aid only: it is **not** spoken, and it is excluded from the English word count.

---

## Pronunciation key (presenter only — not spoken)

| On the slide | Say |
|---|---|
| $\mathcal{N}(r)$ | "N of r" (neighbourhood of r) |
| $\Pa(v),\ \Ch(v),\ \Si(v)$ | "P of v" / "C of v" / "S of v" (parents / children / siblings) |
| $\rk_v$ (written `k`) | "k of v" — the rank of v |
| $t_v,\ b_v$ | "t of v", "b of v" |
| $\bo,\ \bz$ | "one", "zero" |
| $\top,\ \bot$ | "top" (error), "bottom" (reset) |
| $\wedge,\ \vee,\ \neg$ | "and", "or", "not" |
| $\forall,\ \exists$ | "for all", "there exists" |
| $\neq,\ \equiv,\ \longrightarrow$ | "is not equal to", "is equivalent to", "becomes" |
| $\bar b$ | "the complement of b" |
| $\varepsilon,\ \delta(v),\ \pi_v(u)$ | "epsilon", "delta of v", "the port at v leading to u" |
| $O(2^{\varepsilon})$ | "big-O of two to the power epsilon" |

---

## Slide 1 — Title

> *0:00 – 0:15*

Good morning. I am Che Chon Hong, from the University of Macau. My talk is about deterministic, synchronous, self-stabilizing BFS construction with constant space; I will follow the algorithm rule by rule.

> **繁中：** 各位早安。我是澳門大學的朱俊匡（Che Chon Hong）。我這次報告的主題是：具常數空間的確定性、同步、自穩定 BFS 建構；我會逐條規則依序說明這個演算法。

---

## Slide 2 — Part 1: Model and Problem

> *0:15 – 0:45*

First, the model. The network is a connected, undirected graph G, with n vertices and edges E. Vertices talk through local ports, numbered one to delta of v, the degree; ports are fixed and carry no memory. In one step a vertex reads its own variables and its neighbours'. The setting is semi-uniform and anonymous: everybody runs the same code, only the root r is special. The goal is BFS modulo three: each vertex must store the distance to r, modulo three, in a constant number of bits, without knowing n, the maximum degree Delta, or the depth epsilon. In the picture, blue is zero, green one, orange two.

> **繁中：** 首先是模型。網路是一個連通、無向的圖 G，有 n 個頂點與邊集合 E。頂點之間透過本地埠（port）溝通，埠編號由一到 delta of v，即 v 的度數；埠是固定的、不帶任何記憶。在一個步驟中，一個頂點會讀取自己的變數以及鄰居的變數。設定是半均勻（semi-uniform）且匿名的：所有人跑同一份程式碼，只有根 r 是特別的。目標是「BFS 模三」：每個頂點都必須以常數個位元儲存自己到 r 的距離取模三，而且不需要知道 n、最大度數 Delta，或深度 epsilon。圖中，藍色代表零，綠色代表一，橘色代表二。

---

## Slide 3 — Part 2: Algorithm Overview

> *0:45 – 1:15*

The root runs TokBinR and injects tokens; every other vertex runs TokBinV. Tokens arriving from several parents are merged, not multiplied. Each vertex keeps one bit, b of v, and flips it at every reception: b of v becomes the complement of b of v. A vertex forwards a token only if its neighbour's bit equals one — that single condition bounds the lifetime of every token, so no cycle can sustain one forever. It stabilizes in big-O of two to the power epsilon rounds, with thirty-six local states, six bits — and it is *not* silent.

> **繁中：** 根執行 TokBinR 並注入符記（token）；其他所有頂點執行 TokBinV。從多個父節點同時到達的符記會被合併，而不是複製增殖。每個頂點只保留一個位元，即 b of v，並且在每次收到符記時把它翻轉：b of v 變成 b of v 的補數。頂點只有在鄰居的位元等於一時才會轉送符記 — 就是這一個條件限制了每個符記的存活時間，因此沒有任何環能夠永遠維持一枚符記。整個系統會在 big-O of two to the power epsilon 個回合內穩定，使用三十六個本地狀態、六個位元 — 而且它是*非靜默*（not silent）的。

---

## Slide 4 — Part 3: Local variables and the state space

> *1:15 – 2:00*

Each vertex stores three fields. k, the rank, in zero, one, two and bottom: at stabilization it is the distance to the root modulo three; bottom means reset. t, in false, true and wait, cycling in that order. b, in zero, one and top: zero and one alternate, top flags an error. So the state space is four times three times three: thirty-six states, six bits. For example, k equal to two is one-zero, t equal to true is zero-one, b equal to one is zero-one — the six-bit word one zero zero one zero one. On the right, ten vertices at depth three: the circle holds k of v, its colour is that rank, and the triple above it is k, t, b.

> **繁中：** 每個頂點儲存三個欄位。k，即階級（rank），取值為零、一、二與 bottom：穩定時它就是到根的距離取模三；bottom 代表重置（reset）。t，取值為 false、true 與 wait，並依此順序循環。b，取值為零、一與 top：零與一交替出現，top 則標記錯誤。所以狀態空間是四乘三乘三：三十六個狀態，六個位元。舉例來說，k 等於二寫成 one-zero，t 等於 true 寫成 zero-one，b 等於 one 寫成 zero-one — 於是得到六位元字串 one zero zero one zero one。右圖是深度三、共十個頂點的範例：圓圈內是 k of v，圓圈的顏色就是該階級，而圓圈上方的三元組是 k、t、b。

---

## Slide 5 — Part 4: Terms — kinship structure and the reset state

> *2:00 – 3:45*

Now the vocabulary, following the four graphs on the right.

> **繁中：** 接著說明名詞定義，並逐一對照右邊的四張圖。

**First, kinship.** The rank turns a neighbourhood into a family. A neighbour u is a parent of v when k of u equals k of v minus one, modulo three; a child when k of u equals k of v plus one; a sibling when k of u equals k of v. In the first graph the middle vertex has k equal to one: the green vertex below, k equal to two, is its child; the blue one above, k equal to zero, is its parent; the green one on the right is a sibling. A neighbour of rank k of v plus two is *not* a parent: only the two adjacent levels count.

> **繁中：** **第一，親屬關係。** 階級把一個鄰域變成一個家庭。當 k of u 等於 k of v 減一後取模三時，鄰居 u 是 v 的父節點；當 k of u 等於 k of v 加一時，它是子節點；當 k of u 等於 k of v 時，它是兄弟節點。第一張圖中，中間頂點的 k 等於一：下方綠色頂點的 k 等於二，所以是它的子節點；上方藍色頂點的 k 等於零，所以是它的父節點；右側綠色頂點則是它的兄弟節點。要注意，階級為 k of v 加二的鄰居*不是*父節點：只有相鄰的兩個層級才有意義。

**Second, the reset state.** Reset of v is equivalent to: k of v equals bottom, t of v equals false, b of v equals zero. A vertex that detects an error disconnects itself: a reset vertex has no family at all. In the second graph the dashed grey vertex ignores every token from its neighbours.

> **繁中：** **第二，重置狀態。** Reset of v 等價於：k of v 等於 bottom，t of v 等於 false，b of v 等於零。偵測到錯誤的頂點會把自己斷開：一個重置的頂點完全沒有家人。第二張圖中，虛線灰色的頂點會忽略來自鄰居的所有符記。

**Third, parent selection.** Vertices are anonymous, so they use ports: the parent of v minimizes the port number at v among the parents of v. In the third graph the left candidate sits behind port two, the right one behind port one, so port order picks the right-hand vertex — the solid arrow. It is computed on the fly, never stored.

> **繁中：** **第三，父節點選擇。** 由於頂點是匿名的，所以改用埠：v 的父節點，是在 v 的父節點集合中使 v 上埠號最小的那個。第三張圖中，左邊的候選在埠二後面，右邊的候選在埠一後面，因此埠的順序會選中右邊那個頂點 — 也就是實線箭頭。這個結果是即時計算的，從不儲存。

**Fourth: the oriented BFS tree.** Root rank zero, then one, then two, every arrow pointing to its parent.

> **繁中：** **第四：定向 BFS 樹。** 根階級為零，然後是一，然後是二，每個箭頭都指向自己的父節點。

---

## Slide 6 — Part 5: TokBinR — three predicates, two rules

> *3:45 – 5:15*

Now the root algorithm. Three predicates.

> **繁中：** 現在來看根演算法。它有三個述詞（predicate）。

**Number one, N-Synch of r:** for all u and v in N of r, k of u equals k of v equals one, and b of u equals b of v, and t of u equals t of v — is my whole neighbourhood at rank one, and does it agree on the bit and the flag?

> **繁中：** **第一，N-Synch of r：** 對於 N of r 中所有的 u 與 v，k of u 等於 k of v 等於一，且 b of u 等於 b of v，且 t of u 等於 t of v — 也就是：我的整個鄰域是不是都在階級一，而且位元與旗標都是一致的？

**Number two, N-Reset of r:** for all u in N of r, Reset of u — every neighbour is dormant.

> **繁中：** **第二，N-Reset of r：** 對於 N of r 中所有的 u，Reset of u 成立 — 所有鄰居都處於休眠（dormant）狀態。

**Number three, N-Er of r:** for all u in N of r, b of u equals top — every neighbour carries the error bit.

> **繁中：** **第三，N-Er of r：** 對於 N of r 中所有的 u，b of u 等於 top — 所有鄰居都帶著錯誤位元。

Then two rules. **Rule four, R-er-root:** if b of r is not equal to top, and none of the three predicates holds, then b of r becomes top — the root raises an error. **Rule five, R-go:** if b of r equals top and N-Er holds, then b of r becomes one — the root restarts.

> **繁中：** 接著是兩條規則。**第四條，R-er-root：** 若 b of r 不等於 top，且三個述詞都不成立，則 b of r 變成 top — 也就是根舉起錯誤旗標。**第五條，R-go：** 若 b of r 等於 top 且 N-Er 成立，則 b of r 變成一 — 也就是根重新啟動。

The pictures show the synchronized neighbourhood, the reset one, the error one, and the two root rules.

> **繁中：** 圖中依序展示：同步的鄰域、重置的鄰域、錯誤的鄰域，以及根的兩條規則。

---

## Slide 7 — Part 6: Token reception predicates

> *5:15 – 6:50*

Before the seven rules of a non-root vertex: what is a token, and when is it accepted? Six definitions.

> **繁中：** 在說明非根頂點的七條規則之前，先定義：什麼是符記，以及符記在什麼時候會被接受？這裡共有六個定義。

**One, Tok of v:** the neighbours offering a token to v — u is in Tok of v when t of u equals true and b of u equals one. In the first picture, only the neighbour behind the solid arrow satisfies both.

> **繁中：** **第一，Tok of v：** 向 v 提供符記的那些鄰居 — 當 t of u 等於 true 且 b of u 等於一時，u 就屬於 Tok of v。第一張圖中，只有實線箭頭後方的那個鄰居同時滿足這兩個條件。

**Two, TakeP of v:** Tok of v is not empty, and equals the set of parents of v. Here both senders are parents.

> **繁中：** **第二，TakeP of v：** Tok of v 不是空集合，而且恰好等於 v 的父節點集合。圖中兩個發送者都是父節點。

**Three, TakeO of v:** Tok of v is not empty but not equal to the set of parents — the sender here is a child of rank two, offering a token upwards.

> **繁中：** **第三，TakeO of v：** Tok of v 不是空集合，但不等於父節點集合 — 圖中的發送者是階級二的子節點，把符記往上送。

**Four, Tok-rk of v:** the minimum, over u in Tok of v, of k of u.

> **繁中：** **第四，Tok-rk of v：** 在所有 u 屬於 Tok of v 之中，k of u 的最小值。

**Five, TakeR of v:** Tok of v is not empty, and equals the set of neighbours u with k of u equal to Tok-rk of v; here both senders have rank zero.

> **繁中：** **第五，TakeR of v：** Tok of v 不是空集合，且等於所有滿足「k of u 等於 Tok-rk of v」的鄰居 u 所成的集合；圖中兩個發送者的階級都是零。

**Six, k-t of v** equals Tok-rk of v plus one, modulo three: the rank v finally adopts. In the last picture the sender has rank two, so v takes rank zero.

> **繁中：** **第六，k-t of v** 等於 Tok-rk of v 加一後取模三：這是 v 最後採用的階級。最後一張圖中，發送者的階級是二，所以 v 取階級零。

---

## Slide 8 — Part 7: TokBinV — the seven rules

> *6:50 – 8:35*

Seven rules of TokBinV. Before the arrow is the guard, after it the assignment.

> **繁中：** 以下是 TokBinV 的七條規則。箭頭之前是守衛條件（guard），箭頭之後是賦值動作。

**Rule one, R-er:** if b of v is not top and Er of v holds, then t of v becomes wait and b of v becomes top.

> **繁中：** **第一條，R-er：** 若 b of v 不是 top 且 Er of v 成立，則 t of v 變成 wait，且 b of v 變成 top。

**Rule two, R-reset:** if b of v equals top, or Er-var of v holds, then k of v becomes bottom, t of v becomes false, and b of v becomes zero.

> **繁中：** **第二條，R-reset：** 若 b of v 等於 top，或 Er-var of v 成立，則 k of v 變成 bottom、t of v 變成 false、b of v 變成零。

**Rule three, R-er-rank:** if Er of v does not hold, k of v is not bottom, t of v equals false, and TakeO of v holds, then t of v becomes wait and b of v becomes top. A settled vertex receiving a token from a non-parent declares an error.

> **繁中：** **第三條，R-er-rank：** 若 Er of v 不成立、k of v 不是 bottom、t of v 等於 false，且 TakeO of v 成立，則 t of v 變成 wait、b of v 變成 top。一個已穩定的頂點若收到非父節點送來的符記，就宣告錯誤。

**Rule four, R-join:** if Er of v does not hold, k of v equals bottom, and TakeR of v holds, then k of v becomes k-t of v, and b of v becomes one.

> **繁中：** **第四條，R-join：** 若 Er of v 不成立、k of v 等於 bottom，且 TakeR of v 成立，則 k of v 變成 k-t of v，且 b of v 變成一。

**Rule five, R-tok:** if Er of v does not hold, k of v is not bottom, t of v equals false, and TakeP of v holds, then t of v becomes true.

> **繁中：** **第五條，R-tok：** 若 Er of v 不成立、k of v 不是 bottom、t of v 等於 false，且 TakeP of v 成立，則 t of v 變成 true。

**Rule six, R-add:** if Er of v does not hold and t of v equals true, then t of v becomes wait and b of v becomes b of v plus one, modulo two.

> **繁中：** **第六條，R-add：** 若 Er of v 不成立且 t of v 等於 true，則 t of v 變成 wait，且 b of v 變成 b of v 加一後取模二。

**Rule seven, R-ready:** if Er of v does not hold and t of v equals wait, then t of v becomes false.

> **繁中：** **第七條，R-ready：** 若 Er of v 不成立且 t of v 等於 wait，則 t of v 變成 false。

On the right, the automaton: white for the unknown-rank states, red for the error state, grey for reset. The solid arrows are the normal cycle, rules five to seven; the dotted red ones are rules one and three, into the error; the dashed blue ones are rule two, the reset, and rule four, the join.

> **繁中：** 右邊是狀態自動機：白色是階級未知的狀態，紅色是錯誤狀態，灰色是重置。實線箭頭是正常循環，也就是第五到第七條規則；紅色虛點箭頭是第一與第三條規則，指向錯誤狀態；藍色虛線箭頭則是第二條規則（重置）與第四條規則（重新加入）。

---

## Slide 9 — Part 8: Error detection I — state and token flow

> *8:35 – 10:05*

Now the error predicates, in two slides. First the internal-state and token-flow ones — five of them.

> **繁中：** 現在說明錯誤偵測述詞，分成兩張投影片。首先是關於內部狀態與符記流向的五個述詞。

**One, Er-var of v:** k of v equals bottom, and Reset of v does not hold. In the first picture v shows bottom but is red, not grey, precisely because it is *not* a valid reset: t of v is not false.

> **繁中：** **第一，Er-var of v：** k of v 等於 bottom，但 Reset of v 不成立。第一張圖中，v 顯示 bottom 卻是紅色而不是灰色，正是因為它*不是*合法的重置：t of v 不是 false。

**Two, Er-root of v:** a neighbour u of v has k of u equal to zero — only the root may — and v has more than one parent. Here v has two parents and one is the root, the square on the left.

> **繁中：** **第二，Er-root of v：** 有一個 v 的鄰居 u 使得 k of u 等於零 — 只有根可以如此 — 而且 v 的父節點多於一個。圖中 v 有兩個父節點，其中一個是根，即左邊的方形節點。

**Three, Er-noP of v:** k of v is not bottom, yet the set of parents of v is empty. Both neighbours have rank two, so v has no parent.

> **繁中：** **第三，Er-noP of v：** k of v 不是 bottom，但 v 的父節點集合是空的。圖中兩個鄰居的階級都是二，所以 v 沒有父節點。

**Four, Er-tp of v:** Er-prg of v does not hold, b of v is not top, and a child u of v has t of u equal to true while v is not waiting for it — not t equals wait with b equals zero.

> **繁中：** **第四，Er-tp of v：** Er-prg of v 不成立、b of v 不是 top，而且有一個 v 的子節點 u 使得 t of u 等於 true，此時 v 並沒有在等它 — 即並非「t 等於 wait 且 b 等於零」的情況。

**Five, Er-wait of v:** t of v equals wait, b of v equals zero, and there exists a reset neighbour u of v. Waiting for a dormant neighbour is an error: it will never answer.

> **繁中：** **第五，Er-wait of v：** t of v 等於 wait、b of v 等於零，且存在一個重置的鄰居 u。等待一個休眠的鄰居是一種錯誤：它永遠不會回應。

---

## Slide 10 — Part 9: Error detection II — synchronization and propagation

> *10:05 – 11:55*

Second half: synchronization and propagation, with the last six pictures.

> **繁中：** 下半部分：同步性與傳播性的述詞，搭配最後六張圖。

**One, two and three** are variants of one idea, so they share a picture. Er-P of v: two parents u and w of v disagree on b, or on t. Er-C of v: the same among the children. Er-S of v: a sibling u of v disagrees with v on b or on t. In the picture the two neighbours of v carry different tuples — t, one, and f, zero.

> **繁中：** **第一、二、三條** 是同一個概念的三種變體，所以共用一張圖。Er-P of v：v 的兩個父節點 u 與 w，在 b 或 t 上不一致。Er-C of v：同樣的條件，但對象換成子節點。Er-S of v：v 的兄弟節點 u 與 v 在 b 或 t 上不一致。圖中 v 的兩個鄰居帶著不同的三元組 — t, one 與 f, zero。

**Four, Er-Re of v:** k of v is not bottom, the set of children of v is not empty, t of v is not true, and a neighbour u of v has k of u equal to bottom. Here v still owns a child of rank two while an error is coming from below.

> **繁中：** **第四，Er-Re of v：** k of v 不是 bottom、v 的子節點集合不是空的、t of v 不是 true，且有一個 v 的鄰居 u 使得 k of u 等於 bottom。圖中 v 仍擁有一個階級二的子節點，而錯誤正從下方傳來。

**Five, Er-N of v:** Er-prg does not hold, b of v is not top, TakeO of v does not hold, and one of Er-P, Er-C, Er-S, Er-Re holds — the neighbours disagree.

> **繁中：** **第五，Er-N of v：** Er-prg 不成立、b of v 不是 top、TakeO of v 不成立，且 Er-P、Er-C、Er-S、Er-Re 之中有一個成立 — 鄰居之間不一致。

**Six, Er-pi of v:** Reset of v holds, and there exist two neighbours u and w of v with k of u equal to k of w but different bits or different flags — here two neighbours of rank one, t-one and t-zero.

> **繁中：** **第六，Er-pi of v：** Reset of v 成立，且存在 v 的兩個鄰居 u 與 w，使得 k of u 等於 k of w，但位元不同或旗標不同 — 圖中是兩個階級為一的鄰居，分別是 t-one 與 t-zero。

**Seven, Er-prg of v:** there exists a neighbour u of v with b of u equal to top — the error propagates.

> **繁中：** **第七，Er-prg of v：** 存在一個 v 的鄰居 u 使得 b of u 等於 top — 錯誤會往外傳播。

**Eight, the global predicate Er of v:** the OR of Er-root, Er-tp, Er-N, Er-noP, Er-prg, Er-pi and Er-wait, so a vertex is in error as soon as any one of the seven holds.

> **繁中：** **第八，全域述詞 Er of v：** 它是 Er-root、Er-tp、Er-N、Er-noP、Er-prg、Er-pi 與 Er-wait 的邏輯或（OR），所以只要這七個之中有任何一個成立，該頂點就處於錯誤狀態。

In short: these thirteen predicates give the protocol its self-stabilizing power — an error bit is raised, it propagates, vertices reset, and the tree is rebuilt.

> **繁中：** 簡而言之：這十三個述詞賦予了這個協定自穩定的能力 — 舉起錯誤位元、向外傳播、頂點重置，然後整棵樹被重新建構。

---

## Slide 11 — Part 10: Live JavaScript demo

> *11:55 – 14:00*

Let me move from the paper to an implementation. I wrote a complete JavaScript simulation of the rule set we have just seen; the blue link on the slide opens it, live.

> **繁中：** 最後，讓我從論文走到實作。我為剛剛看到的這套規則集寫了一個完整的 JavaScript 模擬器；投影片上的藍色連結可以開啟它，而且是即時運行的。

*[Switch to the demo.]*

> **繁中：** *[切換到 demo。]*

This is the simulator executing the exact algorithm. Every vertex uses the same colour code as my slides — blue for rank zero, green for one, orange for two, red for top, grey for reset — and beside each vertex you can read its three fields: k, t and b. Here I choose the network, a ready-made example or a random connected graph, and here I drive the execution.

> **繁中：** 這就是執行同一套演算法的模擬器。每個頂點都使用與我投影片相同的顏色規則 — 藍色代表階級零、綠色代表一、橘色代表二、紅色代表 top、灰色代表重置 — 而且每個頂點旁邊都可以讀到它的三個欄位：k、t 與 b。這裡可以選擇網路，用現成的範例或隨機產生的連通圖；這裡則用來操作執行。

It is synchronous: in one step all vertices evaluate their guards at the same time, exactly as the paper requires, and then apply their assignments together. That is why a vertex can raise an error even though its own state looks fine — its neighbour changed in the same round. The log records which rule fired where.

> **繁中：** 它是同步的：在一個步驟中，所有頂點同時評估自己的守衛條件，完全符合論文的要求，然後一起套用各自的賦值。這就解釋了為什麼某個頂點明明自己的狀態看起來沒問題，卻會舉起錯誤旗標 — 因為它的鄰居在同一個回合改變了。下方的記錄會顯示哪條規則在哪些頂點被觸發。

Let me run it. *[Play.]* You can see the tokens — the solid arrows — moving towards the root, the bit b of v flipping at every reception, and the flags cycling false, true, wait. After *[N]* rounds the ranks stop changing: every vertex holds the distance to the root, modulo three; this counter gives the round count.

> **繁中：** 讓我執行它。*[播放。]* 你可以看到符記 — 實線箭頭 — 朝根的方向移動，b of v 在每次接收時翻轉，旗標則依 false、true、wait 循環。經過 *[N]* 個回合之後，階級不再改變：每個頂點都存著自己到根的距離取模三，而這個計數器顯示回合數。

Now the real point, self-stabilization. I corrupt one vertex. *[Inject an error.]* Its neighbours detect it at once — the red vertices appear — the error bit propagates outward, the affected vertices reset, then rejoin and the ranks converge again. The system recovers from an arbitrary configuration, with no external help, exactly as the paper proves.

> **繁中：** 現在來看真正的重點：自穩定。我破壞其中一個頂點的狀態。*[注入錯誤。]* 它的鄰居立刻偵測到 — 紅色頂點出現 — 錯誤位元向外傳播，受影響的頂點重置，之後重新加入，階級再次收斂。系統能從任意初始組態中自行復原，不需要任何外部協助，正好就是論文所證明的結果。

So: the model, thirty-six local states, seven rules, thirteen error predicates — and a running implementation of all of it.

> **繁中：** 總結來說：這個模型、三十六個本地狀態、七條規則、十三個錯誤述詞 — 以及一整套實際可執行的實作。

---

## Slide 12 — Part 11: Conclusion

> *14:00 – 14:45*

Five closing points. First, this is the first constant-space deterministic self-stabilizing BFS construction for semi-uniform synchronous networks of arbitrary topology: the vertices know none of the global parameters — not n, not Delta, not epsilon. Second, since point-to-neighbour and distance-to-the-root are unavailable here, we introduced an infinite token stream; it breaks the cycles and may be useful for other tasks. Third, as a by-product, running TokBin gives an optimal two-colouring of a bipartite graph in constant memory, against Omega of log n bits in earlier solutions. Fourth, an open problem: the method assumes lock-step synchrony, and whether it extends to an asynchronous setting in constant space is unclear. Fifth, a second open problem: space is constant but time is exponential, big-O of two to the power epsilon; constant space together with polynomial time is left for future research.

> **繁中：** 最後用五點作結。第一，這是第一個針對「任意拓樸之半均勻同步網路」的常數空間、確定性、自穩定 BFS 建構：頂點不需要知道任何全域參數 — 不需要 n、不需要 Delta、也不需要 epsilon。第二，由於在這裡 point-to-neighbour 與 distance-to-the-root 都不可用，我們引入了無限符記流；它能夠打破環，也可能對其他任務有用。第三，作為一項副產品，持續執行 TokBin 可以在常數記憶體下得到二分圖的最佳二著色，而先前的解法需要 Omega of log n 個位元。第四，一個開放問題：這個方法假設完全同步，它能否在常數空間下推廣到非同步環境仍不清楚。第五，第二個開放問題：空間是常數，但時間是指數級 — big-O of two to the power epsilon；同時做到常數空間與多項式時間，就留待未來研究。

---

## Slide 13 — Closing

> *14:45 – 15:00*

In summary: a deterministic, synchronous, self-stabilizing protocol that builds a BFS tree in big-O of two to the power epsilon rounds, using six bits per vertex, independently of the size, the maximum degree and the depth of the network. Thank you — I am happy to take questions.

> **繁中：** 總結：這是一個確定性、同步、自穩定的協定，能在 big-O of two to the power epsilon 個回合內建立 BFS 樹，每個頂點只使用六個位元，且與網路的規模、最大度數、深度都無關。謝謝各位 — 我很樂意回答問題。

---

## Timing check (measured word counts)

The Chinese `>` blocks are excluded from the counts below; they are never spoken.

| Block | Slides | Spoken words | Slot | Time @ 155 wpm |
|---|---|---|---|---|
| Opening + Parts 1–3 | 1–4 | 359 | 2:00 | 2:19 |
| Parts 4–9 | 5–10 | 1,419 | 10:00 | 9:09 |
| Part 10 (live demo) | 11 | 284 | 3:00 | 1:50 talking + live demo |
| Part 11 (conclusion) | 12 | 137 | 1:00 | 0:53 |
| Closing | 13 | 47 | — | 0:18 |
| **Total** | | **2,246** | **15:00** | **≈ 14:29** |

Read at a normal presentation pace of about **155 words per minute**, the script lands at **14:29**.
The remaining ~30 seconds are the silent gaps of the live demo — waiting for a run to finish, switching windows — so in practice the talk ends at about **15:00**. Part 10 is deliberately under its 3-minute slot for that reason, and Parts 4–9 now run ~50 s under their slot, which absorbs any slower delivery.

**If you still run long**, make these cuts in order (they only remove help, never content):

1. **Slide 10** — for the five predicates, read only the name and the plain-English meaning, skipping the formal guard that the picture already shows. **−30 s**
2. **Slide 8** — cut the automaton sentence to *"on the right is the state automaton; the arrows are the seven rules"*. **−15 s**
3. **Slide 4** — skip the six-bit worked example and keep only *"four times three times three is thirty-six, six bits"*. **−12 s**
4. **Slide 5** — skip the "Fourth: the oriented BFS tree" paragraph; the graph speaks for itself. **−7 s**
