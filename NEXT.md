# NEXT: 最近開いたファイル

`v:oldfiles` を popup で絞って開く。shada 由来の読み取り専用ビュー（自前の履歴を持たない）。

## やること

- `:Trf` で `v:oldfiles` を候補にする
- 表示は `fnamemodify(f, ':~')`
- `filereadable()` で消えたファイルを除外（`:edit` が新規作成してしまうのを防ぐ）
- 確定: `edit`
- キー操作・見た目は tff と同じ

## スコープ外

- 自前の履歴保存・削除（状態を持たないのが利点）
- ピン留め・ブックマーク

## 注意

- `v:oldfiles` は viminfo 由来。`'viminfo'` に `'`（ファイルマーク、既定100）が無いと空になる
- テストは `add(v:oldfiles, path)` で直接足せる（実測で書込可）。削除ファイルの除外もここで検証

## 共通の約束（tpf/tff と同じ）

- Vim9のみ・依存なし・KISS・状態最小。`plugin/trf.vim` + `autoload/trf.vim` の2ファイル
- 骨格は tff のコピーでよい（popup + filter、先頭行=入力欄、`> `マーカー、border:[]、padding:[0,1,0,1]、20件）
- 2本目以降で共通コア切り出しを判断（時期尚早ならコピーのまま）
- 罠: def引数名とスクリプト変数の衝突不可(E1168)／テストは `--cmd "set rtp+=$PWD"`／`writefile(/dev/stderr)` 禁止
