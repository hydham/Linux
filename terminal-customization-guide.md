# Terminal Customization Guide (macOS, Linux, Windows)

Goal prompt:

```
username at hostname in directory
$ 
```

---

## macOS (Zsh – `.zshrc`)

1) Edit:
```bash
nano ~/.zshrc
```

2) Add:
```zsh
# Colors
orange=$(tput setaf 208)   # username
blue=$(tput setaf 228)     # host
green=$(tput setaf 71)     # directory
white=$(tput setaf 15)
bold=$(tput bold)
reset=$(tput sgr0)

# Prompt (multiline)
PS1="%{$bold%}"
PS1+="%{$orange%}%n"
PS1+="%{$white%} at "
PS1+="%{$blue%}%m"
PS1+="%{$white%} in "
PS1+="%{$green%}%1~"
PS1+="
%{$white%}\$ %{$reset%}"

export PS1
```

3) Reload:
```bash
source ~/.zshrc
```

---

## Linux (Bash – `.bashrc` only)

Bash needs `\[` and `\]` around color codes so the cursor doesn’t break.

1) Edit:
```bash
nano ~/.bashrc
```

2) Add:
```bash
# Colors
orange=$(tput setaf 208)   # username
blue=$(tput setaf 228)     # host
green=$(tput setaf 71)     # directory
white=$(tput setaf 15)
bold=$(tput bold)
reset=$(tput sgr0)

# Prompt (multiline)
PS1="\[${bold}\]"
PS1+="\[${orange}\]\u"
PS1+="\[${white}\] at "
PS1+="\[${blue}\]\h"
PS1+="\[${white}\] in "
PS1+="\[${green}\]\W"
PS1+="\n\[${white}\]\$ \[${reset}\]"
export PS1
```

3) Reload:
```bash
source ~/.bashrc
```

---

## Windows (PowerShell)

1) Open PowerShell  
2) Edit your profile:
```powershell
notepad $PROFILE
```

3) Add:
```powershell
function prompt {
    $user = $env:USERNAME
    $hostName = $env:COMPUTERNAME
    $dir = Split-Path -Leaf (Get-Location)

    Write-Host "$user at $hostName in $dir" -ForegroundColor Yellow
    return "`n$ "
}
```

4) Restart PowerShell

---
