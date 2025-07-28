---
title: "Dev ContainerでのSSHエージェントフォワード"
date_created: 2025-07-26
tags:
  - devcontainer
  - docker
  - wsl
  - ssh
---

# 概要

WSL 上で VS Code Dev Container を利用し、ホストの SSH キーおよび SSH エージェントをコンテナ内に自動でマウントフォワードする方法をまとめました。  

# 詳細

- **WSL 側での ssh-agent 自動起動・鍵登録**  
  - `~/.bashrc` に起動／再利用スクリプトを追加して、ターミナル起動時に `ssh-agent` → `ssh-add` を自動実行  
  - もしくは `keychain` を使って一度パスフレーズを入れれば以降自動再利用  

- **Dev Container 設定 (`.devcontainer/devcontainer.json`)**  
  ```jsonc
  "mounts": [
    "source=${localEnv:HOME}/.ssh,target=/home/vscode/.ssh,type=bind,consistency=cached"
  ],
  "runArgs": [
    "-v", "${localEnv:SSH_AUTH_SOCK}:/ssh-agent"
  ],
  "remoteEnv": {
    "SSH_AUTH_SOCK": "/ssh-agent"
  }
