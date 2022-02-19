---
title: 調 Ubuntu 20.04 時採的坑
date: 2021-10-18 07:29:00
updated: 2021-10-18 07:29:00
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace: ''
  wrap: true
  hljs: true
prismjs:
  enable: true
  preprocess: true
  line_number: true
  tab_replace: ''
categories: 心得
tags: 
- Ubuntu
- 2021
--- 
## Dash to Dock Can't `make` and `make install`
```bash=
$ sudo apt install sassc
$ sudo apt-get install gettext
```
## Dash to Dock : Somthing's gone wrong
According to this link [Github issue : Unable to use the extension #1445](https://github.com/micheleg/dash-to-dock/issues/1445)
We know that Dash to Dock doesn't support Gnome 40 yet.
And Tuxman2 said that,
> Note: I took the Dash to dock extension from here: https://github.com/ewlsh/dash-to-dock/tree/ewlsh/gnome-40
That version works with Gnome 40. Of course, it is not perfect yet (some bugs) but it does the job.

> @Sidpatchy: **Dash to Dock extension works with Gnome 40 but you must take the version from Ewlsh and not from Micheleg.**

## Cannot turn on Tweak User Themes and Shell button locked
The first problem is the `Tweak > Appearance > Shell (Locked by the triangle mark) `
**Solution** : Go to `Tweak > Extension > (Turn on) User Themes`
The second problem is "After I turn on the User Themes, it will turn of by itself.". 
Hence, refer to this https://ubuntuhandbook.org/index.php/2017/05/enable-shell-theme-in-gnome-tweak-tool-in-ubuntu/
Open terminal (Ctrl + Alt + T)
```bash=
$ sudo apt install chrome-gnome-shell
```
go to https://extensions.gnome.org/extension/19/user-themes/ and turn on the button.

## What is `sudo nautilus`
Nautilus is a file manager in Ubuntu.
When you type the command `sudo nautilus` , you are entering the file manager as a root.

## VSCode transparent
### Devilspie
參考資料：[這裡](https://www.youtube.com/watch?v=PzObHq72Vug&t=301s&ab_channel=ThatDevOpsGuy)
以下程式碼複製貼上至 Terminal
```bash=
$ sudo apt-get install devilspie
$ mkdir -p ~/.devilspie
$ echo '
(if (contains (window_class) "Code")
	(begin
		(spawn_async (str "xprop -id " (window_xid) " -f _KDE_NET_WM_BLUR_BEHIND_REGION 32c -set _KDE_NET_WM_BLUR_BEHIND_REGION 0 "))
		(spawn_async (str "xprop -id " (window_xid) " -f _NET_WM_WINDOW_OPACITY 32c -set _NET_WM_WINDOW_OPACITY 0xD8000000"))
	)
)
' > ~/.devilspie/vscode_transparent.ds
```
使用前前往 Terminal
```bash=
$ devilspie
```
就可以啟動了
### Auto Start Devilspie
`vim ~/.config/autostart/devilspie.desktop`
```=
[Desktop Entry]
Name="devilspie"
GenericName="devilspie"
Comment="is this necesery?"
Exec=/usr/bin/devilspie
Terminal=false
Type=Application
X-Gnome-Autostart=true
```
`:wq` 就完成了

## Ubuntu 20.04 Lock screen background cannot change
```bash=
$ sudo apt install libglib2.0-dev-bin
$ git clone https://github.com/thiggy01/change-gdm-background.git
$ chmod +x change-gdm-background
$ cd change-gdm-background
$ sudo ./change-gdm-background [your image path]
```
* Line 5 **[ your image path ]** : 可以用 `sudo nautilus` 進去 `/usr/share/backgrounds` 把你想要更換的桌布放進去。

詳細的可以參考以下這兩篇
* [How to change the login picture in Ubuntu 20.04](https://www.linuxmadesimple.info/2020/08/how-to-change-login-picture-in-ubuntu.html)
* [How to change the login picture in Ubuntu 20.04 (Youtube)](https://www.youtube.com/watch?v=KY6uB3lUT8s&ab_channel=linuxmadesimple)

## 雜項
### Vimrc 配置
我是參考 [余原齊](https://blog.smallten.tk/) 電神的文章 [Use Vim as IDE](https://hackmd.io/@Adam7066/SJ5ERzgHv)