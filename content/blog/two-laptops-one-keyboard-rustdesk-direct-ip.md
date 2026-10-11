+++
title = "Two Laptops, One Keyboard: RustDesk Direct IP"
date = 2026-10-11
description = "Connecting a Windows laptop to an Ubuntu laptop on the same network with RustDesk Direct IP Access: no account, no server in between."
# Uncomment if your config.toml defines a "tags" taxonomy:
# [taxonomies]
# tags = ["rustdesk", "ubuntu", "windows", "remote-desktop"]
+++

RustDesk can connect two machines on the same network by IP address alone, with no account and no server in between. I set it up between my Windows laptop and my Ubuntu laptop in about five minutes, and it is the smoothest remote desktop I have used.

<!-- more -->

## Two laptops, one desk

I have two laptops on my desk: one runs Windows, the other runs Ubuntu. I did not want two keyboards, two mice, and a neck that turns left and right all day. I wanted to sit at the Windows machine and reach the Ubuntu one from a window.

Both laptops are on the same Wi-Fi, a metre apart. Most remote desktop tools still want an account, a cloud relay, or a server in the middle before they let two neighbours talk.

Then I found [RustDesk](https://rustdesk.com), an open-source remote desktop app. It has one setting that changed everything for me: Direct IP Access.

## Why direct IP

Normally RustDesk works like the others: each machine gets an ID, and a rendezvous server introduces the two sides. That is the right design when the machines are in different cities.

On the same network it is extra distance. With Direct IP Access, the Windows laptop connects straight to the Ubuntu laptop's local address. Nothing leaves my network, there is no account to create, and it keeps working when the internet is down.

## The setup

You need RustDesk installed on both laptops, and both on the same Wi-Fi or LAN. First, get the Ubuntu laptop's IP address:

```bash
hostname -I
```

Mine was `192.168.1.17`. Yours will differ.

### 1. Enable Direct IP Access on Ubuntu

1. Open RustDesk on the Ubuntu laptop.
2. Click the menu (`≡`) in the top-right corner and choose **Settings**.
3. Go to **Security**, click **Unlock Security Settings**, and enter your Ubuntu password.
4. Tick **Enable Direct IP Access**.

### 2. Open the firewall port on Ubuntu

Direct IP Access listens on TCP port 21118 by default. If `ufw` is active, let it through:

```bash
sudo ufw allow 21118/tcp
```

Many guides open the whole 21115–21119 range plus UDP 21116. Those are the ports of a self-hosted RustDesk server. For a direct connection between two clients, 21118 is the one that matters.

### 3. Connect from Windows

1. Open RustDesk on Windows.
2. In the **Control Remote Desktop** field, type the Ubuntu IP and the port:

   ```text
   192.168.1.17:21118
   ```

3. Click **Connect** and enter the password set in RustDesk on the Ubuntu laptop.

That is the whole setup. The Ubuntu desktop opens in a window on Windows.

## If the screen stays black

Ubuntu uses Wayland by default, and Wayland is strict about screen sharing. If you get a black screen, a frozen picture, or a permission prompt that keeps coming back, switch the session to Xorg:

1. Log out of Ubuntu.
2. On the login screen, click your username, then the gear icon in the bottom-right corner.
3. Choose **Ubuntu on Xorg** and log back in.

## Verdict

Five minutes, one checkbox, one firewall rule. The connection is direct, so it feels close to sitting at the Ubuntu laptop itself.

Two things to remember. A laptop's IP can change when the router hands out a new address, so reserve one for it in the router's DHCP settings. And Direct IP Access is for networks you trust: leave it off on café or hotel Wi-Fi, and keep a strong RustDesk password either way.

If you have two machines on one desk, try it before you buy a second keyboard.
