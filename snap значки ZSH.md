# The Fix

It looks like there are two scripts that are sourced by Bash by default that export environment variables which tell Wayland and Flatpak where to find and load the GUI applications.

Here’s how to restore them to your Zsh environment:

1.  **sudo edit your zprofile:**

> sudo (vim, nano, vi) /etc/zsh/zprofile

**2\. Add these two lines to the end of the profile to ensure Zsh is sourcing the ENV vars that lets wayland etc. find your snap apps:**

> emulate sh -c ‘source /etc/profile.d/apps-bin-path.sh’
> emulate sh -c ‘source /etc/profile.d/flatpak.sh’

**3\. save your edited zprofile**

**4\. reboot your machine**