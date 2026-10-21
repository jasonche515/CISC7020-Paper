# Q & A — after the `main-ppt.tex` talk (10 likely questions)

Ten questions a CISC7020 panel is most likely to ask, in the order they usually come.
Every answer is sized for **≤ 1 minute** spoken (~120–140 words at 155 wpm).

**格式 / Format.** Each item gives the question in English **and** in 繁體中文, then the answer in English (what you say) and in 繁體中文 (reading aid only — it is **not** spoken, same convention as `speech.md`).

**Rules for answering.** Answer in one breath, then stop.
Lead with the *reason*, not the definition — the panel already has the paper.
If a question goes beyond the paper, say so explicitly: *"that is one of the stated open problems"*. Never improvise a bound.

---

## Q1 — Why modulo three, and not the exact distance?

**Q.** Why does TokBin store the distance to the root only modulo three, instead of the real distance?

> **問：** 為什麼 TokBin 只儲存到根的距離取模三，而不是真正的距離？

**A.** Because the exact distance costs logarithmically many bits, and that is precisely the technique we are forbidden to use. All a vertex really needs is the ability to classify each neighbour: for a vertex at distance d, a neighbour sits at distance d minus one, d, or d plus one — never further. Modulo three those three values are pairwise distinct, so two bits already separate parents, children and siblings. And because d plus two is congruent to d minus one modulo three, the two "adjacent" levels exhaust the family. That is the relaxed target BFS-equivalent-mod-three: every vertex holds its distance modulo three, and the paper shows a spanning tree is still recoverable from it.

> **答：** 因為精確距離需要對數級別的位元，而那正是我們被禁止使用的技巧。頂點真正需要的只是把每個鄰居分類：對距離為 d 的頂點而言，鄰居的距離只可能是 d 減一、d、或 d 加一，不會更遠。這三個值取模三之後兩兩不同，所以兩個位元就足以區分父、子、兄弟。又因為 d 加二與 d 減一在模三下同餘，所以「相鄰的兩層」就足以窮盡整個家庭關係。這就是放寬後的目標：BFS 等價模三 — 每個頂點存自己距離取模三，而論文證明仍可由此還原一棵生成樹。

---

## Q2 — Thirty-six states, six bits — could you do it in five?

**Q.** You use 36 local states, which needs 6 bits. Was 5 not enough?

> **問：** 你用了 36 個本地狀態，需要 6 個位元。5 個位元不夠嗎？

**A.** Not for this state space: two to the fifth is 32, and 32 is smaller than 36, so five bits cannot even address the configurations. Concretely, the three fields each need their own alphabet — the rank over zero, one, two and bottom; the flag over false, true and wait; the bit over zero, one and top — and each of those needs two bits, so six in total. You could of course re-encode, since the information content is only about five point one seven bits, but that would destroy the point: neighbours must be able to *read* the three fields separately, so the encoding has to stay field-wise. And the constant factor is not the message — the message is that the requirement is independent of n, of Delta and of epsilon.

> **答：** 對這個狀態空間來說不夠：二的五次方是 32，比 36 小，所以五個位元連組態都無法全部表示。具體來說，三個欄位各自需要自己的取值集合 — 階級是零、一、二與 bottom；旗標是 false、true 與 wait；位元是零、一與 top — 每個都需要兩個位元，合起來就是六個。當然可以重新編碼，因為資訊量只有大約五點一七位元，但那樣就失去意義了：鄰居必須能*分別讀取*這三個欄位，所以編碼必須保持逐欄位的形式。而且常數因子並不是重點 — 重點是這個需求與 n、Delta、epsilon 都無關。

---

## Q3 — Why is the stabilization time exponential, and can it be improved?

**Q.** Your bound is O(2^epsilon) steps. Why exponential, and can it be made polynomial?

> **問：** 你的界是 $O(2^{\varepsilon})$ 步。為什麼是指數的？能不能做到多項式？

**A.** The root has no idea how deep the network is, and one bit is not enough to count. Each level acts like a binary counter: when a token is destroyed the root must start again, so in the worst case the root emits on the order of two to the epsilon tokens before the farthest vertex is served. That is the exponential decay mechanism — it is what makes constant space affordable. The paper shows you can weaken it: if the domain of b becomes zero up to k plus top, and a token is accepted for any value that is neither zero nor top, far fewer tokens are destroyed — but the bound stays exponential. Whether constant space and polynomial time can coexist is left open; the known polynomial solutions use O(log Delta + log log n) bits.

> **答：** 根完全不知道網路有多深，而一個位元不足以計數。每一層就像一個二進位計數器：符記一旦被銷毀，根就必須重來，所以最壞情況下根要送出大約 $2^{\varepsilon}$ 個符記，才能服務到最遠的頂點。這就是指數衰減機制 — 也是常數空間得以實現的原因。論文也指出可以削弱它：若把 b 的取值域擴展為零到 k 再加上 top，並且只要值不是零也不是 top 就接受符記，被銷毀的符記會少很多 — 但界仍然是指數的。常數空間與多項式時間能否並存，論文留為開放問題；已知的多項式解法需要 O(log Delta + log log n) 個位元。

---

## Q4 — Why is the algorithm not silent, and is that a problem?

**Q.** Your algorithm is not silent. Why, and does that break anything?

> **問：** 你的演算法不是靜默的。為什麼？這會造成問題嗎？

**A.** Silence means every execution eventually reaches a point after which no variable ever changes again. TokBin cannot be silent, because the token stream *is* the mechanism: once the system has stabilized the ranks are frozen, but the root keeps injecting tokens and the rules R-tok, R-add and R-ready keep firing forever, so the flag and the bit keep cycling. All the other rules — error detection, reset, join, R-er-rank — fire only during the stabilizing phase. So the non-silence is by design, not a leak: it buys us the absence of pointers and of distances. It is also a well-known trade-off — the literature on silent algorithms shows that silence is exactly what forces extra memory. Nothing is broken: the specification is about the ranks, and those are stable.

> **答：** 靜默指的是每個執行最終都會到達某一點，之後任何變數都不再改變。TokBin 不可能靜默，因為符記流*就是*機制本身：系統穩定之後階級固定不變，但根會持續注入符記，R-tok、R-add、R-ready 會永遠持續觸發，所以旗標與位元持續循環。其他所有規則 — 錯誤偵測、重置、重新加入、R-er-rank — 只在穩定期間觸發。所以非靜默是設計上的選擇，不是缺陷：它換來的是不需要指標、也不需要距離。這同時也是眾所周知的取捨 — 關於靜默演算法的文獻顯示，靜默正是迫使記憶體增加的原因。系統並沒有被破壞：規格談的是階級，而階級是穩定的。

---

## Q5 — Why does the root need its own algorithm?

**Q.** Why is the root given a separate program, TokBinR, with its own three predicates?

> **問：** 為什麼根要有一個獨立的程式 TokBinR，還帶自己的三個述詞？

**A.** Because the setting is semi-uniform: exactly one vertex is distinguished, and it is the source of the token stream. The seven rules of TokBinV all assume that a vertex has parents to receive tokens from, and that it can be told off by an error coming from above — neither holds for the root, which has no parent. So its job is different: it watches its neighbourhood and decides whether it is synchronized, meaning every neighbour is at rank one and they all agree on the bit and the flag; or fully reset; or fully in error. If none of the three holds, there is an inconsistency it cannot repair locally, so it sets its bit to top. That is the seed of the reset wave. Without it, a corrupted root neighbourhood would never be cleaned.

> **答：** 因為設定是半均勻的：恰好有一個頂點是特別的，而它就是符記流的來源。TokBinV 的七條規則都假設頂點有父節點可以接收符記，也會被來自上方的錯誤所提醒 — 這兩點對根都不成立，因為根沒有父節點。所以根的職責不同：它監看自己的鄰域，判斷它是否同步（每個鄰居都在階級一，且對位元與旗標的取值一致）、是否全部重置、或是否全部處於錯誤。若三者皆不成立，就存在它無法在本地修復的不一致，於是它把自己的位元設為 top。這就是重置波的種子。沒有它，根附近被破壞的鄰域永遠不會被清理。

---

## Q6 — Why can a token never circulate forever in a cycle?

**Q.** Cycles are the classic trap. Why can no token survive forever in a cycle?

> **問：** 環是典型的陷阱。為什麼沒有符記能在環裡永遠存活？

**A.** Because of the single bit. Every reception flips b of v, and a token is forwarded only if the bit allows it — otherwise the token simply disappears. So walk around a cycle: the bits form a finite pattern of length at most n, and after at most n receptions the token meets a vertex whose bit forbids forwarding and dies there. Tokens created by the root have no such problem, because only parents' tokens are taken — a token arriving from a child immediately triggers the R-er-rank rule, which raises an error instead of passing the token along. So the bit is a lifespan, not a counter: it bounds how long *any* token can exist, which is exactly what the older cycle-detection schemes, such as the Thue-Morse sequence, could only do for graphs of degree two.

> **答：** 因為那一個位元。每次接收都會翻轉 b of v，而符記只有在位元允許時才會被轉送 — 否則符記就直接消失。於是沿著環走一圈：這些位元形成一個長度不超過 n 的有限樣式，因此至多經過 n 次接收，符記就會遇到一個位元不允許轉送的頂點，並在那裡死亡。由根產生的符記沒有這個問題，因為只有父節點的符記才會被接收 — 來自子節點的符記會立刻觸發 R-er-rank 規則，舉起錯誤旗標而不是繼續傳遞。所以這個位元是一個「壽命」，不是計數器：它界定了*任何*符記能存在的時間，而這正是舊有的環偵測機制（例如 Thue-Morse 序列）只能在二度圖上做到的事。

---

## Q7 — Vertices are anonymous and store only a rank — so where is the tree?

**Q.** If every vertex is anonymous and keeps only its distance modulo three, how do you produce the actual spanning tree?

> **問：** 如果每個頂點都是匿名的、而且只保存距離取模三，那真正的生成樹在哪裡？

**A.** The output of the algorithm is the parent function, not a stored pointer. The rank already tells a vertex which neighbours are candidates — those whose rank is its own minus one — and among those candidates it picks the one reachable through the smallest local port number. Port numbers are part of the model: they are fixed, they carry no memory, and they are the only way to tell neighbours apart once identifiers are gone. So the choice is computed on the fly from the current neighbourhood and contributes nothing to the space complexity. Each vertex therefore names exactly one parent, at rank minus one, which yields a spanning tree rooted at the root; and edges between siblings of equal rank are simply not used. The two-colouring by-product comes from the same rank.

> **答：** 這個演算法的輸出是「父函數」，而不是儲存下來的指標。階級本身已經告訴一個頂點哪些鄰居是候選 — 也就是階級比自己小一的那些 — 而在這些候選之中，它挑選透過最小本地埠號可到達的那一個。埠號是模型的一部分：它們固定不變、不攜帶任何記憶，而且一旦沒有識別碼，它們就是唯一能區分鄰居的方式。所以這個選擇是即時計算出來的，對空間複雜度沒有任何貢獻。於是每個頂點恰好指定一個父節點，其階級為自己的階級減一，這就得到一棵以根為根的生成樹；而階級相同的兄弟之間的邊則不會被使用。二著色的副產品則來自同一個階級。

---

## Q8 — How do you actually prove that it stabilizes?

**Q.** Correctness looks intricate. What is the shape of the proof?

> **問：** 正確性的論證看起來很複雜。證明的整體結構是什麼？

**A.** It is a chain of attractors, each reached from the previous in bounded time. First, no obvious error remains, within O of one steps. Second, all tokens of the initial configuration have disappeared, within O of n steps — the lifespan argument. Third, all residual errors are eliminated, within O of two to the epsilon steps. Fourth, every configuration is legal, again within O of two to the epsilon steps, and the legal set is closed, so the system never leaves it. The last two are the expensive ones and share one tool: a potential function, the sum over vertices of a natural number measuring how far a vertex is from a correct rank with no pending token. It strictly decreases at every step and is bounded by a quantity in two to the epsilon, so it hits zero in O of two to the epsilon steps.

> **答：** 它是一連串的吸引集（attractor），每一個都能從前一個在有界的步數內到達。第一，明顯的錯誤消失，在 O(1) 步內。第二，初始組態中存在的符記全部消失，在 O(n) 步內 — 這就是壽命論證。第三，殘餘的錯誤被清除，在 $O(2^{\varepsilon})$ 步內。第四，所有組態都合法，同樣在 $O(2^{\varepsilon})$ 步內，而且合法組態集合是封閉的，所以系統不會離開它。第三與第四步是代價最高的部分，兩者使用同一個工具：一個位能函數（potential function），即對所有頂點累加一個自然數，用來度量某個頂點距離「階級正確且沒有待處理符記」還有多遠。它在每一步都嚴格下降，且其上界是一個 $2^{\varepsilon}$ 量級的量，因此會在 $O(2^{\varepsilon})$ 步內歸零。

---

## Q9 — What is genuinely new compared with previous work?

**Q.** What is the key novelty, and how does it compare with the existing literature?

> **問：** 關鍵的新意在於什麼？與既有文獻相比又如何？

**A.** The result: the first deterministic self-stabilizing BFS construction in constant space for semi-uniform synchronous networks of arbitrary topology, with no knowledge of n, of the maximum degree or of the root eccentricity. The obstacle is that both classical techniques are ruled out — pointer-to-neighbour and distance-to-root each need at least logarithmic memory, and the known constant-memory tricks, distance-two colouring or the Thue-Morse sequence, only work for bounded or degree-two graphs. Moreover, it is known that a constant-space spanning tree without a designated leader is impossible, which is exactly why semi-uniform and synchronous are assumed. The new idea is the infinite token stream: tokens with a bounded lifespan break the cycles, and that technique may be reusable for other tasks. A constant-space optimal two-colouring of bipartite graphs follows.

> **答：** 結果方面：這是第一個針對「任意拓樸之半均勻同步網路」的常數空間、確定性、自穩定 BFS 建構，而且完全不需要知道 n、最大度數或根離心率。障礙在於兩種古典技巧都被排除 — 指向鄰居的指標與到根的距離各自至少需要對數級別的記憶體，而既有的常數記憶體技巧（距離二著色、Thue-Morse 序列）只在有界度數或二度圖上有效。此外已知：在沒有指定領袖的情況下，常數空間的生成樹是不可能的 — 這正是必須假設半均勻且同步的原因。新的想法是無限符記流：具有有限壽命的符記能打破環，而這個技巧可能可以重複使用在其他任務上。常數空間的最優二分圖二著色則隨之而來。

---

## Q10 — Does it work in an asynchronous network?

**Q.** Everything here is synchronous. What happens under an asynchronous or distributed scheduler?

> **問：** 這裡的一切都是同步的。在非同步或分散式排程下會怎樣？

**A.** As stated, it does not carry over, and the paper says so plainly — this is its first open problem. Synchrony is used in three places. First, when a vertex takes a token it takes it from *all* its parents, and "in the same round" is only meaningful if all parents send in the same step, so that duplicates can be merged into one. Second, the flag value wait records which neighbours sent a token in the previous step, and that is exactly what the sibling and child error predicates compare. Third, the lifespan bound and the exponential argument are counted in rounds. To go asynchronous you would need a synchronizer, which itself costs memory — typically logarithmic — so whether the constant-space property survives is genuinely open. If asked to speculate: the reset-and-rejoin mechanism looks more portable than the token stream.

> **答：** 就目前的陳述而言，它不能直接移植，而論文也明確承認了這一點 — 這是它的第一個開放問題。同步性用在三個地方。第一，當頂點取得符記時，它是從*所有*父節點取得，而「在同一個回合」只有在所有父節點都在同一步發送時才有意義，這樣重複的符記才能合併成一個。第二，旗標的 wait 值記錄了哪些鄰居在上一回合發送了符記，而這正是兄弟與子節點的錯誤述詞所比較的內容。第三，壽命的上界與指數級的論證都是以回合來計數的。要走向非同步，就需要一個同步器（synchronizer），而它本身就要花費記憶體 — 通常是對數級別 — 所以常數空間的性質是否還能保持，確實是開放問題。若要猜測：重置—重新加入的機制看起來比符記流更容易移植。

---

## Panic sheet (one line each)

| # | Question | One-line answer |
|---|---|---|
| 1 | Why mod 3? | A neighbour's distance is d−1, d or d+1, and those are distinct mod 3 — 2 bits classify the family. |
| 2 | Why 6 bits? | 2^5 = 32 < 36 = 4 × 3 × 3, and the three fields must stay separately readable. |
| 3 | Why exponential? | The root cannot count levels; tokens act as a binary counter. Extending b's domain helps, but stays exponential. |
| 4 | Why not silent? | The token stream is the algorithm; ranks freeze, but t and b keep cycling forever by design. |
| 5 | Why a root program? | Semi-uniform: the root has no parent, and only it can seed the reset wave. |
| 6 | Why no cycle traps a token? | One bit: after at most n receptions along a cycle the bit forbids forwarding and the token dies. |
| 7 | Where is the tree? | In the parent function: minimum port number among the parents, computed on the fly, never stored. |
| 8 | How is it proved? | Chain of attractors plus a strictly decreasing potential bounded by 2^epsilon. |
| 9 | What is new? | First constant-space deterministic self-stabilizing BFS, via the infinite token stream. |
| 10 | Asynchronous? | Not yet — synchrony is needed for merging, for `wait`, and for the round counts. Open problem. |

---

## Timing guide

| Item | Target | Notes |
|---|---|---|
| Read the question back | 5 s | Buying time and confirming you understood it. |
| Answer | 45–55 s | ~120–140 words. Stop after the reason; do not add caveats. |
| Total per question | ≤ 1 min | If you are over, drop the second half of the answer (the "and by the way" clause). |

**If a question is not covered here.** Say what the paper states, name the section, and if it is outside the paper say *"that is not covered; I would treat it as future work"*. Three safe fallbacks: the bound is O(2^ε) steps and 6 bits; the technique is the infinite token stream; the model is semi-uniform, anonymous, synchronous.
