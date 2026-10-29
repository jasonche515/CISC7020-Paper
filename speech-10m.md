This is a 12-minute talk with 12 slides. The title page and the last page take little time, so the 10 content slides use most of the 12 minutes. Every slide has the same text in English and in Chinese. Choose one language for practice.

> 這是 12 分鐘的報告，共 12 頁。標題頁和最後一頁佔的時間很少，其餘 10 頁內容用掉 12 分鐘的大部分。每頁的英文和中文內容相同，練習時選一種語言即可。

---

## Slide 1 — 標題 (Title)
Good evening. My name is XXX. Today I present the paper Deterministic Synchronous Self-Stabilizing BFS Construction with Constant Space Complexity.

> 午安。我是 XXX。今天我報告的論文是 Deterministic Synchronous Self-Stabilizing BFS Construction with Constant Space Complexity。

---

## Slide 2 — 模型與問題 (Model and Problem)
The network is connected and undirected. Every node is anonymous. It is semi-uniform: with a single root. Nobody knows any global parameters. The goal is BFS. Each node shows its distance to the root modulo three. This uses six bits per node. It is deterministic, and it is self-stabilizing from any initial state.

> 網路連通 (connected) 且無向 (undirected)。每個節點匿名 (anonymous)。它是半均勻 (semi-uniform) 的：只有一個根 (root)。沒有人知道任何全域參數 (global parameter)。目標是 BFS。每個節點顯示到根的距離取模三 (distance modulo three)。每個節點用六個位元。它是確定性的 (deterministic)，而且從任何初始狀態 (initial state) 都能自穩定 (self-stabilizing)。

---

## Slide 3 — 為何困難 (Why is this problem hard?)
The problem is a trade-off between space and time. In the past solutions, either silent or talkative algorithm, cannot work in constant space. Only if pointers are allowed. Without a leader, the problem is impossible. So we use the semi-uniform model. Here is the difficulty. Nodes are anonymous, so you cannot name the parent. There is no counter, so you cannot detect a cycle. A token would stay in the loop forever. Also, a stored pointer would break the constant memory bound. To solve all of these, our idea here is one token whose lifespan is bounded.

> 這個問題是空間與時間的取捨 (trade-off)。過去的解法，無論是靜默 (silent) 或多話 (talkative) 的演算法，都無法做到常數空間 (constant space)。只有在允許指標 (pointer) 時才行。沒有領導者 (leader)，這個問題不可能。所以我們用半均勻 (semi-uniform) 模型。難點在這裡。節點匿名 (anonymous)，所以無法指名父節點。沒有計數器 (counter)，所以偵測不到環。token 會永遠留在迴圈裡。而且儲存指標會破壞常數記憶體界 (constant memory bound)。為了解決這一切，我們的想法是一個壽命有界 (bounded lifespan) 的 token。

---

## Slide 4 — 演算法總覽 (Algorithm Overview)
There are two programs. tokbinR runs at the root, and tokbinV runs at all the other nodes. The root sends tokens. When several tokens arrive at the same node, they merge into one. Each node keeps one bit and flips it on every arrival. A node sends a token only if the next neighbour's bit allows it. This condition bounds the lifespan of every token, so no cycle can keep a token alive forever. It stabilizes in big-O of two to the power epsilon rounds, and it is never silent.

> 有兩支程式。tokbinR 在根執行，tokbinV 在其他所有節點執行。根送出 token。多個 token 到同一個節點時會合併成一個。每個節點存一個位元，每收到一次就翻轉。只有下一個鄰居的位元允許時，節點才送出 token。這個條件限制每個 token 的壽命 (lifespan)，所以環無法讓 token 一直活著。它在 $O(2^{\varepsilon})$ 個回合內穩定，而且從不靜默 (never silent)。

---

## Slide 5 — 視覺化 (Visualization)
Here is a visualization of the algorithm. I built a JavaScript program.  
First, I can set the number of nodes. I can pick a topology: chain, ring, grid, tree, random, or triangular. Then I choose the initial configuration: all reset, BFS, random ranks, or with errors. When I press Play, each step is a synchronous round. The panel shows the local variables, and the log message here shows which rule is used. Tokens spread out from the root. Each node updates its rank and so on.
Case 1: Every node starts at reset, and the root's tokens come down until the network stabilizes. Case 2: I flip one bit of one node by hand. The error climbs to the root, so every node resets, and then the tokens rebuild the same tree. Case 3: I created a cycle here, so the parent links inside this cycle never reach the root. No local check can detect it. Only the tokens expose the cycle. So these cases show the process, how to recover from a fault, and how to kill a cycle.

> 這是演算法的視覺化 (visualization)。我用 JavaScript 寫了一支程式。首先，我可以設定節點數量 (number of nodes)。我可以挑選拓樸 (topology)：鏈 (chain)、環 (ring)、格子 (grid)、樹 (tree)、隨機 (random) 或三角 (triangular)。接著我選擇初始設定 (initial configuration)：全部重置 (all reset)、BFS、隨機 rank，或帶有錯誤。當我按下 Play，每一步就是一個同步回合 (synchronous round)。面板顯示本地變數 (local variables)，而這裡的日誌訊息 (log message) 會顯示用了哪一條規則。token 從根往外擴散 (spread out)。每個節點更新自己的 rank，諸如此類。案例一 (Case 1)：每個節點從重置狀態開始，根的 token 一層一層往下，直到整個網路穩定。案例二 (Case 2)：我手動翻轉某個節點的一個位元。錯誤往上爬到根，於是每個節點重置，接著 token 重建同一棵樹。案例三 (Case 3)：我在這裡造了一個環，所以這個環裡面的父指標 (parent link) 永遠到不了根。沒有任何局部檢查 (local check) 能偵測到它。只有 token 才會揭露 (expose) 這個環。所以這些案例展示了整個過程、如何從錯誤中復原 (fault)，以及如何消滅一個環。

---

## Slide 6 — 本地變數 (Local variables)
Each node stores three variables. First: k, the rank. It can be 0, 1, 2, or the bottom symbol for reset. The rank is the distance modulo three. Second: t. It can be FALSE, TRUE, or WAIT. Third: b. It can be 0, 1, or the top symbol for errors. So there are thirty-six states in total, using six bits, and this number does not grow with the network. Here is an example graph. The label above each node is the variables (k, t, b).

> 每個節點存三個變數。第一：k，也就是 rank，可以是 0、1、2，或 bottom 符號代表重置 (reset)。rank 就是距離取模三。第二：t，可以是 FALSE、TRUE 或 WAIT（接受、持有、等待）。第三：b，可以是 0、1，或 top 符號代表錯誤。所以有 36 個狀態、6 個位元，而這個數量不隨網路變大而成長。這裡是一個範例圖 (example graph)。每個節點上方的標籤就是變數 (k, t, b)。

---

## Slide 7 — 親屬結構 (Kinship structure)
First, family relationship is read from the rank. Rank minus one is a parent. Rank plus one is a child. The same rank is a sibling. Second, a reset node has no family. It just ignores every token. Third, nodes are anonymous, so the parent is the neighbour of rank k minus one that has the smallest port number.

> 第一，家庭關係 (family relationship) 是從 rank 讀出來的。rank 減一是父。rank 加一是子。相同 rank 是兄弟。第二，重置 (reset) 的節點沒有家庭，它忽略所有 token。第三，節點匿名 (anonymous)，所以父節點是 rank k 減一的鄰居中，埠號 (port number) 最小的那一個。

---

## Slide 8 — 根演算法 (Root algorithm TokBinR)
There are two states in the state machine graph. In the quiet state, every neighbour has rank one, with the same t and b. In the error state, the neighbourhood is not synchronized. When every neighbour carries error, the root goes back to the quiet state and restarts.

> 狀態機圖 (state machine graph) 裡有兩個狀態。在安靜 (quiet) 狀態，每個鄰居都是 rank 一，t 和 b 也相同。在錯誤狀態，鄰域不同步 (not synchronized)。當每個鄰居都帶著錯誤時，根回到安靜狀態並重新開始。

---

## Slide 9 — 非根演算法 (Non-root algorithm TokBinV)
The seven rules are on the left. On the right is the state machine graph. The bottom part is the normal cycle. In the quiet state the node waits until a parent's token arrives, then the node passes it on and flips the bit. Next, it is released and goes back to the quiet state. Each turn of the cycle pushes a token one level down. The top part is the error path. All the red arrows lead to the error state. From there the node goes to the reset state. Finally, it comes back to the quiet state.

> 七條規則在左邊。右邊是狀態機圖 (state machine graph)。下面一行是正常循環。在安靜 (quiet) 狀態，節點等待。某個父節點的 token 到達，然後節點把它往下送並翻轉位元，接著被釋放，回到安靜狀態。每轉一圈，token 就往下走一層。上面是錯誤路徑 (error path)。所有紅色箭頭都指向錯誤狀態。節點從那裡進入重置 (reset) 狀態，最後回到安靜狀態。

---

## Slide 10 — 錯誤偵測 (Error detection)
There are three kinds of error: internal error, local error, and global error. Every check is local, because a node sees only its neighbourhood. The error bit spreads to all the nodes. Then every node resets, and the tokens rebuild the entire tree. Here is the self-stabilizing loop. Error detected, then raise error bit, reset, rejoin and rebuild.

> 錯誤有三種：內部錯誤 (internal error)、局部錯誤 (local error)、全域錯誤 (global error)。每個檢查都是局部的 (local)，因為節點只看得到自己的鄰域 (neighbourhood)。錯誤位元擴散 (spread) 到所有節點。然後每個節點重置 (reset)，token 重建整棵樹。這就是自穩定 (self-stabilizing) 的循環：偵測到錯誤，然後舉起錯誤位元，重置，重新加入 (rejoin) 並重建。

---

## Slide 11 — 結論 (Conclusion)
To conclude. This is the first constant-space, self-stabilizing BFS algorithm. Six bits per node, without global parameters. The key idea is a token stream with a bounded lifespan. It breaks cycles and rebuilds the levels. We see two directions for future work. First, asynchronous schedulers. In our model, all nodes move in the same round. If they move at different times, the argument fails. Second, the time bound. We stabilize in two to the power epsilon rounds, which is very slow. We would like a polynomial time bound, so the tree can be built faster.

> 總結 (conclude)。這是第一個常數空間、自穩定 (self-stabilizing) 的 BFS 演算法。每個節點六個位元，不需要全域參數 (global parameter)。關鍵想法是壽命有界 (bounded lifespan) 的 token 流 (token stream)。它打破環並重建層級。未來工作 (future work) 有兩個方向。第一，非同步排程器 (asynchronous scheduler)。在我們的模型裡，所有節點在同一個回合移動；如果移動的時間不同，就行不通。第二，時間界 (time bound)。我們在 $O(2^{\varepsilon})$ 個回合內穩定，這非常慢。我們希望有多項式界 (polynomial bound)，這樣樹就能建得更快。

---

## Slide 12 — 謝謝 (Thank you)
That is the end of my presentation. Thank you.

> 我的報告到此結束。謝謝。
