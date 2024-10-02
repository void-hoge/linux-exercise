# セットアップ
## 1. パッケージリストのアップデート, システムのアップグレード
```shellsession
$ sudo apt update
$ sudo apt upgrade
```
## 2. sudo のインストール
```shellsession
$ su
$ apt install sudo
$ gpasswd -a <user> sudo
$ exit (re-login)
```

## 3. テキストエディタのインストール

```shellsession
$ sudo apt install emacs vim
```

## 4. 基本的な開発環境のインストール
```shellsession
$ sudo apt install build-essential cmake git
```

## 5. capslock の無効化
- `/etc/default/keyboard` の `XKBOPTIONS` を `ctrl:nocaps` に変更
- `/sbin/reboot`

## 6. fish のインストール
```shellsession
$ sudo apt install fish
$ chsh -s /bin/fish
$ exit (re-login)
```
`$HOME/.config/fish/config.fish` に, `set -x PATH /bin /sbin $HOME/bin` を追加
    
## 7. tmux のインストール
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

## 8. nvidia-driver のインストール
- `/etc/apt/sources.list`のどこかに、`deb https://ftp.jp.debian.org/debian/ bullseye main non-free contrib`を追加
   
```
$ sudo apt update
$ sudo apt install nvidia-detect
$ nvidia-detect
Detected NVIDIA GPUs:
03:00.0 VGA compatible controller [0300]: NVIDIA Corporation Device [10de:2504] (rev a1)

Checking card:  NVIDIA Corporation Device 2504 (rev a1)
Your card is supported by the default drivers.
Your card is also supported by the Tesla 460 drivers series.
It is recommended to install the
    nvidia-driver
package.
$ sudo apt install nvidia-driver
$ nvidia-smi
Mon Apr 18 22:27:18 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 460.91.03    Driver Version: 460.91.03    CUDA Version: 11.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  GeForce RTX 3060    On   | 00000000:03:00.0  On |                  N/A |
|  0%   50C    P8    14W / 170W |    223MiB / 12045MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A     15323      G   /usr/lib/xorg/Xorg                159MiB |
|    0   N/A  N/A     15452      G   ...347265267157153257,131072       61MiB |
+-----------------------------------------------------------------------------+
```

## 9. サウンドドライバのインストール
```shellsession
$ sudo apt install alsa-utils pulseaudio
```


## 10. pip と python-xlib のインストール
```shellsession
$ sudo apt install python-pip
$ pip3 install xlib
```

## 11. X11 と urxvt のインストール
```shellsession
$ sudo apt install xbase-clients rxvt-unicode 
```

## 12. powerline フォントのインストール
```shellsession
$ cd
$ git clone https://github.com/powerline/fonts.git
$ cd fonts
$ ./install.sh
```

## 13. ウィンドウマネージャのインストール
```shellsession
$ sudo apt install imagemagick
$ cd
$ git clone https://github.com/void-hoge/hogewm.git
$ cp hogewm/home/.* .
```

## 14. ウィンドウマネージャの起動
```shellsession
$ startx
```
    - C-M-1 でターミナルを開く.
    - C-M-2 でテキストエディタを開く.
    - C-M-3 でウェブブラウザを開く (デフォルトは google-chrome なので開けない).

## 15. ウェブブラウザのインストール
```shellsession
$ sudo apt install firefox-esr
```
    - firefox を開き, google-chrome のインストーラ (deb) ファイルをとってきてインストール.
    - hogewm で, ブラウザを `google-chrome` から `firefox` に変更する.

## 16. LaTeX 環境の作成
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
