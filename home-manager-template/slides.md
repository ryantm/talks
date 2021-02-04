---
title: home-manager template
author: RyanTM
patat:
  wrap: true
  pandocExtensions:
    - patat_extensions
    - pipe_tables
...

# home-manager template

https://github.com/ryantm/home-manager-template

---

# Why home-manager?

* Reproduce your config instantly on new computers
* Leverage common configuration ecosystem (less duplicate work)

---

# What's home-manager?

* Organize $HOME with NixOS-like configuration
* home.nix ≅ configuration.nix
* `home-manager switch` ≅ `nixos-rebuild switch`
* Does not require NixOS (works on Linux)

---

# Who is home-manager template for?

* Allergic to installation instructions
* Don't like state
* Maximum reproducibility!

---

# What's home-manager template?

A template file structure, and a Nix shell where home-manager is
installed with all dependencies are pinned.

---

# Seven step program

1. Install nix `curl -L https://nixos.org/nix/install | sh`
2. Go to https://github.com/ryantm/home-manager-template
3. Click the "Use this template" button on GitHub
4. Clone your repository onto the computer you want to configure
5. Update dependencies  `./update-dependencies.sh`
6. Edit `home.nix`
7. Switch `./switch.sh`

---

# Example: home.nix

```nix
{ pkgs, ... }: {
  home.username = "ryantm";
  home.homeDirectory = "/home/ryantm";
  home.stateVersion = "20.09";
  programs.bash.enable = true;
  home.packages = with pkgs; [ cowsay ];
}
```

---

# Example

```console
$ ./switch.sh
...
$ cowsay "hello nixcon 2020"
 ___________________
< hello nixcon 2020 >
 -------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

```

---

# Questions

Thank you to all the organizers and attendees.

|             |                                                                             |
|-------------+-----------------------------------------------------------------------------|
|template     | https://github.com/ryantm/home-manager-template                             |
|home-manager | https://github.com/nix-community/home-manager                               |
|manual       | https://rycee.gitlab.io/home-manager/                                       |
|             |                                                                             |
|slides       | https://github.com/ryantm/talks/blob/master/home-manager-template/slides.md |
|email        | ryan@ryantm.com                                                             |
