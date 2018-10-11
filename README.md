# dockerfiles

Real Dockerfiles I really use, with X11 magic cookie and Pulse Audio support.  (Except Discord, it's broken.)  See notes at the top of the Dockerfiles or [my Docker aliases](https://github.com/Lizards/dotfiles/blob/master/.docker_aliases) for usage.

Sending requests to `xdg-open` on the host machine from within the container (e.g. clicking a link in Slack) can be enabled using the `xdg-open-server` utility ([source](https://github.com/kitsunyan/xdg-open-server), [AUR](https://aur.archlinux.org/packages/xdg-open-server/)).


Build all:
```console
$ ./build.sh
```

Build one or some:
```console
$ ./build.sh filezilla spotify
```
