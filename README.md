# Docker Containers

Repository of containers I use.

## arch-latex

- Source: [Dockerfile](arch-latex/Dockerfile)
- Args:
  - `VARIANT`: `base`
  - `USERNAME`: `remote`
  - `PASSWORD`:`remote`
  - `USER_UID`: `1000`
  - `USER_GID`: `$USER_UID`
- Ports:
  - 22 (SSH)
- Volumes:
  - `/code`

Container for LaTeX projects that can be remote controlled via SSH (also works with VSCode). You can mount your project in `/code` and use the [`texlive-localmanager`](https://gitlab.archlinux.org/remy/texlive-localmanager) aka `tllocalmgr` to manage CTAN packages.
