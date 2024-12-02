# setup-wsl2

GitHub action to enable WSL2 and install a Linux distro in GitHub-hosted runners.

GitHub-hosted Windows runners come with WSL 1 by default. This action upgrades to WSL 2 and installs a Linux distro of your choice (Ubuntu by default).

It also enables systemd support if you are using `windows-2022`/`windows-latest` runner. See https://learn.microsoft.com/windows/wsl/systemd.

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
        shell: wsl-run {0} # add this to run the commands inside linux
      - run: |
          ls -al
          env
        shell: wsl-run {0} # don't forget to add this
        env:
          MY_ENV_VAR: MY_VALUE
          # WSLENV is a special environment variable to share environment variables between windows and linux
          # see https://devblogs.microsoft.com/commandline/share-environment-vars-between-wsl-and-windows
          WSLENV: MY_ENV_VAR
```

### Inputs

| Input            | Description                                                                                                                                             | Default  |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| `distro`         | The Linux distro to install. You can use any distro which is available in `wsl --list --online`.<br>**Note:** Use `none` to skip installing any distro. | `Ubuntu` |
| `enable-systemd` | Enable systemd.<br>**Note:** Please set it to `false` if you are using `windows-2019` runner.                                                           | `true`   |
