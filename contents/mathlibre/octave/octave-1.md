# Octave入門補足

## 連立一次方程式の解と逆行列について

「Octave入門」の中で連立一次方程式について触れたが，

```octave
> A=[1 0 1 2; -2 1 4 1; 4 -3 -4 1; -1 1 2 1];
> b=[6; 3; -3; 4];
```

として行列とベクトルを定義して

```octave
> A^-1 * b
```

として解を得てはいけない．むしろ，

```octave
> A ¥ b
```

とするのがよい．
（"¥"は「左からの除算」を意味する．ただし、ディスプレイ上は逆スラッシュ"\"として表示されるかもしれない．）

内部的には，逆行列は連立一次方程式を解いて得ているので，二度手間になる．
連立一次方程式の解は掃き出し法などを基にした効率的なアルゴリズムを用いて求めている．（Cramerの公式は使わない．）

## 繰り返しの利用について

繰り返しをするのにfor文やwhile文が使えるが，性能に配慮する場合できればこのような繰り返しでなくベクトルや行列の形で処理するのがよい．後者では内部でベクトルの処理を高速に行うルーチンが利用されるので格段に高速になる．

以下の例を試すとよい．
なお，"tic"はタイマーを起動し，"toc"はタイマーを停止して経過時間（秒）を表示する．

```octave
> n = 1; wns = randn(100000, 1);
> tic; for k=1:length(wns); if wns(k) > 0.5; idx(n) = k; n=n+1; end; end; toc

> n = 1; wns = randn(100000, 1);
> tic; idx = find (wns > 0.5); toc
```

結果が同じであることも確かめておくこと．（どうやって？）

つぎは「[Octave入門続編](octave-2.md)」へ進んで下さい．

## Thanks

性能の例は

「行列演算のすすめ」 <http://adlib.rsch.tuis.ac.jp/~akira/unix/octave/matrix.html>

によります．
このページにはほかの例も挙げられているので，参考にしてください．
