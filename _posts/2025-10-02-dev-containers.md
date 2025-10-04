---
layout: post
title: "Using Dev Containers in VS Code"

tags: [learning, dev]
---

I am working with an open-source library called GICI. The authors explicitly said that if not familiar with cross-platform development, build the program in Ubuntu 22 which is the system that they used. My host PC is running Ubuntu 24. There are couple of ways to address the problem. 
- dual boot
- install Ubuntu 22 using virtual box.
- use a separate PC
None of the above is appealing. Fortunately, I happen to come across Dev Containers in VS Code. Essentially, what's happening is you are running a specific system configuration, including the operating system (OS), inside a bubble. Or as they call it, **the container**. What this means is you can easily develop in different platforms by just configuring a container for each platform and using only one PC. I tried to remember the steps I took, and I will outline them here. I am using VS Code. 

### 1. Install Dev Containers
You can do this by going to extensions, and then search for Dev Containers. It looks like the image below. Click install. 
<img align="center" width="954" height="188" alt="image" src="https://github.com/user-attachments/assets/28b0e565-9b3f-4f6c-9b7a-1c95386028de" />

### 2. Create the `.devcontainer/devcontainer.json`
This is my first time doing this and I asked Claude to do it for me. But for more brain activity, I'll try to write a separate blog about the contents of `devcontainer.json`. 
``` code
{
    "name": "GICI-LIB Ubuntu 22.04",
    "build": {
        "dockerfile": "../Dockerfile",
        "context": ".."
    },
    "workspaceMount": "source=${localWorkspaceFolder},target=/workspace,type=bind",
    "workspaceFolder": "/workspace",
    "customizations": {
        "vscode": {
            "extensions": [
                "ms-vscode.cpptools",
                "ms-vscode.cmake-tools"
            ]
        }
    }
}
```
### 3. Open the container
Press `Ctrl+Shift+P` to open the Command Prompt and type `Dev Containers: Reopen in Container`. You should see it like this. Click that 
<img align="center" width="605" height="98" alt="image" src="https://github.com/user-attachments/assets/3f7ef972-5c32-404c-b37e-3a381f99ba1c" />

I asked Claude what happens we select that commands. It does couple of things:
- Reads the `.devcontainer.json` which is essentially the config of this container. This includes the Dockerfile, the workspace you are running the container, priviledges to I/O like serial devices.
- Builds the image based on the Dockerfile if not yet build. This could take long to build depending on the commands you set in the Dockerfile. This is only done once. But if you change anything in your Dockerfile, you need to build the image again so it could take effect. Otherwise, it will just open the container based on the built image.

### 4. Verification
If successfull, you can see that you are running using the container. You could verify this by looking at the leftmost low corner. In my case it is shown like this:
<img align="center" width="289" height="99" alt="image" src="https://github.com/user-attachments/assets/c5687619-ed2c-421d-ba06-0521a07ad9ae" />

The name was GICI-LIB (the program I will modify and develop) Ubuntu 22.04 (the OS environment). If I run `cat /etc/os-release`, it will show 
<img align="center" width="727" height="211" alt="image" src="https://github.com/user-attachments/assets/3dc1d2f0-073b-4290-93da-5a76f63e4ce2" />
which shows the Ubuntu 22 info (Jammy Jellyfish). 

Laters!

