* Clone the repository to your local machine

```
git clone https://github.com/dynatrace/demo-netflow
```

* Open the folder in Visual Studio code
* Ensure the [Microsoft Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers){target=_blank} and [Dev Containers CLI](https://code.visualstudio.com/docs/devcontainers/devcontainer-cli#_installation){target=_blank} are installed in VSCode
* Open a new terminal in VSCode and set your environment variables as appropriate:

```
set DT_ENVIRONMENT_ID=abc12345
set DT_ENVIRONMENT_TYPE=live
set DT_API_TOKEN=dt0c01.******.***********
```

* Start Docker / Podman
* Create the environment

```
devcontainer up
```

It will take a few moments but you should see:

```
{"outcome":"success","containerId":"...","remoteUser":"root","remoteWorkspaceFolder":"/workspaces/obslab-jmeter"}
```

* Connect to the demo environment. This will launch a new Visual Studio Code window

```
devcontainer open
```

In the new Visual Studio code window, open a new terminal and continue with the tutorial.