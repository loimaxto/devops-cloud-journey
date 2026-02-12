### Instruction
1. Install devpod, add provider of devpod
2. Add dev container config
3. Run command `devpod up https://github.com/loft-sh/devpod`


Command
```bash
devpod ssh # select run in remote container
cat /etc/os-release

devpod up https://github.com/loft-sh/devpod
devpod up . --idea none --dotfiles git@github.com:loimaxt/dotfiles-dev --recreate


```


Dockerfile file of dev container:
```
FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

COPY --from=jdxcode/mise /usr/local/bin/mise /usr/local/bin/

# make sure mise is activated in both zsh and bash. Might be overridden by a user's dotfiles.
RUN echo 'eval "$(mise activate bash)"' >> /home/vscode/.bashrc && \
    echo 'eval "$(mise activate zsh)"' >> /home/vscode/.zshrc
```


```


#### mise tools download
```yaml
neovim
ripgrep
lsd
fzf
fd
node
chezmoi
usage # for mise completion
```