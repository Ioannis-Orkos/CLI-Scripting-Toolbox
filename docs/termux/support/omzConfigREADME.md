## Setup ohmyzsh

>copy and paste the following contents on termiux one afer the other

#### Initialilze with backing up other terminals
```sh

setup_omz()  

# backup previous termux and omz files

echo -e ${RED}"[*] Setting up OMZ and termux configs..."

omz_files=(.oh-my-zsh .termux .zshrc)

for file in "${omz_files[@]}"; do
       echo -e ${CYAN}"\n[*] Backing up $file..."
        if [[ -f "$HOME/$file" || -d "$HOME/$file" ]]; then
                 { reset_color; mv -u ${HOME}/${file}{,.old}; }
        else
                 echo -e ${MAGENTA}"\n[!] $file Doesn't Exist."
        fi
done

```

#### Installiation
```sh

echo -e ${CYAN}"\n[*] Installing Oh-my-zsh... \n"
{ 
        reset_color; 
        git clone https://github.com/robbyrussell/oh-my-zsh.git --depth 1 $HOME/.oh-my-zsh; 
}
cp $HOME/.oh-my-zsh/templates/zshrc.zsh-template $HOME/.zshrc
sed -i -e 's/ZSH_THEME=.*/ZSH_THEME="aditya"/g' $HOME/.zshrc

```

#### Setting up theme and configuration for omz

```sh

# ZSH theme
cat > $HOME/.oh-my-zsh/custom/themes/aditya.zsh-theme <<- _EOF_
# Default OMZ theme
if [[ "\$USER" == "root" ]]; then
PROMPT="%(?:%{\$fg_bold[red]%}%{\$fg_bold[yellow]%}%{\$fg_bold[red]%} :%{\$fg_bold[red]%} )"
PROMPT+='%{\$fg[cyan]%}  %c%{\$reset_color%} \$(git_prompt_info)'
else
PROMPT="%(?:%{\$fg_bold[red]%}%{\$fg_bold[green]%}%{\$fg_bold[yellow]%} :%{\$fg_bold[red]%} )"
PROMPT+='%{\$fg[cyan]%}  %c%{\$reset_color%} \$(git_prompt_info)'
fi
ZSH_THEME_GIT_PROMPT_PREFIX="%{\$fg_bold[blue]%}  git:(%{\$fg[red]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{\$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{\$fg[blue]%}) %{\$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%{\$fg[blue]%})"
_EOF_

# Append some aliases
cat >> $HOME/.zshrc <<- _EOF_
#------------------------------------------
alias l='ls -lh'
alias ll='ls -lah'
alias la='ls -a'
alias ld='ls -lhd'
alias p='pwd'
#alias rm='rm -rf'
alias u='cd $PREFIX'
alias h='cd $HOME'
alias :q='exit'
alias grep='grep --color=auto'
alias open='termux-open'
alias lc='lolcat'
alias xx='chmod +x'
alias rel='termux-reload-settings'
#------------------------------------------
# SSH Server Connections
# linux (Arch)
#alias arch='ssh UNAME@IP -i ~/.ssh/id_rsa.DEVICE'
# linux sftp (Arch)
#alias archfs='sftp -i ~/.ssh/id_rsa.DEVICE UNAME@IP'
_EOF_

# configuring termux
echo -e ${CYAN}"\n[*] Configuring Termux..."
if [[ ! -d "$HOME/.termux" ]]; then
mkdir $HOME/.termux
fi

# copy font
cp $(pwd)/files/.fonts/icons/dejavu-nerd-font.ttf $HOME/.termux/font.ttf

# color-scheme
cat > $HOME/.termux/colors.properties <<- _EOF_
background 		: #263238
foreground 		: #eceff1
color0  			: #263238
color8  			: #37474f
color1  			: #ff9800
color9  			: #ffa74d
color2  			: #8bc34a
color10 			: #9ccc65
color3  			: #ffc107
color11 			: #ffa000
color4  			: #03a9f4
color12 			: #81d4fa
color5  			: #e91e63
color13 			: #ad1457
color6  			: #009688
color14 			: #26a69a
color7  			: #cfd8dc
color15 			: #eceff1
_EOF_

# button config
cat > $HOME/.termux/termux.properties <<- _EOF_
extra-keys = [ \\
['ESC','|', '/', '~','HOME','UP','END'], \\
['CTRL', 'TAB', '=', '-','LEFT','DOWN','RIGHT'] \\
]
_EOF_

# change shell and reload configs
{ chsh -s zsh; } \
&& { echo -e "${GREEN}Changed shell to /bin/zsh"; } \
|| { echo -e "${MAGENTA}Failed to change shell. Please run $ chsh -s zsh"; }
{ termux-reload-settings; } \
&& { echo -e "${GREEN}Settings reloaded successfully"; } \
|| { echo -e "${MAGENTA}Failed to run $ termux-reload-settings. Restart app after installation is complete"; }
{ termux-setup-storage; } \
&& { echo -e "${GREEN}Ran termux-setup-storage successfully, you should now have a ~/storage folder"; } \
|| { echo -e "${MAGENTA}Failed to execute $ termux-setup-storage"; }

```