---
title: "docker buildでentrypoint.shが永久に見つからない"
date: 2023-10-27
---

```ruby
exec entrypoint.sh: no such file or directory
```

が出て詰むことがあった。

- Mac, Windows両方で発生した
- entrypoint.shは存在する
- COPY entrypoint.sh は成功している
- container内を見ようとしても上記エラーで落ちる

かなり困った状況。

## 解決策

[entrypoint file not found](https://stackoverflow.com/a/56791772)

entrypoint.shをCRLFからLFにする。

## 改行コード…

OSによって改行コードが異なっていて、LFはUNIXで使われる改行コード。

## 対応

`git config`まわりいじったほうが良いか？とも思ったが、数名で試したうちの一名だけで発生するのみだったので今回はピンポイントでLFに変更するのみにした。
