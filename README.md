SugarSync API Document 日本語訳
=======================================
誰か代わりにやってください。

以下、メモ

---

OmegaT3系を使用

5. 原文を更新した場合は、OmegaTを開く前にコミット&amp;プッシュする

1. 外部リンクについては、絶対URLで、斜字・リンクテキストの前後に":::"をつける

  `[*:::Developer Home:::*](https://www.sugarsync.com/dev/home.html)`
  [*:::Developer Home:::*](https://www.sugarsync.com/dev/home.html)

7. 内部リンクについては、スラッシュから始まる場合は`/source`を先頭につけ、そうでない相対URLはそのままとする

  `[Developer Home](/source/dev/home.md)`
  [Developer Home](/source/dev/home.md)

  * 太字のリンクは`**[text](link)**`とする(`[**text**](link)`ではない)。

2. (i)のマークはDisc型リストで代用

3. 画像については、斜字の代用テキスト、もしくは以下の形式の引用文を使用?

  `*Create app button*` *Create app button*

  ```
> *See the following picture: https://www.sugarsync.com/images/*
  ```

  > *See the following picture: https://www.sugarsync.com/images/*

4. アンカータグを忘れない

  `<a name="hoge"></a>`

6. 内部リンクの拡張子を .html から .md に変更するのを忘れない

8. 表内の改行は`<br>`を入れましょう
