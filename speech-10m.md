This is a 12-minute talk with 12 slides. The title page and the last page take little time, so the 10 content slides use most of the 12 minutes. Every slide has the same text in English and in Chinese. Choose one language for practice.

> 這是 12 分鐘的報告，共 12 頁。標題頁和最後一頁佔的時間很少，其餘 10 頁內容用掉 12 分鐘的大部分。每頁的英文和中文內容相同，練習時選一種語言即可。

---

## Slide 1 — 標題 (Title)
Good evening. My name is XXX. Today I present the paper Deterministic Synchronous Self-Stabilizing BFS Construction with Constant Space Complexity. In this talk I first explain the algorithm, and then I show a demo of it.

> 午安。我是 XXX。今天我報告的論文是 Deterministic Synchronous Self-Stabilizing BFS Construction with Constant Space Complexity。在這場報告中，我先說明演算法 (algorithm)，然後示範 (demo) 它。

---

## Slide 2 — 模型與問題 (Model and Problem)
The network is connected and undirected. Every node is anonymous. A node sees only its neighbours and its own port numbers. Nobody knows any global parameters. The goal is BFS. Each node shows its distance to the root modulo three. This uses six bits per node. It is deterministic, and it is self-stabilizing from any initial state.

> 網路連通 (connected) 且無向 (undirected)。每個節點匿名 (anonymous)。節點只看得到鄰居 (neighbour) 和自己的埠號 (port number)。沒有人知道任何全域參數 (global parameter)。目標是 BFS。每個節點顯示到根的距離取模三 (distance modulo three)。每個節點用六個位元 (six bits)。它是確定性的 (deterministic)，而且從任何初始狀態 (initial state) 都能自穩定 (self-stabilizing)。

---

## Slide 3 — 為何困難 (Why is this problem hard?)
The problem is a trade-off between space and time. Past solutions, silent or talkative, cannot work in constant space. Constant space is known only if pointers are allowed. Without a leader, the problem is impossible. So we use the semi-uniform model: a network with one root. Here is the difficulty. Nodes are anonymous, so a node cannot name its parent. There is no counter, so no node can detect a cycle. A token would stay in the loop forever. Also, a stored pointer would break the constant bound. Our idea is one token whose lifespan is bounded.

> 這個問題是空間與時間的取捨 (trade-off)。過去解法無論是靜默 (silent) 或多話 (talkative)，都無法做到常數空間 (constant space)。常數空間只在允許指標 (pointer) 時才存在。沒有領導者 (leader)，這個問題不可能 (impossible)。所以我們用半均勻 (semi-uniform) 模型：一個根 (root) 的網路。難點在這裡。節點匿名 (anonymous)，所以無法指名父節點。沒有計數器 (counter)，所以偵測不到環 (cycle)。token 會永遠留在迴圈裡 (stay in the loop forever)。而且儲存指標會破壞常數界 (constant bound)。我們的想法 (idea) 是一個壽命有界 (bounded lifespan) 的 token。

---

## Slide 4 — 演算法總覽 (Algorithm Overview)
There are two programs. tokbinR runs at the root, and tokbinV runs at all the other nodes. The root sends tokens. When several tokens arrive at the same node, they merge. Each node keeps one bit and flips it on every arrival. A node sends a token only if the next neighbour's bit allows it. This condition bounds the lifespan of every token, so no cycle can keep a token alive forever. Here is an example. The root sends a token down to each child. After one round, both children have a token. A few rounds later the tokens die.

> 有兩支程式 (program)。tokbinR 在根執行，tokbinV 在其他所有節點執行。根送出 token。多個 token 到同一個節點時會合併 (merge)。每個節點存一個位元 (bit)，每收到一次就翻轉 (flip)。只有下一個鄰居的位元允許時，節點才送出 (send) token。這個條件限制每個 token 的壽命 (lifespan)，所以環 (cycle) 無法讓 token 一直活著。這裡是一個例子。根把 token 往下送給每個子節點。一回合之後，兩個子節點各有一個 token。再過幾回合，token 就消失 (die)。

---

## Slide 5 — 視覺化 (Visualization)
Here is a visualization of the algorithm. I built a JavaScript program for it, and the link under the screenshot opens it. It has three prepared cases, and I run them in this order.
Case one, all reset. Every node starts at bottom, and the root's tokens sweep down until the ranks are the distance modulo three.
Case two, injection. The tree is already correct, and I flip one bit of one node by hand. The error climbs to the root, every node resets, and the tokens rebuild the same tree.
Case three, a rank ring. I hand-set ranks that close a loop, so the parent links inside the loop never reach the root. No local check fires, so only the tokens expose it.
So the three cases show convergence from any state, recovery from a fault, and a cycle that local rules alone cannot catch.

> 這裡是演算法 (algorithm) 的視覺化 (visualization)。我做了一個 JavaScript 程式 (program)，截圖 (screenshot) 下方的連結會開啟它。程式裡有三個預先準備的案例 (case)，我依序示範。
> 案例一，全部重置 (all reset)。每個節點都從 bottom 開始，根的 token 往下擴散 (spread)，直到 rank 等於距離取模三 (distance modulo three)。
> 案例二，注入錯誤 (injection)。樹本來就是正確的，我手動翻轉 (flip) 某個節點的一個位元 (bit)。錯誤一路往上爬到根，所有節點重置 (reset)，token 重建 (rebuild) 同一棵樹。
> 案例三，rank 環 (rank ring)。我手動設定 rank，讓它們圍成一個環 (loop)，環內的父連結 (parent link) 永遠到不了根。沒有任何局部檢查 (local check) 會觸發，所以只有 token 能揭發 (expose) 它。
> 所以這三個案例分別展示了：從任意狀態收斂 (convergence)、從錯誤中復原 (recovery)，以及單靠局部規則抓不到的環 (cycle)。

---

## Slide 6 — 本地變數 (Local variables)
Each node stores three variables. First: k, the rank. It can be 0, 1, 2, or the bottom symbol for reset. The rank is the distance modulo three. Second: t. It can be FALSE, TRUE, or WAIT. Third: b. It can be 0, 1, or the top symbol for errors. So there are thirty-six states in total, using six bits, and this number does not grow with the network. Here is an example graph where the nodes have different local variables. The label above each node is its (k, t, b) triple.

> 每個節點存三個變數 (variable)。第一：k，也就是 rank，可以是 0、1、2，或 bottom 符號代表重置 (reset)。rank 就是距離取模三。第二：t，可以是 FALSE、TRUE 或 WAIT（接受、持有、等待）。第三：b，可以是 0、1，或 top 符號代表錯誤 (error)。所以有 36 個狀態 (state)、6 個位元，而這個數量不隨網路變大而成長 (grow)。這裡是一個範例圖 (example graph)，圖中節點有不同的本地變數 (local variable)。每個節點上方的標籤 (label) 就是它的 (k, t, b) 三元組 (triple)。

---

## Slide 7 — 親屬結構 (Kinship structure)
A node reads its family from the rank, modulo three. Rank minus one is a parent. Rank plus one is a child. The same rank is a sibling. Nothing is stored, so this costs no memory. A reset node has no family. It just ignores every token. Nodes are anonymous, so the parent is the neighbour of rank k minus one that has the smallest port number.

> 節點從 rank 取模三讀出家庭 (family)。rank 減一是父 (parent)。rank 加一是子 (child)。相同 rank 是兄弟 (sibling)。不儲存任何東西，所以不佔記憶體 (memory)。重置 (reset) 的節點沒有家庭，它忽略所有 token。節點匿名 (anonymous)，所以父節點是 rank k 減一的鄰居中，埠號 (port number) 最小的那一個。

---

## Slide 8 — 根演算法 (Root algorithm TokBinR)
There are two states in the state machine graph. In the quiet state, every neighbour has rank one, with the same t and b. In the error state, the root raises the error bit when the neighbourhood is not synchronized. When every neighbour carries the error bit, the root goes back to the quiet state and restarts.

> 狀態機圖 (state machine graph) 裡有兩個狀態 (state)。在安靜 (quiet) 狀態，每個鄰居都是 rank 一，t 和 b 也相同。在錯誤 (error) 狀態，當鄰域不同步 (synchronized) 時，根舉起 (raise) 錯誤位元。當每個鄰居都帶著錯誤位元時，根回到安靜狀態並重新開始 (restart)。

---

## Slide 9 — 非根演算法 (Non-root algorithm TokBinV)
The seven rules are on the left. On the right is the state machine graph. The bottom part is the normal cycle. In the quiet state the node waits until a parent's token arrives, then the node passes it on and flips the bit. Next, it is released and goes back to the quiet state. Each turn of the cycle pushes a token one level down. The top part is the error path. All the red arrows lead to the error state. From there the node goes to the reset state. Finally, it comes back to the quiet state.

> 七條規則 (rule) 在左邊。右邊是狀態機圖 (state machine graph)。下面一行是正常循環 (cycle)。在安靜 (quiet) 狀態，節點等待。某個父節點的 token 到達，然後節點把它往下送並翻轉 (flip) 位元，接著被釋放，回到安靜狀態。每轉一圈，token 就往下走一層。上面是錯誤路徑 (error path)。所有紅色箭頭都指向錯誤狀態。節點從那裡進入重置 (reset) 狀態，最後回到安靜狀態。

---

## Slide 10 — 錯誤偵測 (Error detection)
There are three kinds of error. First: internal error, the rank and the parent do not agree. Second: local error, parents, children, or siblings do not agree. Third: global error, a neighbour already has the error bit. Every check is local, because a node sees only its neighbourhood. The error bit spreads to all the nodes. Then every node resets, and the tokens rebuild the entire tree. This works from any initial state.

> 錯誤 (error) 有三種。第一：內部錯誤 (internal error)，rank 和父節點不一致 (do not agree)。第二：局部錯誤 (local error)，父、子或兄弟不一致。第三：全域錯誤 (global error)，鄰居已經有錯誤位元。每個檢查都是局部的 (local)，因為節點只看得到自己的鄰域 (neighbourhood)。錯誤位元擴散 (spread) 到所有節點。然後每個節點重置 (reset)，token 重建 (rebuild) 整棵樹。這從任何初始狀態 (initial state) 都成立。

---

## Slide 11 — 結論 (Conclusion)
To conclude. This is the first constant-space, deterministic, self-stabilizing BFS construction for semi-uniform synchronous networks. Six bits per node, without global parameters. The key idea is a token stream with a bounded lifespan. It breaks cycles and rebuilds the levels. We see two directions for future work. First, asynchronous schedulers. In our model, all nodes move in the same round. If they move at different times, the argument fails. Second, the time bound. We stabilize in two to the power epsilon rounds. We would like a polynomial time bound, so the tree can be built faster.

> 總結 (conclude)。這是第一個適用於半均勻 (semi-uniform) 同步網路的常數空間、確定性 (deterministic)、自穩定 (self-stabilizing) BFS 建構 (construction)。每個節點六個位元 (six bits)，不需要全域參數 (global parameter)。關鍵想法 (key idea) 是壽命有界 (bounded lifespan) 的 token 流 (token stream)。它打破環 (cycle) 並重建層級 (level)。未來工作 (future work) 有兩個方向。第一，非同步排程器 (asynchronous scheduler)。在我們的模型裡，所有節點在同一個回合 (round) 移動；如果移動的時間不同，就行不通 (the argument fails)。第二，時間界 (time bound)。我們在 2 的 ε 次方個回合內穩定 (stabilize)。我們希望有多項式界 (polynomial bound)，這樣樹就能建得更快。

---

## Slide 12 — 謝謝 (Thank you)
That is the end of my presentation. Thank you.

> 我的報告到此結束 (that is the end of my presentation)。謝謝 (thank you)。
