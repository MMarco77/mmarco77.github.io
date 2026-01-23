```
                     ▄▄       
                     ██       
 ████████  ▄▄█████▄  ██▄████▄ 
     ▄█▀   ██▄▄▄▄ ▀  ██▀   ██ 
   ▄█▀      ▀▀▀▀██▄  ██    ██ 
 ▄██▄▄▄▄▄  █▄▄▄▄▄██  ██    ██ 
 ▀▀▀▀▀▀▀▀   ▀▀▀▀▀▀   ▀▀    ▀▀ 
 ```

 ## My default configuration

 ```ini
```

### 

```ini
# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
export ZSH="$HOME/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
# ZSH_THEME="robbyrussell"
# ZSH_THEME="af-magic"
ZSH_THEME="spaceship"

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in $ZSH/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment one of the following lines to change the auto-update behavior
# zstyle ':omz:update' mode disabled  # disable automatic updates
# zstyle ':omz:update' mode auto      # update automatically without asking
# zstyle ':omz:update' mode reminder  # just remind me to update when it's time

# Uncomment the following line to change how often to auto-update (in days).
# zstyle ':omz:update' frequency 13

# Uncomment the following line if pasting URLs and other text is messed up.
# DISABLE_MAGIC_FUNCTIONS="true"

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# You can also set it to another string to have that shown instead of the default red dots.
# e.g. COMPLETION_WAITING_DOTS="%F{yellow}waiting...%f"
# Caution: this setting can cause issues with multiline prompts in zsh < 5.7.1 (see #5765)
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git z colorize colored-man-pages jump zsh-syntax-highlighting rust)

source $ZSH/oh-my-zsh.sh

# User configuration

# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"

# Android Studio alias
alias astudio="/usr/local/android-studio/bin/studio.sh"

# Add exercism.io path
export EXERCISMPATH="/home/user/bin/exercism"
export PATH="$PATH:$EXERCISMPATH"

# Export Golang
export GOLANG_PATH="/usr/lib/go-1.18/bin"
export PATH="$PATH:$GOLANG_PATH"

# Open Banner
neofetch

# Ghidra
export GHIDRA_PATH="/home/user/bin/ghidra_10.3_PUBLIC"
export PATH="$PATH:$GHIDRA_PATH"
alias ghidra="$GHIDRA_PATH/ghidraRun"

# Flutter path
export PATH_OF_FLUTTER_GIT_DIRECTORY="/home/user/snap/flutter/common/flutter"
export PATH="$PATH:$PATH_OF_FLUTTER_GIT_DIRECTORY/bin"

# Android SDK/NDK
export ANDROID_HOME="$HOME/Android/Sdk"
export ANDROID_SDK_ROOT="$ANDROID_HOME/platforms"
export ANDROID_NDK_ROOT="$ANDROID_HOME/ndk-bundle"
export ANDROID_NDK_HOME="${ANDROID_HOME}/ndk/${ANDROID_NDK_VERSION}"
export PATH="$ANDROID_HOME/cmdline-tools/latest/bin/:$PATH"
export PATH="$ANDROID_HOME/emulator/:$PATH"
export PATH="$ANDROID_HOME/platform-tools/:$PATH"
export ANDROID_NDK_VERSION="25.2.9519653"
export NDK_HOME="${ANDROID_HOME}/ndk/${ANDROID_NDK_VERSION}"

# Alias Android emulator
alias pixel_5='emulator @Pixel_5_API_31 -no-boot-anim -netdelay none -no-snapshot&'
alias pixel_5_wiped='emulator @pixel_5 -no-boot-anim -netdelay none -no-snapshot -wipe-data &'

# Docker
kali () {
	if [[ $(systemctl is-active docker.service) == "inactive" ]]
	then
		sudo systemctl start docker
	fi
	xhost +local:docker
	pushd /media/user/Backup/kali_docker > /dev/null
	docker compose up -d --build
	popd > /dev/null
	echo "[+] Kali is running."
	echo "[+] docker exec -it kali zsh"
}

kali_shell() {
	running=$(docker container inspect -f '{{.State.Running}}' kali 2>/dev/null)
	if [ "$running" = "true" ]
	then
		docker exec -it kali zsh
	else
		echo "[-] kali container is not running"
	fi
}

# Atuin smart history
eval "$(atuin init zsh)"

# apktool
export APKTOOL_PATH="$HOME/bin/apktool"
export PATH="$PATH:$APKTOOL_PATH"

# Jadx-gui
export JADX_VERSION="1.4.7"
export JADX_BIN="$HOME/bin/jadx-$JADX_VERSION/bin"
export PATH="$PATH:$JADX_BIN"

# NPM
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

# JetBrain Rust Rover 
export RUST_ROVER_VERSION="2024.1.5"
export RUST_ROVER_BIN="$HOME/bin/RustRover-$RUST_ROVER_VERSION/bin"
export PATH="$PATH:$RUST_ROVER_BIN"

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# Add Neovim
export NEOVIM_PATH="/home/user/bin/nvim-linux64/bin"
export PATH="$PATH:$NEOVIM_PATH"

# Add for Helix
export HELIX_RUNTIME="/home/user/bin/helix/runtime"

# Add pyenv setup
# WARNING - Let's those lines at the bottom of theis file
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```