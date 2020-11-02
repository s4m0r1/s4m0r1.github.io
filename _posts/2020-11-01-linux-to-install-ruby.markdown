---
layout: post
title:  "RubyやGemをLinuxにインストールする"
date:   2020-11-01 21:42:24 +0900
categories: Linux
---

# RubyやGemをインストールする

自分のやったことのメモをmdでよく残しているので、~~後悔~~公開しようかと思い[Jekyll](https://github.com/jekyll/jekyll)を選択したがRubyなど一式色々必要だったためインストールしたときのメモ

ディストリビューションはArchLinuxを使用しており何かとズレがあるかと思うのでこの記事通りやる場合適宜読み替えをしてください

## rbenvのインストール

rbenvのgithubは[ここ](https://github.com/rbenv/rbenv)

上記の記事の`Basic GitHub Checkout`を参考に実行しています

### GitHubからクローン

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
```

### フォルダに移動してコンパイル

```bash
cd ~/.rbenv && src/configure && make -C src
```

### パスを通す

.bashrcに書き込みました

```bash=.bashrc
echo '# Ruby'
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "(rbenv init)"'
exec bash
```

### rbenvを実行してみる

```bash
rbenv -v
rbenv 1.1.2-36-g60c9339
```

## ruby-buildをインストール

rbenvでrubyをインストールする際に必要らしいので[AUR](https://aur.archlinux.org/packages/ruby-build/)で見つけたものをインストール

### クローン
```bash
git clone https://aur.archlinux.org/ruby-build.git
```

### インストール

```bash
makepkg -si
```

### ruby-buildを実行してみる

```bash
ruby-build --version
ruby-build 20201005
```
## rbenvでインストール

### 安定版を探す

```bash
rbenv install --list
2.5.8
2.6.6
2.7.2
jruby-9.2.13.0
maglev-1.0.0
mruby-2.1.2
rbx-5.0
truffleruby-20.2.0
truffleruby+graalvm-20.2.0
```

### インストール
```bash
Downloading ruby-2.6.6.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.6.tar.bz2
Installing ruby-2.6.6...
Installed ruby-2.6.6 to /home/smr/.rbenv/versions/2.6.6
```

### バージョンの固定
```bash
rbenv global 2.6.6
```

### rubyの実行
```bash
ruby -v
ruby 2.6.6p146 (2020-03-31 revision 67876) [x86_64-linux]
```

## Bundlerをインストール

### gemを使ってbundlerをインストール
```bash
rbenv exec gem install bundler
```

### bundlerの実行
```bash
bundler -v
Bundler version 2.1.4
```

