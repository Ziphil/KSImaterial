# R Matrix

## 行列演算

Rは行列演算も得意である。

```r
> A <- matrix(c(1, 0, 0, 0, -1, -2, 0, 0, 0, 0, -2, 1, 0, 0, 3, 1),
+ nrow=4, ncol=4)    # 途中でリターンを押すと、"+"のプロンプトが出るので続けて入力する
> B <- matrix(c(2, 0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 2, 0, 0, 1, 3), nrow=4)  
                     # カラム数は省いても明らかなのでOK。ncolだけ指定しても可。
> A; B
     [,1] [,2] [,3] [,4]
[1,]    1   -1    0    0
[2,]    0   -2    0    0
[3,]    0    0   -2    3
[4,]    0    0    1    1
     [,1] [,2] [,3] [,4]
[1,]    2    1    0    0
[2,]    0    1    0    0
[3,]    0    0    1    1
[4,]    0    0    2    3
```

ベクトルで与えた要素を、
行数(nrow)と列数(ncol)に従って行列に配置するコマンド`matrix()`を使っている。
要素は列優先（縦方向に並べられる）ことに注意せよ。

```r
> A + B                 # 要素ごとの和
     [,1] [,2] [,3] [,4]
[1,]    3    0    0    0
[2,]    0   -1    0    0
[3,]    0    0   -1    4
[4,]    0    0    3    4 
> A * B              # 要素ごとの績
     [,1] [,2] [,3] [,4]
[1,]    2   -1    0    0
[2,]    0   -2    0    0
[3,]    0    0   -2    3
[4,]    0    0    2    3
> A %*% B           # 行列としての績
     [,1] [,2] [,3] [,4]
[1,]    2    0    0    0
[2,]    0   -2    0    0
[3,]    0    0    4    7
[4,]    0    0    3    4
```

小さな行列を編集したり作ったりするには、表計算ソフト風の入力支援が得られる`edit()`を合わせて使うと良い。

```r
> A <- edit(A)
```

とすると、行列Aの内容を示す別ウィンドウが起動する。
要素をクリックし、書き換えることができる。

```r
> edit(matrix(0, nrow=3, ncol=3)) -> X
```

とすると、3x3の零行列を初期値とする別ウィンドウが起動するので、
適当に値を入力し、ウィンドウを閉じるとXに行列の値が設定される。

```r
> edit(matrix(0, ncol=4, nrow=4)) -> M  # 行列Mを入力
> M       # 確認
     col1 col2 col3 col4
[1,]    3    3   -5   -6
[2,]    1    2   -3   -1
[3,]    2    3   -5   -3
[4,]   -1    0    2    2
> det(M)   # 行列式
[1] 1
> solve(M)   # 逆行列（誤差が加わっていることに注意）
              [,1] [,2] [,3]          [,4]
col1  4.000000e+00   18  -16 -3.000000e+00
col2 -1.110223e-16   -1    1  1.000000e+00
col3  1.000000e+00    3   -3  3.330669e-16
col4  1.000000e+00    6   -5 -1.000000e+00 
> b <- matrix(c(1, 2, 1, 3), ncol=1)   # 縦ベクトル
> b
     [,1]
[1,]    1
[2,]    2
[3,]    1
[4,]    3
> solve(M, b)    # 連立一次方程式 Mx = b を解く
     [,1]
col1   15
col2    2
col3    4
col4    5
> solve(M, b) -> a   # 解を代入
> M %*% a       # 検算；bになる
     [,1]
[1,]    1
[2,]    2
[3,]    1
[4,]    3
```

次の場合は、逆行列を求めるより連立一次方程式を解くほうが精度的にも時間的にも良い。

```r
> P <- matrix(c(1, -1, 2, 3, -2, 4, 2, -1, 3), ncol=3)
> A <- matrix(c(-1, 2, -4, -5, 4, -8, -1, 0, 0), ncol=3)
> A
     [,1] [,2] [,3]
[1,]   -1   -5   -1
[2,]    2    4    0
[3,]   -4   -8    0
> B <- solve(P)    # B = P^(-1)
> B %*% A %*% P   # P^(-1)APで対角化
     [,1] [,2] [,3]
[1,]    2    0    0
[2,]    0    1    0
[3,]    0    0    0
> X <- solve(P, A)  # そのかわりに、P^(-1)A を PX = Aの解として求める
> X
     [,1] [,2] [,3]
[1,]   -4   -2    2
[2,]    1   -1   -1
[3,]    0    0    0
> X %*% P    # 結果は同じ
     [,1] [,2] [,3]
[1,]    2    0    0
[2,]    0    1    0
[3,]    0    0    0
```

固有値、固有ベクトルも求められる。

```r
> A = matrix(c(6, -1, 5, -3, 2, -3, -7, 1, -6), ncol=3)
> A
     [,1] [,2] [,3]
[1,]    6   -3   -7
[2,]   -1    2    1
[3,]    5   -3   -6
> eigen(A)    # 固有値固有ベクトルを求めるコマンド
$values
[1]  2  1 -1

$vectors
           [,1]      [,2]          [,3]
[1,] -0.5773503 0.8164966 -7.071068e-01
[2,]  0.5773503 0.4082483 -9.440836e-16
[3,] -0.5773503 0.4082483 -7.071068e-01
> eigen(A) -> C
> C$vectors[,1]     # 固有ベクトルの一つ
[1] -0.5773503  0.5773503 -0.5773503
> A %*% C$vectors[,1]  # 検算したいが
          [,1]
[1,] -1.154701
[2,]  1.154701
[3,] -1.154701 
> A %*% C$vectors[,1] / C$vectors[,1]   # 要素ごとに割り算するので固有値が出るはず
     [,1]
[1,]    2
[2,]    2
[3,]    2
> eigen(A, only.values=TRUE)    # 固有値だけが必要な場合
$values
[1]  2  1 -1

$vectors
NULL

> eigen(A, only.values=TRUE)$values  # 固有ベクトルの表示をしない
[1]  2  1 -1
```

上の例で、`eigen()`の出力から固有値だけ、固有ベクトルだけを取り出すのに
`$value, $vectors`
という表記を使っている。
これは、`eigen()`の出力は実はリストというものになっていて、
それは名前（valueなど）とそれに対応する値（固有値からなる（Rでいう）ベクトル）の組である。
リストから一つの名前に対応する値を取り出すのに$というオペレータを使うのである。

---

行列の例は、

* 齋藤正彦『線型代数入門』（東京大学出版会）

からとりました。

次は、[R データ入力とグラフィックス](data.md)に進んでください。