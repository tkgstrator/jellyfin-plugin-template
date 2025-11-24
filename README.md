## Jellyfin Plugin Development Template

### Requirements

- [Docker](https://www.docker.com/)
- [VSCode](https://code.visualstudio.com/)

Building the plugin requires an Ubuntu Docker image. Since this image is quite large, it is strongly recommended to pull it on your host machine beforehand.

```zsh
docker pull catthehacker/ubuntu:full-24.04
```

If you need a `GITHUB_TOKEN`, you can log in using GitHub CLI beforehand, which allows you to pass the `GITHUB_TOKEN` when running GitHub Actions locally.

```zsh
gh auth login
```

You can verify that the token is available by running `gh auth token`.

### Initialize Project

```zsh
cd Jellyfin.Plugin.Template
```

Now add the Jellyfin shared libraries.

```zsh
dotnet add package Jellyfin.Model
dotnet add package Jellyfin.Controller
```

### Customize Plugin Information

You need to populate some of your plugin's information. Go ahead a put in a string of the Name you've overridden name, and generate a GUID.

```zsh
sed -i "s/^guid: \".*\"/guid: \"$(od -x /dev/urandom | head -n1 | awk '{OFS="-"; srand($6); sub(/./,"4",$5); sub(/./,substr("89ab",1+rand()*4,1),$6); print $2$3,$4,$5,$6,$7$8$9}')\"/" /home/vscode/app/build.yaml
```

### Development

A Jellyfin server is running at [localhost:8096](http://localhost:8096), so you can test your plugin there.

#### Publish

Use `act` to build the plugin and automatically copy the built files to `config/plugins`.

### Build Plugin

You can verify the build using one of the following two methods.

#### Act (Recommended)

Emulates the GitHub Actions environment and outputs build artifacts to `artifacts`.

```zsh
act -W .github/workflows/publish.yaml --artifact-server-path artifacts
```

#### dotnet

Uses the `dotnet` command and outputs build artifacts to `Jellyfin.Plugin.Template/bin/Debug/net9.0/*`.

```zsh
dotnet build /property:GenerateFullPaths=true /consoleloggerparameters:NoSummary
```
