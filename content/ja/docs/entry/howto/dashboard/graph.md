---
Title: グラウウィジェットを利用する
Date: 2018-10-31T15:18:46+09:00
URL: https://mackerel.io/ja/docs/entry/howto/dashboard/graph
EditURL: https://blog.hatena.ne.jp/mackerelio/mackerelio-docs-ja.hatenablog.mackerel.io/atom/entry/10257846132662339682
---

# グラフウィジェットを利用する
グラフウィジェットを利用すると、カスタムダッシュボードにグラフを表示することができます。

[f:id:mackerelio:20181102153824p:plain]

## グラフウィジェットを作成する
グラフウィジェットのアイコンをドラッグ&amp;ドロップして、ウィジェットを作成する位置を決定します。

[f:id:mackerelio:20181102173147p:plain]

続いてウィジェットに表示するグラフを選択します。選択できるグラフは

- ホストグラフ
- ロールグラフ
- サービスグラフ
- 式グラフ[^1]

のいずれかです。

[f:id:mackerelio:20181102152010p:plain]

次にグラフの表示期間を選択します。表示期間は以下のいずれかを選択できます。

<dl>
    <dt>期間の変更を可能にする</dt>
    <dd>ダッシュボードの編集が完了した後、画面上部のコントローラーを操作したり、グラフ内の領域を選択することで、グラフの表示期間を変更できます。</dd>
    <dt>現在時刻を基準に期間を指定する</dt>
    <dd>期間の長さと、期間の終端を現在時刻を基準にして指定します。「一年前の同時期のグラフが見たい」といったような場合にお使いいただけます。</dd>
    <dt>期間を固定して表示する</dt>
    <dd>常に指定した期間のグラフを表示します。</dd>
</dl>

選択が完了したら作成ボタンでウィジェットを作成します。

[^1]: 式グラフは[実験的機能](https://mackerel.io/ja/docs/entry/advanced/experimental-features)となっています。