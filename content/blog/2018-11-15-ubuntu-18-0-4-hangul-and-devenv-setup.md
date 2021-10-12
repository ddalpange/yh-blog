---
title: Ubuntu 18.04 개발환경, 한글 세팅하기
date: 2018-11-15 15:34:40
thumbnail: https://i2.wp.com/wp.laravel-news.com/wp-content/uploads/2016/12/laravel-valet-ubuntu.png?resize=2200%2C1125
tags: [Ubuntu, Hangul]
categories: [Linux]
---

우분투 18.04 버전에서 개발환경과 한글 세팅하는 방법을 설명합니다.

<!-- more -->

### 사전 준비

1. 패키지 설치하기

	```bash
	sudo apt-get install uim
	sudo apt-get install gnome-tweak-tool
	sudo apt-get install chrome-gnome-shell
	sudo apt-get install zsh
	sudo apt-get install curl
	sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
	chsh -s /usr/bin/zsh
	```

2. [Gnome Shell Intergration](https://chrome.google.com/webstore/detail/gnome-shell-integration/gphhapmejobijbbhgpjhcjognlahblep?hl=en) 설치하기



### 한글 설정하기







1. Tweaks 실행하기



2. Keyboard & Mouse > Additional Layout Options > Korean Hangul/Hanja keys > Right Alt as Hangul, right Ctrl as Hanja 체크



3. Language Support > Keyboard input method system 여기서 uim으로 변경



4. 재부팅 후 Input Method 실행



5. Global Settings > Input method delployment > Specity default IM 체크



6. Global Settings > Input method delployment > Default input method 를 Byeoru로 변경



7. Global Settings > Input method toggle > Enable input method toogle by hot keys 체크



8. Global Settings > Input method toggle > Alternative input method 를 Byeoru로 변경



9. Byepri key bindings 1 > Byeoru [on]/[off] 를 "hangul"로 변경 



10. 재부팅 후 잘 되나 확인





2번을 모르는 사람들이 많은데 저걸 체크해주면 Xmodmap에서 부팅해줄때마다 오른쪽 Alt의 키코드를 바꿀 필요가 없다.



### 테마 세팅하기



[gnome look](https://www.gnome-look.org/)에서 원하는 테마 및 아이콘을 다운받은 후 아이콘은 `~/icons` 또는 `/usr/share/icons` 디렉토리에, 테마는 `~/.themes` 또는 `/usr/share/themes` 디렉토리에 복사하고 적용하면 된다. 



Dock은 [Gnome Shell Extension](https://extensions.gnome.org/)에서 Dash To Dock을 적용하였고 쓸데없는 메뉴바를 사라지기 위해 Unite도 설치하였다.



<!--stackedit_data:
eyJoaXN0b3J5IjpbOTgwNzI3MjgwLDk1MDkyODAzMCwxMDY2Nj
U5NTc1LDEzNjU1NzQwMzFdfQ==
-->