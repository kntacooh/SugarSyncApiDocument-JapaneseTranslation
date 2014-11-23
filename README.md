SugarSync API Document 日本語訳
=======================================
誰か代わりにやってください。

以下、メモ

---

OmegaT 3.1.7 を使用している。

### 諸注意

* 原文をローカル環境で更新した場合は、OmegaTを開く前に**必ず**コミット&amp;プッシュする。でないと、OmegaTを開いた時に最新の**リモートリポジトリ**に変更され、更新部分が消失する。自分の環境では。しかしどんな回避方法を取るべきか迷う。

### 原文をマークダウン形式に書き直す時に気をつける点

1. 外部リンクについては、絶対URLで、斜字・リンクテキストの前後に":::"をつける。

  ```
[*:::Developer Home:::*](https://www.sugarsync.com/dev/home.html)
  ```

  [*:::Developer Home:::*](https://www.sugarsync.com/dev/home.html)

2. 内部リンクについては、スラッシュから始まる場合は`/source`を先頭につけ、そうでない相対URLはそのままとする。

  * 太字のリンクは`**[text](link)**`とする(`[**text**](link)`ではない)。

  ```
[Developer Home](/source/dev/home.md)
  ```

  [Developer Home](/source/dev/home.md)

3. 画像については、斜字の代用テキスト、もしくは以下の形式の引用文を使用する。

  ```
*Create app button*
  ```

  *Create app button*

  ```
> *See the following picture: https://www.sugarsync.com/images/*
  ```

  > *See the following picture: https://www.sugarsync.com/images/*

4. アンカータグを忘れないようにする。

  ```
<a name="hoge"></a>
  ```

5. 内部リンクの拡張子を .html から .md に変更するのを忘れないようにする。

6. コードブロックについて、シンタックスハイライトは可能な限り適用させる。また、HTTPのリクエストヘッダ、レスポンスヘッダは全てxmlとみなす。

  javaのインデントについては、

  * ブロックのインデントは空白4つ
  * 行の分割時の継続行のインデントは空白8つ

  xmlのインデントについては、一律で空白2つとする。

  また、javaのとじ括弧、xmlの終了タグを忘れないようにする。

7. 表内の改行は`<br>`, リストは`<ul>`,`<ol>`,`<li>` を使用する。

8. URLを表す文字列でリンクの自動作成を回避させるにはコードブロックを用いる。

  ```
`https://www.sugarsync.com/dev/home.html`
  ```

  `https://www.sugarsync.com/dev/home.html`

9. (i)マークはDisc型リストで代用する。
