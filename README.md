# 2301

# 278E Grid Filling   diff 996

解答遷移 AC

計 58:54

備考

➀　思考

(H-h+1)(W-w+1)個のマスすべてに対して、隣接hw マスを全操作すればこの問題に解答できるが、計算量は O( 10^8 ) になるので不可能。⇒ 次のマスの隣接マスと重複している領域は新しく探索する必要がないことに注目。 これによって O( h(W-w) ) で同じ行について調べられる。行を更新する際は、前の行の初めの状態を覚えておいてそれを更新する。これによって、全体での操作回数は Nhw : 初めの隣接hw マスの調査 + h(W-w)(H-h+1) : 列方向での調査 + w(H-h) 行の更新 となって、計算量は O(10^6) に改善できた。

➁ 別解

類題 129D Lump


# 0120

# 285C abc285_brutmhyhiizp    diff　157

解答遷移 AC

計 13:38

備考

➀　思考

長さ L の文字列の場合、L-1 以下の文字列は Σ 26^k 個存在する。また、L文字の文字列の順番を求めるためには先頭の文字から順に、何番目の文字列か に注目する。例えば、先頭が C の場合、AAA...A ～ CAA...A までに、2* 26^(L-1) 個存在するので、これを最後の文字に至るまで繰り返すことで順番を特定した。


➁ 別解

26進数で考える。すると、例えば ABC の場合、3桁の文字列の中で 012(26) 番目と考えることができる。

その桁以下の文字列を考量する必要があるが、これは各桁の値を +1 することで実現が可能。1桁目はこの限りではないが、index揃えで +1 するので、すべての桁でこの処理を施せばよい。ABC は 123(26) 番目となる

後はこの26進数の番号を10進数に変換すればよい。


関連問題

171C One Quadrillion and One Dalmatians (逆処理)

216C Many balls (逆処理 2進数 ver)



# 266B Modulo Number  diff  95

解答遷移 AC

計 05:49

備考

➀　思考

x= N (mod) に気が付いてAC

# JSC2021  Xor of Sequences  diff 51

解答遷移 AC

計 05:03

備考

➀　思考

Atcoder では計算量の観点から集合演算が最適解になることはないので、これは避けて解答したかった。⇒ ありえる数を全探索して、条件を満たすか判定すればよいと判断 ⇒ AC


# 0121

# 216C Many Balls   二回目  diff 145

解答遷移 AC

備考

➀　思考

N から 0 を目標に操作を行う。2で割っていくのもよいが、Nを2進数で表記すれば、各桁が 1 なら操作 A が必要と判断するシミレーションを行えると考えた。

なお、逆順では deque が有効と考えたが、最終的な文字列の結合を厳密に行う場合( 一番最初の 操作B を取り除く場合)、deque はリストと異なりスライスできないので注意。

➁ 別解

上から見ていけば逆順処理は必要なくなる


# 171C One Quadrillion and One Dalmatians    2回目  diff 560

解答遷移 AC

備考

➀　思考

桁数を特定し、その桁の中で何番目なのかわかれば名前を決定できると判断。⇒ まず桁数を特定し、その過程でその桁以下の名前の総数を引いていく。後はその求めるべき番号を 26進数で表記すれば、各桁のアルファベットを上から求められると判断した。


➁ 別解

その桁の中で何番目か求めることは、各桁を -1 することに一致する。また、26で割った余りに注目することで下の桁からアルファベットを特定できる。これによって桁数を求めることなく、文字列を特定することができる。


類題 

216C Many Balls ( 2進数 ver )   

関連問題

285C abc285_brutmhyhiizp (逆処理)


# 最短経路問題　～ BFS  vs  ダイクストラ  vs 01-BFS　～

## 実装編

BFS は全辺の重さが等しいグラフにおける単一始点の最短経路を求めることができる。暫定最短距離が更新されることがないので、que に同じ頂点が格納されることはないので、queから取り出した頂点が最短経路をなすか考量する必要がないぶん実装が簡単

ダイクストラ法は、全辺が非負の重さをもつグラフにおける単一始点の最短経路を求めることができる。暫定最短距離が更新されることがあるので、[最短経路のコスト、次の頂点] をヒープに入れていくと、ヒープから取り出した頂点が最短経路をなさない場合がある。よって、これをコストと現在の暫定最短距離で判定する処理が必要。

01-BFS は全辺の重さが 0 or 1 のグラフにおける単一始点の最短経路を求めることができる。 次の辺が 0 なら 頂点を管理する deque の頭に、1 なら後ろに 注目する頂点を格納していくことで、最短経路を探索するための正しい順番を形成できる。ダイクストラ法同様、暫定最短距離は更新されるが、コストを格納しない場合、頂点の情報のみではその頂点が探索済か判断することができない。⇒ 頂点の重複探索を許してしまい deque に格納する部分で判定するか、コストの情報も管理することにするか、あるいは最短距離リストとは別で探索済みリストを作成することで判断するか することになると考える (230122現在)

まとめると、

まず、次に探索する頂点を格納したデータ構造から頂点を取り出し、BFSでなければコストなどで暫定最短距離と比較する。この条件をクリアできたら、次の頂点を探索し、その頂点の暫定最短距離を更新することができるならデータ構造に格納する。この一連の流れを、各アルゴリズムで適切なデータ構造を用いて行えばよい。



https://betrue12.hateblo.jp/entry/2018/12/08/000020


## 意識編   0126 213E解答後

BFS は辺の重みが全て等しいので、探索アルゴリズムとして考えてもよいが、01-BFS および ダイクストラ法 は適切な重みの辺を順番に頂点間に貼り、グラフを構築するイメージを持つ必要がある。

# 0125

# ☆ 246E　Bishop 2　　diff  1476

解答遷移 TLE WA AC

計 38:51 + 10:00

備考

➀ 思考

いけるところまでいく BFS で解答できると考えたが計算量が O(N^3) になり TLE ⇒ 行先の暫定最短距離 > 現在地の最短距離 +1 でなければ打ち止めることで計算量を改善できると考えたが WA ⇒ 下図のような例で 行先の暫定最短距離 = 現在地の最短距離 +1 の場合に打ち止めてしまうのではなく、その先を見る必要があることに気づいて、行先の暫定最短距離 < 現在地の最短距離 +1 の場合に打ち止めることにして AC

なお、解答時は計算量推定する余裕はなく、6sec だしいけるやろで提出している。

改善した実装では、対角線の探索が一度切りで、各頂点から 4方向の頂点を探索すると考えると計算量は全体で O(2(2N+1) + 4N^2) であると推定できる。
 
![image](https://user-images.githubusercontent.com/109026838/214480828-b9702512-1db3-46fe-ab48-30d93af8efab.png)

➁ 解法

現在地に到達した方向と同じ方向の頂点と連結する辺の重みを 0 , それ以外の辺の重みを 1 としてグラフを構築して 01BFS をすることでこの問題を解答できる。

ただし、現在地に到達した方向次第で同じ行き先であっても辺の重みが変化してしまう。そこで頂点を(x座標,y座標,直前の方向) で管理することにする。この工夫でグラフはとてもシンプルになる

![image](https://user-images.githubusercontent.com/109026838/214483970-d8533b1f-edb2-4392-acb2-433ce6e18c07.png)

もしこの工夫を施さないとすると、重み1 の辺で連結している行き先の頂点の更新条件を 行き先の暫定最短距離 ≧ 現在地最短距離 +1 とすることで全方向を考慮する対策が考えられる。しかし、この方法では結局、方向ごとに暫定最短距離を持つことができず、行き先の暫定最短距離の予期しない更新が生じてしまう。

![image](https://user-images.githubusercontent.com/109026838/214486873-8740c712-b772-4c29-ae14-bd01ccc207dc.png)

また、直前の方向の情報を含めて頂点を管理する場合、頂点数は 4×2×N^2 (4方向 × 暫定最短距離 x , x+1 の2種) なので計算量は O(2V) = O(8E) = O(1.44×10^8) となるが、更新条件を緩くした方針では、頂点数は 16×4×N^2 になるので、全体で O(1.1×10^9) となって全く間に合わない。

③ 感想

グラフの構築の観点から、状態の情報で頂点を管理する意義を理解することができた。また、更新条件を緩くすると想定以上に計算量が増加してしまうことを理解出来た。


# 0126

# ☆ 213E  Stronger Takahashi   diff 1423

備考

➀　思考

道ならコスト0 、壁ならコスト1で移動するので、01-BFSで解答できると考えた。しかし、今回の壁を破壊して移動する処理に癖があり一筋縄ではいかない ⇒ あるマスを破壊する場合、そのマスを左上、左下、右上、右下に設定する4通りが考えられるので、頂点数を4倍する処理を考えた。しかしこれではずっと同じ破壊を繰り返す(異なる種類の破壊が共存しない)ことになって適切ではないと判断した。⇒ そのマスを中心とした 3×3 のマスへは距離 1 でいけることに気が付いた。

ただし、隣接4マス以外のこれら 3×3　マスをどのタイミングで que に格納すべきか非常に難しかった。つまり、現在いるマスが壁の場合これらを探索することにすべきか、隣接4マスが壁の場合に追加でこれらのマスも探索すべきかである。前者の場合、壁のとき 3×3 マスを現在地と同じ最短距離に更新することになるがこれでは連続で壁を移動する場合コストが生じなくなってしまう(サンプル3で距離1になる)。そこで壁からの移動後に 3×3のマスを全て道に変更することも考えたが、この場合もこの場合で本来壁であったマスが道になることで適切に移動できなくなってしまった。後者の場合も、隣接マスが同じ最短距離の場合に、3×3マスの更新がうまくできなかった。

ここで降参


➁ 解法

まず、方針は◎

壁移動の範囲をすばやく判断すること、適切にグラフを構築することができなかったことが敗因。

辺の重みを意識しない BFS とは異なり、01-BFS やダイクストラ法ではグラフを構築する意識を持つ必要がある。つまり、マスから移動可能なマスに向かって辺を貼っていく意識である。

隣接4マスが壁の場合、確かに隣接マスを中心とした 3×3 の範囲のマスが移動可能だが、これは隣接マスからではなく始点からコスト1で行けるマスである。したがって、始点から辺を貼る必要がある。

![image](https://user-images.githubusercontent.com/109026838/214760527-53c67c9d-5334-4acd-973c-4f9fe87cabd8.png)


# 184E Third Avenue  diff 1418

解答遷移 WA →　AC

計 46:33 + 05:00

備考

➀　思考

隣接4マス および ワープマスを探索していく BFS で解答できると判断。各アルファベットごとにワープを一度切りとすれば計算量 O(4HW + HW)　となって十分高速と判断　⇒ WA  ⇒ ワープ時の暫定最短距離の更新条件が誤っていることに気づいてAC

# 277E  Crystal Switches  diff 1183

解答遷移 AC

計 26:49

備考

➀　思考

一度解いたことがある + 246E Bishop 2 にて状態で頂点を管理することを勉強していたので、自然に現在のスイッチの情報で頂点を分類することが分かった。後は、スイッチを持つ頂点でスイッチのの情報が異なる同じ番号の頂点に、重み0 の辺を張る 01-BFSを行えばよいと判断。⇒ AC


# 284E Count Simple Paths  diff 1043

解答遷移 TLE TLE  AC

計 44:13 + 10:00

備考

➀　思考

BFS で数え上げられるかと思ったが全く無理　⇒ 帰りがけで記憶をなくす DFS でパスを数え上げられると気づく。また、入次数 10 より各頂点が最高でも 10回ずつしか探索されないため　計算量が　O(10N) となって十分高速と判断　⇒ TLE ⇒ パスの上限で DFS をうち止める　⇒ TLE ⇒ もう他に工夫は考えられないと感じていた時に、pypyは再帰に弱いことを思い出し、pythonで提出 ⇒ AC

➁ 計算量推定

頂点が探索される回数は 10回に抑えられるとは限らない。同じ頂点からの別のパスが容易に考えられる。そのため、パスの上限で抑える必要があった。また辺が密集している場合、次に探索する頂点を取り出すために次数分計算回数が必要になるので、次数に制限がないと パスの上限で抑えていても計算回数はこれを上回ってしまう可能性がある。

つまり、全体の計算量は O(10N + min(K,10^6)) となって、これでようやく十分高速なわけである。

# ☆ 253E Distance Sequence  diff 1073

解答遷移 WA WA 降参

備考

➀　思考

前の値から決定していく dp で解けそうだ。直前の値 + K , 直前の値 -K の値を操作する imos法で更新していけば 計算量 O((M+M)N) なので行けると判断。⇒ 後は、累積和をとる方向が +K,-K で異なるので、dpテーブルを 3NM として 0次元方向でそれぞれ、+K累積和, -K累積和, 累積和合算 の操作を行えばよいと考えた。⇒ WA ⇒ 余りが0になることが原因ではないかと考えたが、違った。打つ手なし。ここで降参

➁ 解法

K=0　の場合、直前の値と同じ位置の値を重複して操作することになってしまうので WA してしまった。この重複部をうまく解消すれば AC 可能

③ 補足

i+1 行目へ i行目の値をそのまま使う更新を行うので、i行目の累積和を求めておけば高速化できる。。。らしいが直感的でないので、imos 法の方が分かりやすい。

# 0127

# 286C Rotate and Palindrome diff 565

解答遷移　AC

計 39:36

備考

➀　思考

前から順に、1,なにもしない  2,a～z で置き換え  3,移動 の遷移を考える方針を考えたが、これだと状態数が 30^5000 程度まで爆発するので無理そう　⇒  いろいろ模索し、お金の方からアプローチする、つまり A,B を全探索して解答できないか考えた　⇒  移動回数 A を全探索して、この時回文にするために何回置き換えが発生するか調べて、お金を更新していけばよいと判断。計算量は O(N^2) で十分高速 ( Aの全探索　×  回文調べ )

# 264E Blackout 2  diff 1229

解答遷移 WA WA TLE

計 57:30 + 10:00

備考

➀　思考

281E で学んだように、グラフは破壊よりも構築を目指すべきなので電線を逆順に張っていく方針を考えた。このとき辺をなす頂点についてそれが都市であり、かつ現在電気が通っていなければ、この辺を張ることでこの都市に電気が通るか考えていけばよさそう。⇒ この判定はこの都市が、発電所または電気が通っている都市に連結されるかどうかで可能。さらにこの都市に連結していて、かつまだ電気が通っていない都市があれば、その都市にも電気が通ることになるので、隣接頂点を探索する必要があり BFS を実装。計算量は、クエリの中で各都市ごとに一度、隣接頂点を探索する必要があるので、O(Q + min(N(N+M),E)) となる。これは十分高速

2度の WA は 隣接都市の探索におけるミス、初めのグラフでの数え上げにおけるミス で食らってしまった。

➁ 別解

発電所を親にした Union-find で発電所と同じ連結成分となる都市を管理する。また、発電所はどれであっても同じなので、超発電所頂点を作成する。

③ メモ

グラフを逆順に構築する方針は一瞬でたったが、実装をどうするか考え作成すること、修正で死ぬほど時間がかかった。


# 0128 

# ☆ 247E max Min   diff 1256

降参

備考

➀ 解法

区間を考える時は、片側を固定し片側を動かす意識を持つ必要がある。例えば、この問題では条件を満たす区間について右側を固定すると、右側の移動によって区間に含まれる要素数は単調に増加する。したがって区間の左側は右側へ進むことだけを考えればよいとわかるので尺取り法で解答できるようになる。 ⇒ 左側の位置は、Xである最も右側の位置、Yである最も右側の位置 の内大きい方であり、これは 右側の移動で含まれるようになった新たな値で更新していけばよい。

ここまでで条件を満たす区間に対する考察は完了。あとは区間に Y未満 Xより大きな値が含まれる場合を処理できれば良いが、これはそのような値が含まれない区間だけを抽出すればよい。

# 229D LongestX  diff 745   n回目

解答遷移 AC

備考

➀　思考

連続区間なので右側固定で考える。⇒ 区間に含まれる . の数は単調に増加するので尺取り法的に考え、右側の移動時に . を含む かつ 残り置き換え回数が0以上 でなければ左側を動かして数え上げることにした。なお、. の位置を保存して、左側の移動を簡略化( 詳細 247E ) したかったが、K個覚えておく必要があって面倒なので、素直に while ループで動かした。

# 246D variable Function  diff 1148

解答遷移 AC

計 31:50

備考

➀　思考

a,b 区間を aを固定して考えると b は単調に減少する。また、a,b はそれぞれ 10^6 以下であるので 0<=a<=10^6 の範囲で尺取り法を行えばよいと判断。なお、bは0 まで動かし、a>b と考えても一般性を失わないのでここでうち止めることにした。

# 124E Handstand  diff 1138

解答遷移 AC

計 32:56

備考

➀　思考

連続区間問題なので右端を固定して考える。また、連続する 0,1 の個数を記憶してランレングス圧縮する方が実装が簡単になりそうだと考えた。あとは単純に、右端に0 が追加された時そこで 残数が K を超えれば、尺取り法の要領で 左端を右側に移動すればよいと判断。








# 270E Apple Baskets on Circle  diff 1211

解答遷移 WA AC 

計 51:05 + 05:00

備考

➀ 思考

次になくなる可能性のある位置、つまり 0以上である最小値の位置について、この位置のりんごをいくつ食べられるか考える。これは min(この位置のリンゴの数 , 残数//(非ゼロ位置数)) で求められる。これを順に繰り返せば最終周に至るまでに各位置のりんごがいくつになったかを求めることができる。⇒ あとは最終周で食べられるだけ前から一つずつ食べていく。

➁ 別解

➀ でも考えたが、何周できるかがポイントになる。そこで何周できるかを二分探索する。評価条件は、m周した場合に K個未満しか食べられない場合を ok とすることで、最終手前周 = ok とできる。あとは、残った分だけ前から食べればよい。


# 160E Red and Green Apples   diff 1036

解答遷移 AC

計 12:48

備考

➀　思考

すべてのりんごをおいしいものから順に、その色の制限を超えていなければ食べることを繰り返す貪欲法で解けると判断　⇒ 無色はどちらの色にも制限なくなれるので総数を超えない範囲で自由に食べ続けても、各色の縛りを満たす。

計算量はソートがボトルネックとなり、 O(NlogN)で十分高速



# 0131

# 178E Dist Max   diff 1054

解答遷移 WA AC

計 1:14:59 + 05:00

備考

➀　思考

ある頂点を始点に、x座標 Xに存在する頂点を終点にする場合、最もy座標が小さいもの　または 最もy座標が大きいもの 2点だけを考えればよい。これ以外の頂点を終点にしたマンハッタン距離は必ずこれらを下回るからである。 ⇒ またこれらを (X,Ymax_X) , (X,Ymin_X) と表現し、始点のX 座標 Xs が Xs<Xi を 満たすとき、(Xs,Ymax_Xs),(Xi,Ymin_Xi) と (Xs,Ymin_Xs),(Xi,Ymax_Xi) の組がマンハッタン距離の最大値を更新する可能性がある。( 最大×最小 の組になるということ ) さらに、Xs<Xi<Xj , Xj-Xi < Ymax_Xi - Ymax_Xj を満たすならば、(Xs,Ymin_Xs) と (Xj,Ymax_Xj)でマンハッタン距離を更新することはない。 ( (Xi,Ymax_Xi)を用いた方が必ずマンハッタン距離が大きいため)　⇒ したがって、保存すべき頂点は常に2頂点のみであり、これであれば全頂点を始点に考えても 計算量が O(2N) となるので十分高速である。

➁ 別解

マンハッタン距離の最大値を考える場合、座標系を 45°回転するのが定石であるようだ。これによって、各軸(新しい方)ごとに独立に最大値,最小値を求めて、これらの差を比較することで答えが得られるようになる。具体的には max((max X - min Xi) , (max Yi - min Yi) )を計算する。ただし、Xi,Yi = yi-xi ,xi+yi


![image](https://user-images.githubusercontent.com/109026838/215762658-d0c639e7-5eaf-46ec-8a1b-ac2ed4f430a0.png)

図のように回転させた系では同じマンハッタン距離を持つ頂点が正方形を成し、この距離が ((max X - min Xi) , (max Yi - min Yi)) に一致することが視覚的に理解できる。

厳密には

![image](https://user-images.githubusercontent.com/109026838/215763896-fa19a509-3a32-43be-95de-55772eb13f02.png)

参考：https://kagamiz.hatenablog.com/entry/2014/12/21/213931



補足

45度回転に忠実に、Xi = yi - xi として定義したが、Xi= xi - yi としても最大値と最小値の差 に変化は生じない。


# 192E Train 

解答遷移 TLE 降参

(終了時 42:56)

備考

➀ 思考

コストを (到着時刻 + K -1)　* K + T としてダイクストラでグラフを構築すればよいと考えた　⇒ TLE ⇒ 原因がわからず降参　⇒ ヒープから取り出した[コスト,頂点]で進めるべきか、コストで判定する部分をシンプルに間違えていたことで余計な計算が発生していたので、修正しAC

➁ 補足

ACコードは ダイクストラのテンプレであるので、確認できるようにアルゴ式にもコピーしておいた。ダイクストラは類題が少なく自然に覚えるのは難しいので。











