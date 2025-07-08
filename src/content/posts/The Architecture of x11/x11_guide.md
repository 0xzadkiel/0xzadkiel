---
title: "The Architecture of X11"
published: 2025-07-08
image: "./124.gif"
tags: ["General"]
category: "Guide"
draft: false
---

# The Architecture of X11

The `X Window System` (commonly referred to as `X11`) is a widely used display server that enables graphical applications in Unix-like operating systems. It acts as a `middleman` between software (apps) and hardware (GPU, keyboard, mouse), providing the infrastructure to create and manage graphical interfaces.

> **Note**: Wayland is the modern replacement for X11.


### Core Components

- **X Server**: The program (`Xorg`) that manages hardware (input/output) and renders graphics.
- **X11 Protocol**: Defines how clients and servers communicate.
- **X Client**: Requests display services from the X Server (e.g., `xterm`, `gedit`).


### Startup Process

1. **`systemd`** → Starts the display manager (e.g., `gdm`, `lightdm`).
2. **Display Manager** → Launches the **X Server**.
3. **X Server** → Takes control of the GPU (for rendering), keyboard, mouse, and monitors.

---

# How X11 Works

The X Server uses `Unix sockets` for local communication and `TCP ports (6000–6009)` for network access. Unlike `VNC` and `RDP` (which transmit full-screen images), X11 sends `rendering instructions`, making it faster and more efficient.

- The first display (`:0`) uses `port 6000`, the second (`:1`) uses `6001`, and so on. This allows multiple users to run separate GUI sessions on the same machine.

 **Process Flow**:

1. Clients connect to the X Server and send drawing requests (e.g., "move this window").
2. The X Server renders the final image and displays it.

**Security Considerations**
By default, X11 transmits data `unencrypted`, but it can be secured using protocols like `SSH`.

---

# Why Use X11?

1. `Less Data Transfer`: Only sends drawing commands (not full screens).
2. `Efficient`: Lower resource usage compared to other protocols.
3. `Native to Linux/Unix`: No additional software needed.

---

# How X11 Forwarding Works

When you connect via SSH with `-X` or `-Y`:

1. SSH creates a **Unix socket**(e.g., `:10`) and sets the `DISPLAY` environment variable to it.
2. The remote application connects to this socket, believing it's a real X Server.
3. Traffic is encrypted and tunneled over SSH (port 22).
4. Your local SSH client forwards it to your **actual X Server** (`:0`).
5. The final output is rendered on your screen.

Here's a Diagram : 
![Cover image2](/images/Diagram3.png)


---

# Trusted vs. Untrusted Mode


### 1. Untrusted Mode (`-X`)
When we use `-X` flag the untrusted mode gets enabled and it applies some security restrictions such as 
+ It blocks certain x11 extenshions .
+ It sandboxes the running application
+ Prevents apps from accessing other windows and input devices freely

To enable this on server side we have to set the `ForwardX11Trusted` Variable to `no` in `/etc/ssh/sshd_config`

**Issues**
Some graphical apps may run slower due to these restrictions 


### 2. Trusted Mode (`-Y`)
This mode can be enabled using the `-Y` flag with ssh . Now Applications will run without any security restrictions . All x11 extensions are allowed and is equivalent to running the app locally on your machine resulting in better performance

To enable this , on the server side we have to set the `ForwardX11Trusted` Variable to `YES` in `/etc/ssh/sshd_config`

 **Risk**: Malicious apps can keylog/screenshot (use only on trusted hosts).



**Example** : 
```bash
alchemist@lea:~$ ssh -X debian@192.168.0.154 /usr/bin/firefox
```
![Cover image1](/images/x11.png)

