---
layout: post
title:  "Installing Bitwarden via AppImage on a KDE Distro"
date:   2021-02-21 19:10:01 -0800
categories: linux security appimage kde bitwarden 
---

## Background

I've been wanting to try the password manager [Bitwarden](https://bitwarden.com/) for some time now, and the recently announced [changes](http://blog.lastpass.com/2021/02/changes-to-lastpass-free/) to the free edition of LastPass was just the push I needed to give Bitwarden an [earnest](https://upload.wikimedia.org/wikipedia/en/0/08/Ernest_P._Worrell.jpg) try.

I can report that the process of exporting my data from the LastPass browser extension to csv, then importing it into Bitwarden was easy and painless. While the steps were self-explanatory, I do appreciate that Bitwarden has a reference [document](https://bitwarden.com/help/article/import-from-lastpass/) for the process. I exported from the LastPass browser extension and imported into the Bitwarden web interface.

I really appreciate that while Bitwarden provides the usual iOS, Android, Windows, and macOS binaries, they also provide a cross-distro compatible Linux package in the form of an [AppImage](https://appimage.org/). While the [Snap](https://snapcraft.io/) vs [Flatpak](https://flatpak.org/) debate seems to get the most attention, it is nice to see AppImage getting some love.

## Installation

Disclaimer: this is not the only way to handle installation, and may not be the best method, depending on your needs, but this is how I like to do it.

This method installs into your home directory, so we assume that no other users on your system will need access to Bitwarden. Of course, it would be trivial to modify these steps slightly to install to some centrally accessible location like `/usr/local/bin` provided that you have sudo rights on the machine.

```
# Create bin directory in your home:
mkdir ~/bin
cd ~/bin

# Get latest AppImage: 
wget "https://vault.bitwarden.com/download/?app=desktop&platform=linux"

# Make the AppImage executable:
chmod 555 Bitwarden*.AppImage

# For added flexibility, create symlink pointing to AppImage:
ln -s Bitwarden*.AppImage bitwarden 
```

Next, I like to integrate the application into my KDE menu. This should work regardless of the underlying distro, as long as your desktop environment is KDE (and why wouldn't it be?). 

Right-click on the KDE menu, click "New Item", type the name "Bitwarden", and click "ok". On the left hand side, drag the Bitwarden menu item into the category that makes the most sense to you - I usually put it in the "Office" category. On the right-hand side: Optionally add a description and comment, but definitely modify the "Command" field to point to the path where you put the AppImage. Hint: use the symlink you created earlier: `~/bin/bitwarden`. That way when you get a newer version of the AppImage, you can simply re-point your symlink and not need to also update your KDE menu entry. You can also add an icon by clicking the empty square in the KDE Menu Editor (right-hand side). Usually I search within system icons for "lock" and just use a generic padlock icon. When done, click "save" to close out of the Menu Editor. 

I have heard there is an AppImage daemon [appimaged](https://github.com/probonopd/go-appimage) as well as a Launcher project [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher), either of which would probably help with the automation of integrating AppImage apps into GUI menus, and also might add some automation around updating the AppImages themselves. I should probably look into these if I start using AppImages on a larger scale. If I do, I'll try to come back and edit this post.

It doesn't much matter where you put the AppImage and whether or not you integrate it into the menu of your desktop environment of choice. As far as I can tell, Bitwarden will store local data in your home directory, as a well-behaved desktop application should, in `~/.config/Bitwarden`.

Finally, just a friendly reminder to ensure you have configured two factor [authentication](https://bitwarden.com/help/article/setup-two-step-login/).

Happy password managing!