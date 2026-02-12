## Instruction
1. chezmoi dotfile manager: https://www.loom.com/share/438fc4038cfe4fe2b568a34e1a3189b6
2. automic setup dotfiles chezmoi: https://www.loom.com/share/b3fb16f5e02d422bb4d67d333a6014cb

### Chezmoi

Download
```bash
sh -c "$(curl -fsLS get.chezmoi.io)"

# public git 
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply $GITHUB_USERNAME

# private git
sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply git@github.com:$GITHUB_USERNAME/dotfiles.git

cd ~/.local/share/chezmoi
chezmoi cd
chezmoi diff
chezmoi apply

```

#### Setup
```bash
#!/bin/bash
set -euo pipefail

if ! command -v chezmoi > /dev/null; then
	sh -c "$(curl -fsLS get.chezmoi.io)" -- init --apply git@github.com:loimaxto/dotfiles.git
fi
exit 0
```

Link: https://www.chezmoi.io/quick-start/#start-using-chezmoi-on-your-current-machine

#### Command
```bash
# modify dotfile in chezmoi( not real system )
chezmoi cd
chezmoi apply 
```
### Mise

```txt
FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

COPY --from=jdxcode/mise /usr/local/bin/mise /usr/local/bin/

# make sure mise is activated in both zsh and bash. Might be overridden by a user's dotfiles.
RUN echo 'eval "$(mise activate bash)"' >> /home/vscode/.bashrc && \
    echo 'eval "$(mise activate zsh)"' >> /home/vscode/.zshrc
```