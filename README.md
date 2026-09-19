# 最佳 WSL + Ubuntu 22 環境設定

這份文件主要是用來快速設定 WSL 2 上的 Ubuntu 22 環境，包含了常用的工具和配置。

## 使用前請先確認

這是一份**版本敏感且帶有個人偏好的環境設定指南**，不是所有 WSL 使用者都必須照做的官方安裝清單。

- 先確認你的 Windows、WSL、Ubuntu 版本與 CPU 架構。
- 標示為個人偏好的字型、shell、工具與 alias，可以自行略過。
- 任何會改寫 sudo、shell profile、SSH key、系統檔案或從網路下載並直接執行程式的指令，都應先理解內容與回復方式。
- 指令若與目前官方文件不同，以目前官方文件與實際版本為準。
- 維護與驗證原則見 [MAINTENANCE.md](MAINTENANCE.md)。

## 設定 Windows Terminal 終端機環境

設定 WSL + Ubuntu 22 環境的第一步就是選一個好用的終端機環境，這樣可以讓你在 Windows 上更方便地使用 WSL。

這裡我唯一推薦 [Windows Terminal](https://github.com/microsoft/terminal) 應用程式，沒有之一！✨

如果你還沒有安裝 Windows Terminal，可以從 Microsoft Store 下載：

- [Windows Terminal - 在 Windows 上免費下載並安裝 | Microsoft Store](https://aka.ms/terminal)

你也可以安裝預覽版的 Windows Terminal，這樣可以體驗最新的功能和改進：

- [Windows Terminal Preview - 在 Windows 上免費下載並安裝 | Microsoft Store](https://aka.ms/terminal-preview)

想用命令列快速安裝也可以執行：

```sh
winget install --id Microsoft.WindowsTerminal -e
```

基本參數設定請參考 [Install and get started setting up Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/install?WT.mc_id=DT-MVP-4015686) 文件。

我一定會做的設定，就是調整終端機中要使用的預設字型，請參考 [Appearance profile settings in Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/customize-settings/profile-appearance?WT.mc_id=DT-MVP-4015686) 進行設定，這個步驟超級重要！🔥

字型部分我也是唯一推薦 `JetBrainsMonoNL Nerd Font Mono` 字型，這個字型有很多好用的圖示，並且支援程式碼等寬顯示，非常美觀。但記得不要去 [JetBrains](https://www.jetbrains.com/lp/mono/) 官網下載字型，要去 [Nerd Fonts - Iconic font aggregator, glyphs/icons collection, & fonts patcher](https://www.nerdfonts.com/font-downloads) 下載名為 `JetBrainsMono Nerd Font` 的字型檔案回來，然後壓縮檔中有很多字型檔，你只需要安裝以下這四個字型：

1. `JetBrainsMonoNLNerdFontMono-Bold.ttf`
1. `JetBrainsMonoNLNerdFontMono-BoldItalic.ttf`
1. `JetBrainsMonoNLNerdFontMono-Light.ttf`
1. `JetBrainsMonoNLNerdFontMono-Regular.ttf`

記得在 Windows Terminal 的設定中，將「預設設定檔」的字型設定為 `JetBrainsMonoNL Nerd Font Mono` 字體。

## 安裝 WSL 2 服務

請參考微軟官網的 [How to install Linux on Windows with WSL](https://learn.microsoft.com/en-us/windows/wsl/install?WT.mc_id=DT-MVP-4015686) 文件。


現在只要以系統管理者身份執行以下命令就可以安裝完畢：

```sh
wsl --install
```

查詢有多少 Linux 發行版可供安裝：

```sh
wsl --list --online
```

```txt
The following is a list of valid distributions that can be installed.
Install using 'wsl.exe --install <Distro>'.

NAME                            FRIENDLY NAME
AlmaLinux-8                     AlmaLinux OS 8
AlmaLinux-9                     AlmaLinux OS 9
AlmaLinux-Kitten-10             AlmaLinux OS Kitten 10
AlmaLinux-10                    AlmaLinux OS 10
Debian                          Debian GNU/Linux
FedoraLinux-42                  Fedora Linux 42
SUSE-Linux-Enterprise-15-SP6    SUSE Linux Enterprise 15 SP6
SUSE-Linux-Enterprise-15-SP7    SUSE Linux Enterprise 15 SP7
Ubuntu                          Ubuntu
Ubuntu-24.04                    Ubuntu 24.04 LTS
archlinux                       Arch Linux
kali-linux                      Kali Linux Rolling
openSUSE-Tumbleweed             openSUSE Tumbleweed
openSUSE-Leap-15.6              openSUSE Leap 15.6
Ubuntu-18.04                    Ubuntu 18.04 LTS
Ubuntu-20.04                    Ubuntu 20.04 LTS
Ubuntu-22.04                    Ubuntu 22.04 LTS
OracleLinux_7_9                 Oracle Linux 7.9
OracleLinux_8_7                 Oracle Linux 8.7
OracleLinux_9_1                 Oracle Linux 9.1
```

安裝 Ubuntu-22.04 LTS 發行版本：

```sh
wsl --install -d Ubuntu-22.04
```

查看目前正在運作的 Linux 發行版本：

```sh
wsl --list --verbose
```

更新 [WSL 2 核心](https://aka.ms/wsl2kernel)： (這個步驟很重要)

```sh
wsl --update
```

設定預設 `wsl.exe` 啟用的 Linux 發行版本：

```sh
wsl --set-default Ubuntu-22.04
```

## 設定 Linux 使用者名稱和密碼

首次登入會需要設定一組自己的帳號密碼：

```sh
wsl
```

日後想以 `root` 身份登入可以使用：

```sh
wsl -u root
```

你有了 `root` 權限後，可以使用以下命令來設定任意使用者的密碼：

```txt
passwd <username>
```

## 隨時更新 Ubuntu 到最新版本

```sh
sudo apt update && sudo apt upgrade -y
```

> 💡 建議不要執行 `sudo apt dist-upgrade -y`

## 設定 WSL 開發環境

部分設定參考自 [Set up a WSL development environment](https://learn.microsoft.com/en-us/windows/wsl/setup/environment?WT.mc_id=DT-MVP-4015686) 文件，但大多是我個人的經驗與習慣。

### 設定無密碼變身 `root` 執行

> [!WARNING]
> 這會讓指定使用者在 WSL 內執行 `sudo` 時不再要求密碼，等於明顯降低本機提權的摩擦與保護。只有在你理解風險、這是個人受控開發環境、且確實需要時才使用；一般情況保留 sudo 密碼會更保守。

```sh
# sudoers
echo "will ALL=(ALL:ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/will
```

請記得將上述命令的 `will` 置換成你手冊登入 WSL 時註冊的帳號，如果你的帳號叫 `david` 的話，命令就是：

```sh
# sudoers
echo "david ALL=(ALL:ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/david
```

### 設定作業系統時區

```sh
# set timezone to +0800
sudo ln -sf /usr/share/zoneinfo/Asia/Taipei /etc/localtime \
    && sudo dpkg-reconfigure --frontend noninteractive tzdata
```

### 安裝常用的工具

> [!CAUTION]
> 下方部分安裝方式會從遠端下載程式、安裝腳本或預編譯 binary。執行前確認網址、CPU 架構與專案官方安裝方式；對 `curl | sh` 類型指令，若環境要求較高的供應鏈安全，先下載、檢查內容／checksum，再執行。

這些工具可以幫助你更方便地使用 Linux 環境，特別是對於開發者來說，這些工具都是必不可少的。

```sh
# Installing essential packages...
sudo apt update && sudo apt install -y wslu xdg-utils \
  net-tools ripgrep jq lftp moreutils btop bat zip zstd gnupg2 \
  ffmpeg 7zip poppler-utils fd-find zoxide imagemagick exiftool

# Install Rust: https://www.rust-lang.org/tools/install
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
# Upgrade Rust is you already installed long time ago
rustup update stable

# yazi: https://yazi-rs.github.io/
cargo install --locked yazi-fm yazi-cli
ya pkg add yazi-rs/flavors:catppuccin-frappe
cat <<'EOF' | tee -a ~/.bashrc > /dev/null
function y() {
	local tmp="$(mktemp -t "yazi-cwd.XXXXXX")" cwd
	yazi "$@" --cwd-file="$tmp"
	IFS= read -r -d '' cwd < "$tmp"
	[ -n "$cwd" ] && [ "$cwd" != "$PWD" ] && builtin cd -- "$cwd"
	rm -f -- "$tmp"
}
EOF

# 將 batcat 建立一個 symbolic link 為 bat，方便日後使用
ln -s /usr/bin/batcat ~/.local/bin/bat

# jq: https://github.com/jqlang/jq
curl -sL https://github.com/jqlang/jq/releases/latest/download/jq-linux64 -o ~/.local/bin/jq && chmod +x ~/.local/bin/jq

# yq: https://github.com/mikefarah/yq
curl -sL https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64 -o ~/.local/bin/yq && chmod +x ~/.local/bin/yq

# DotSlash: https://dotslash-cli.com/
curl -LSfs "https://github.com/facebook/dotslash/releases/latest/download/dotslash-ubuntu-22.04.$(uname -m).tar.gz" | tar fxz - -C ~/.local/bin

# 建立自用的執行檔目錄
mkdir -p ~/.local/bin
source ~/.profile
```

以下列出上述安裝工具的相關連結：

- [net-tools](https://sourceforge.net/projects/net-tools/)：經典的網路診斷與管理工具集
- [ripgrep](https://github.com/BurntSushi/ripgrep)：超高速的遞迴全文搜尋工具
- [jq](https://jqlang.org/)：輕量且彈性的命令列 JSON 處理器
- [lftp](https://lftp.yar.ru/)：支援多種協定的進階檔案傳輸程式
- [moreutils](https://github.com/pgdr/moreutils)：補齊傳統 UNIX 工具缺口的輔助工具集
- [btop](https://github.com/aristocratos/btop)：互動式系統資源監控元件
- [wslu](https://github.com/wslutilities/wslu)：Windows Subsystem for Linux 專用的實用工具集
- [yq](https://github.com/mikefarah/yq)：可處理 YAML⁄JSON⁄INI⁄XML 的命令列解析器
- [bat](https://github.com/sharkdp/bat)：一個具有語法高亮和 Git 整合的 `cat` 替代命令

### 設定 Node.js 環境

我會建議用 [nvm](https://github.com/nvm-sh/nvm) (Node Version Manager) 來管理 Node.js 的版本，這樣可以方便地切換不同的 Node.js 版本，真的可以減少很多麻煩事。

```sh
curl -s -o- https://raw.githubusercontent.com/nvm-sh/nvm/$(curl -s "https://api.github.com/repos/nvm-sh/nvm/releases/latest" | jq -r .tag_name)/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22
nvm alias default 22
node -v
```

### 設定 Bash 環境

```sh
# profile setup
cat <<'EOF' | tee -a ~/.profile
# 這是從 WSL 取得 Windows 使用者名稱最簡單的方法
export WINDOWS_USERNAME=$(powershell.exe '$env:UserName')

# 針對暗色背景終端機的明亮色彩配置
export JQ_COLORS="33:93:93:96:92:97:1;97:4;97"

export EDITOR=vim
export GPG_TTY=$(tty)
EOF

# bash setup
cat <<'EOF' | tee -a ~/.bashrc
# Enable programmable completion features
shopt -u direxpand
shopt -s no_empty_cmd_completion
EOF

# ssh setup
ssh-keygen -t rsa -b 4096 -f $HOME/.ssh/id_rsa -P ""
touch ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
# Remember add your SSH key to GitHub: $(cat ~/.ssh/id_rsa.pub)"

# setup workspace
mkdir -p ~/projects

# setup VS Code
code .
```

### 設定華麗的 Bash 提示符號

這裡我推薦用 [Starship](https://starship.rs/) 來當作為 Bash 的提示符號，這個工具非常輕量且功能強大，可以讓你的終端機看起來既美觀又實用。

```sh
# Install Starship
curl -sS https://starship.rs/install.sh | sh -s -- -y

# 參考設定 https://starship.rs/config/
mkdir -p ~/.config && touch ~/.config/starship.toml

# 安裝預設主題 https://starship.rs/presets/catppuccin-powerline
starship preset catppuccin-powerline -o ~/.config/starship.toml

cat <<'EOF' | tee -a ~/.bashrc > /dev/null
eval "$(starship init bash)"
EOF

source ~/.bashrc
```

### 安裝 [fzf](https://github.com/junegunn/fzf) 工具

這是一個非常強大的命令列模糊搜尋工具，可以讓你在終端機中快速找到檔案或目錄。

```sh
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install --all
source ~/.bashrc
```

💡 常用快速鍵：

1. 按下 `Ctrl-R` 可以快速搜尋歷史命令
2. 按下 `Ctrl-T` 可以快速搜尋檔案 (從當前資料夾開始找起)

### 設定 Git 環境

```sh
# git setup
npx -y @willh/git-setup --name 'Your Name' --email username@gmail.com
git config --global core.autocrlf input
git config --global init.defaultBranch main
```

> 💡 這個 [@willh/git-setup](https://www.npmjs.com/package/@willh/git-setup) 是我多年前開發的小工具，換新電腦的時候很實用，支援跨平臺自動設定 Git 常見參數。

如果您打算使用 Azure Repos 或 Azure DevOps，則需要進行一些額外的設定，以下命令可以讓你在 WSL 直接跟 Windows 共用同一個 [Git Credential Manager (GCM)](https://github.com/GitCredentialManager/git-credential-manager)：

```sh
git config --global credential.https://dev.azure.com.useHttpPath true
```

### 設定 Vim 編輯器

Vim 是一個非常強大的文字編輯器，雖然它的學習曲線有點陡峭，但一旦掌握了，就會發現它的效率非常高。

```sh
cat <<EOF | tee ~/.vimrc
syntax on
set background=dark

let &t_SI .= "\<Esc>[?2004h"
let &t_EI .= "\<Esc>[?2004l"

inoremap <special> <expr> <Esc>[200~ XTermPasteBegin()

function! XTermPasteBegin()
  set pastetoggle=<Esc>[201~
  set paste
  return ""
endfunction
EOF

# Copy vimrc to root
cat ~/.vimrc | sudo tee /root/.vimrc
```

### 安裝 [GitHub CLI](https://cli.github.com/)

> [Installing gh on Linux and BSD](https://github.com/cli/cli/blob/trunk/docs/install_linux.md)

```sh
(type -p wget >/dev/null || (sudo apt update && sudo apt-get install wget -y)) \
    && sudo mkdir -p -m 755 /etc/apt/keyrings \
        && out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
        && cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
    && sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
    && sudo mkdir -p -m 755 /etc/apt/sources.list.d \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
    && sudo apt update \
    && sudo apt install gh -y

gh help environment

gh auth login --web -h github.com
gh auth status

gh extension install github/gh-copilot
gh extension upgrade github/gh-copilot

gh copilot --help

echo 'eval "$(gh copilot alias -- bash)"' >> ~/.bashrc
source ~/.bashrc
```

```txt
github.com
  ✓ Logged in to github.com account doggy8088 (/home/will/.config/gh/hosts.yml)
  - Active account: true
  - Git operations protocol: https
  - Token: gho_************************************
  - Token scopes: 'gist', 'read:org', 'repo', 'workflow'
```

```sh
# 透過自然語言生成 CLI 指令建議 (ghcs = GitHub Copilot Suggest)
ghcs -h

# 透過自然語言生成 CLI 指令解釋 (ghce = GitHub Copilot Explain)
ghce -h
```

### 安裝 [AIChat](https://github.com/sigoden/aichat) 工具

[AIChat: All-in-one LLM CLI Tool](https://github.com/sigoden/aichat) 是一個多合一的 LLM CLI 工具，具有 Shell 助理、CMD & REPL 模式、RAG、AI 工具與代理等功能，讓你再也不必記憶指令。

以下這個命令會將 `aichat` 最新版下載並解壓縮到 `/usr/local/bin/aichat` 路徑下：

```sh
AIChatVersion=$(curl -s "https://api.github.com/repos/sigoden/aichat/releases/latest" | jq -r .tag_name)
sudo bash -c "
  curl -sL https://github.com/sigoden/aichat/releases/download/${AIChatVersion}/aichat-${AIChatVersion}-x86_64-unknown-linux-musl.tar.gz \
    | tar -xzO aichat > /usr/local/bin/aichat && chmod +x /usr/local/bin/aichat
"
aichat -V
```

設定使用 Gemini 來讓 [AIChat](https://github.com/sigoden/aichat) 回答問題，你可以從 <https://ai.dev/app/apikey> 取得免費金鑰：

```sh
cat <<'EOF' | tee -a ~/.profile
export GEMINI_API_KEY='YOUR_GEMINI_API_KEY'
export AICHAT_PLATFORM=gemini
# 追求高品質回應，可用 gemini-2.5-pro 模型
#export AICHAT_MODEL=gemini:gemini-2.5-pro
# 追求高效率回應，可用 gemini-2.5-flash-lite-preview-06-17 模型
export AICHAT_MODEL=gemini:gemini-2.5-flash-lite-preview-06-17
EOF
```

> 💡 完整設定可以參見 <https://github.com/sigoden/aichat/wiki/Environment-Variables>

第一次用 [AIChat](https://github.com/sigoden/aichat) 要先更新模型清單：

```sh
aichat --sync-models
```

### 安裝 [Codex CLI](https://github.com/openai/codex) 程式設計代理人工具

目前 Codex CLI 有兩個版本：

1. 一個是 Node.js 寫的，功能相對完整，但現已半放棄狀態，官方已不再維護。
2. 一個是 Rust 寫的版本，目前正積極開發中，功能陸續完善，是個明日之星。

以下是 Codex CLI 的 Node.js 版本的安裝方式：

```sh
# 安裝 Codex CLI (Node.js)
npm install -g @openai/codex

# 查看版本(codex-cli 0.5.0)
codex --version

# 使用 ChatGPT 帳號登入並取得 API Key
codex login
```

開啟畫面上的登入連結，走完 OAuth 授權流程後，將瀏覽器最後的網址透過以下命令在容器內執行：

```sh
curl -v -L 'http://localhost:1455/success?xxxxxxplatform.openai.com'
```

> 💡 目前 (2025-07-12) 這個 codex `0.5.0` 版本完全無法使用 Azure OpenAI Services 的端點。

如果要使用 Azure OpenAI Services 的端點與金鑰，必須參考 [OpenAI_Codex_CLI.md](OpenAI_Codex_CLI.md) 從原始碼開始建置 `codex` 程式才能用。但我有自己發佈一個可用的版本：

```sh
npm install -g @willh/codex
```

### 安裝 [Gemini CLI](https://github.com/google-gemini/gemini-cli/) 程式設計代理人工具

我有翻譯一份 [Gemini CLI 使用手冊](https://gemini-cli.gh.miniasp.com/)，可以參考看看。

```sh
npm install -g @google/gemini-cli && gemini -v

# 設定 Gemini CLI 登入
gemini
```

安裝 Gemini CLI 設定精靈

```sh
mkdir -p ~/.local/bin && curl -sSL https://github.com/doggy8088/gemini-init/raw/main/gemini-init -o ~/.local/bin/gemini-init && chmod +x ~/.local/bin/gemini-init
```

之後在任意資料夾就可以用以下命令快速初始化常用 Gemini CLI 設定：

```sh
gemini-init
```

> 此命令會寫入 `.gemini/settings.json` 設定檔。

### 安裝 [Claude Code](https://www.anthropic.com/claude-code) 程式設計代理人工具

```sh
npm install -g @anthropic-ai/claude-code

# 設定任務完成的鈴聲
claude config set --global preferredNotifChannel terminal_bell

# 設定暗黑佈景主題
claude config set -g theme dark

# CI 模式必須關閉 verbose
claude config set -g verbose false
```

### 安裝 [uv](https://docs.astral.sh/uv/getting-started/installation/) 工具 (Python)

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
uv -V
```

如果要執行一個 Python 程式，可以這樣執行：

```sh
uv run main.py
```

### 安裝 Google Cloud SDK

```sh
echo "Installing Google Cloud SDK..."
curl -sSL https://sdk.cloud.google.com | bash
source  ~/.bashrc
gcloud init
```

### 搞定網路問題

若要從 WSL 直接連接 Windows 主機的網路，有兩種方法：

1. **Windows 10** 的 WSL 2 僅支援 NAT 網路

    因為 WSL 是透過與 Windows 共用網路的方式，所以預設網路閘道是走虛擬網卡的 IP 地址，而每次 Windows 重新開機時，這個網址都會改變。所以取得 IP 的方法有點麻煩，有以下 3 種，都有點小麻煩，前兩個要記比較長的命令，最後一個雖然有域名，但也不是非常直覺：

    透過 `ip route show default` 取得預設閘道的網路資訊，再透過 `awk` 擷取 IP 部分：

    ```sh
    ip route show default | awk '{print $3}'
    ```

    透過 `/etc/resolv.conf` 記錄的 DNS IP 位址，預設就是與 Windows 共用的那個網路介面，再透過 `awk` 擷取 IP 部分：

    ```sh
    grep -m1 nameserver /etc/resolv.conf | awk '{print $2}'
    ```

    WSL 安裝時會自動為 Windows 建立 `<電腦名稱>.local` 的 mDNS 條目(Multicast DNS)，可直接透過 mDNS 連線，但有些人不太會記得自己的電腦名稱：

    ```sh
    echo $(hostname).local
    ```

    所以我的解法是，登入時自動建立一個 `$local` 變數，日後透過該變數就可以快速取得 Windows 的 IP 地址：

    ```sh
    export local=$(ip route show default | awk '{print $3}')
    ```

    將該設定寫入到 `~/.bashrc` 檔案中，之後每次登入就不用煩惱了，但你要記得有個 `$local` 變數可以用：

    ```sh
    cat <<'EOF' | tee -a ~/.bashrc
    export local=$(ip route show default | awk '{print $3}')
    EOF
    ```

2. **Windows 11 22H2+** 的 WSL2 可啟用 Mirrored Networking 設定

    Microsoft 在 Windows 11 22H2 引入 Mirrored networking，WSL2 會「鏡像」所有 Windows 介面，讓你在 WSL 2 中直接用 `localhost` 就可以連接 Windows 網路，等於是完全共用網路空間！

    請在 Windows 建立或編輯 `%USERPROFILE%\.wslconfig` 檔案，加入以下設定：

    ```ini
    [wsl2]
    networkingMode=mirrored
    ```

    接著登出 WSL 2 並在 Windows 執行 `wsl --shutdown` 將 WSL 2 完整關閉，下次登入 WSL 就會啟用鏡像網路了！

    這個設定的好處就是可以完美支援 IPv6，未來也不需再查 IP 地址，而且與多數 VPN 或 Multicast 的相容性都有大幅提升！👍

### 其他手動安裝的注意事項

1. 安裝 Docker Engine for Linux

    可以參考 [如何移除 Docker Desktop 並在 Windows 與 WSL 2 改安裝 Docker Engine](https://blog.miniasp.com/post/2025/06/14/How-to-remove-Docker-Desktop-and-install-Docker-Engine-on-Windows-with-WSL-2)

## 相關連結

- The Will Will Web (保哥的部落格)
  - [使用 WSL 2 打造優質的多重 Linux 開發環境](https://blog.miniasp.com/post/2020/07/26/Multiple-Linux-Dev-Environment-build-on-WSL-2)
  - [關於 Linux 下 Bash 與 Zsh 啟動檔的載入順序研究](https://blog.miniasp.com/post/2021/07/26/Bash-and-Zsh-Initialization-Files)
- Microsoft Learn
  - [Advanced settings configuration in WSL](https://learn.microsoft.com/en-us/windows/wsl/wsl-config?WT.mc_id=DT-MVP-4015686)
  - [Accessing network applications with WSL](https://learn.microsoft.com/en-us/windows/wsl/networking?WT.mc_id=DT-MVP-4015686)
  - [Use systemd to manage Linux services with WSL](https://learn.microsoft.com/en-us/windows/wsl/systemd?WT.mc_id=DT-MVP-4015686)
  - [Working across Windows and Linux file systems](https://learn.microsoft.com/en-us/windows/wsl/filesystems?WT.mc_id=DT-MVP-4015686)
  - [File Permissions for WSL](https://learn.microsoft.com/en-us/windows/wsl/file-permissions?WT.mc_id=DT-MVP-4015686)
  - [Get started using Git on Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-git?WT.mc_id=DT-MVP-4015686)
  - [Get started using Visual Studio Code with Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode?WT.mc_id=DT-MVP-4015686)
