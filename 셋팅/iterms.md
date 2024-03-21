
oh-my-zsh 설치


sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"


iterm 테마변경

[테마목록](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)
ZSH_THEME 변수 변경


PC 상태바 추가
- Settings > Preference > Profile > Sessions
	- "Status bar enabled" 체크
- Settings > Preference > Appearance
	- "Status bar location" Bottom 으로 변경
- 원하는 바 선택 셋팅


플러그인

plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
)


자동완성 플러그인

$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

하이라이팅

$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting



vi 에서 스크롤동작 셋팅
- Settings > Advance
	- "Scroll wheel sends arrow keys" Yes로 변경