# Linux 集中講座
## 日程
- 第 1 回: 10/03 (木) 5 限
- 第 2 回: 10/09 (水) 3 限
- 第 3 回: 10/09 (水) 4 限
- 第 4 回: 10/10 (木) 5 限
- 第 5 回: 10/16 (水) 3 限
- 第 6 回: 10/16 (水) 4 限
- 第 7 回: 10/17 (木) 5 限


## 講師
- 野田麦 (石浦研究室, M2)
  
## 目的
- 普段の生活 & 研究を Linux 上で遂行できるようになる.
- UNIX の CUI 操作に慣れる.

## セットアップ
1. sudo のインストール
```shellsession
$ su
$ apt install sudo
$ gpasswd -a <user> sudo
$ exit (re-login)
```

2. パッケージリストのアップデート, システムのアップグレード
```shellsession
$ sudo apt update
$ sudo apt upgrade
```

3. テキストエディタのインストール

```shellsession
$ sudo apt install emacs vim
```

4. 基本的な開発環境のインストール
```shellsession
$ sudo apt install build-essential cmake git
```

5. capslock の無効化
   - `/etc/default/keyboard` の `XKBOPTIONS` を `ctrl:nocaps` に変更
   - `/sbin/reboot`

6. fish のインストール
```shellsession
$ sudo apt install fish
$ chsh -s /bin/fish
$ exit (re-login)
```
`$HOME/.config/fish/config.fish` に, `set -x PATH /bin /sbin $HOME/bin` を追加
    
7. tmux のインストール
```shellsession
$ sudo apt install tmux
$ cd /tmp
$ git clone https://github.com/thewtex/tmux-mem-cpu-load.git
$ cd tmux-mem-cpu-load
$ cmake .
$ make 
$ sudo make install
```

以下のファイルを `$HOME/.tmux.conf` として保存
```conf
set -g status-bg black
set -g status-fg white
set -g status-interval 1
set -g status-right "%Y-%m-%d (%a) %H:%M:%S #(tmux-mem-cpu-load --interval 1 --averages-count 0 --cpu-mode 1 --powerline-right --graph-lines 0)"
set -g status-right-length 100

set -g prefix C-q
bind -n C-o select-pane -t :.+
bind 3 split-window -h
bind 2 split-window -v
```

起動
```
$ tmux
```

8. サウンドドライバのインストール
```shellsession
$ sudo apt install alsa-utils pulseaudio
```

9. pip と python-xlib のインストール
```shellsession
$ sudo apt install python-pip
$ pip3 install xlib
```

10. X11 と urxvt のインストール

```shellsession
$ sudo apt install xbase-clients rxvt-unicode 
```

11. powerline フォントのインストール
```shellsession
$ cd
$ git clone https://github.com/powerline/fonts.git
$ cd fonts
$ ./install.sh
```

12. ウィンドウマネージャのインストール
```shellsession
$ sudo apt install imagemagick
$ cd
$ git clone https://github.com/void-hoge/hogewm.git
$ cp hogewm/home/.* .
```

13. ウィンドウマネージャの起動
```shellsession
$ startx
```
    - C-M-1 でターミナルを開く.
    - C-M-2 でテキストエディタを開く.
    - C-M-3 でウェブブラウザを開く (デフォルトは google-chrome なので開けない).

14. ウェブブラウザのインストール
```shellsession
$ sudo apt install firefox-esr
```
    - firefox を開き, google-chrome のインストーラ (deb) ファイルをとってきてインストール.
    - hogewm で, ブラウザを `google-chrome` から `firefox` に変更する.

15. LaTeX 環境の作成
```shellsession
$ sudo apt install texlive-lang-cjk xdvik-ja evince
$ sudo apt install texlive-fonts-recommended texlive-fonts-extra
```

以下を `$HOME/bin/maketex` として保存

```bash
#!/bin/bash -x
if [ "$1" = '' ]; then
	echo "no input file"
	exit
fi
​
name=`basename $1 .tex`
if [ $name = $1 ]; then
	echo "input file name must end with .tex."
	exit
fi
platex "$name.tex"
platex "$name.aux"
dvipdfmx "$name.dvi"
rm $name.aux $name.dvi *.log
```

実行ビット付与
```shellsession
$ chmod +x $HOME/bin/maketex
```
