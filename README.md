# setup-wsl2

GitHub action to enable WSL2 and install a Linux distro in GitHub-hosted runners.

GitHub-hosted Windows runners come with WSL 1 by default. This action upgrades to WSL 2 and installs a Linux distro of your choice (Ubuntu by default).

It also enables systemd support if you are using `windows-2022`/`windows-latest` runner. See https://learn.microsoft.com/windows/wsl/systemd.

### Inputs

| Input            | Description                                                                                      | Default  |
| ---------------- | ------------------------------------------------------------------------------------------------ | -------- |
| `distro`         | The Linux distro to install. You can use any distro which is available in `wsl --list --online`. | `Ubuntu` |
| `enable-systemd` | Enable systemd.<br>**Note:** Please set it to `false` if you are using `windows-2019` runner.                 | `true`   |

### Example Usage

```yaml
name: CI/CD
on:
  push:
    branches: [main]
jobs:
  build:
    # always windows. why would you use wsl on linux or macos 😂
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4
      - uses: vedantmgoyal9/setup-wsl2@main
      - run: apt update && apt upgrade -y
        # add this line to run the commands inside linux
        shell: wsl-run {0}
      - run: |
          ls -al
          curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
        shell: wsl-run {0} # don't forget to add this
```
