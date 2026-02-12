
##### Import bashrc config

```bash

[ -f ~/.common_shell_rc ]() && source ~/.common_shell_rc

```

#### Apply zsh
```bash
sudo chsh -s $(which zsh) $USER
```


### Install startship zsh
```bash
curl -sS https://starship.rs/install.sh | sh

# ~/.bashrc
eval "$(starship init bash)"
# ~/.zshrc
eval "$(starship init zsh)"
```

setup modern UI: https://starship.rs/
#### Using bash in zsh
```bash
emulate bash
```
##### Extention
- auto complete zsh
- hightlight

