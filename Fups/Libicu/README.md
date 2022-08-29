# libicu*.so.70
Recently, the Atom text editor has been discontinued and will no longer receive support. It is my favourite editor, so I was quite saddened when a recent system update upgraded dependent packages, crashing Atom.

The solution I've come up with was to search for the offending file (given by the error produced by Atom) in pacman:

```
sudo pacman -F <file to look for>
```

This will reveal which packages contain the needed files. I then installed the resulting packages, moved the libraries I needed into /lib64, then (optionally) uninstalled the packages.
