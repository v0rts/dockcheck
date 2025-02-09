# dockcheck
### A script checking updates for docker images **without pulling** - then selectively auto-update some/all containers.  

### :warning: URGENT! 
The 2.0 change had a breaking error - compose-recreation might have included previous containers compose-file.
If you've had odd errors, you can use the [errorCheck.sh](https://github.com/mag37/dockcheck/blob/main/errorCheck.sh) script to list current running container configs in a readable way. Look especially for **Compose files** listed in wrong places.   
Recreate the suspicious container(s) manually with `docker compose down && docker compose up -d`.

### :pushpin: Recent changes:
- **v0.2.2**: Fixed breaking errors with multi-compose, odd breakage and working dir error.
- **v0.2.1**: Added option to exclude a list of containers.
- **v0.2.1**: Added multi-compose support (eg. override). 
- **v0.2.0**: Fixed error with container:tag definition. 
- **v0.1.9:** Fixed custom env-support. 
- **v0.1.8:** Added option to prune dangling images. 
___

## Dependencies:
Running docker (duh) and compose, either standalone or plugin.   
[`regclient/regctl`](https://github.com/regclient/regclient) (Licensed under [Apache-2.0 License](http://www.apache.org/licenses/LICENSE-2.0))   
User will be prompted to download `regctl` if not in `PATH` or `PWD`
___


![](https://github.com/mag37/dockcheck/blob/main/example.gif)

## `dockcheck.sh`
```
$ ./dockcheck.sh -h
Syntax:     dockcheck.sh [OPTION] [part of name to filter]
Example:    dockcheck.sh -a -e nextcloud,heimdall

Options:
-h     Print this Help.
-a|y   Automatic updates, without interaction.
-n     No updates, only checking availability.
-p     Auto-Prune dangling images after update.
-e     Exclude containers, separated by comma.
-r     Allow updating images for docker run, wont update the container.
```

Basic example:
```
$ ./dockcheck.sh
. . .
Containers on latest version:
glances
homer

Containers with updates available:
1) adguardhome
2) syncthing
3) whoogle-search


Choose what containers to update:
Enter number(s) separated by comma, [a] for all - [q] to quit:

```
Then it proceedes to run `pull` and `up -d` on every container with updates.   
After the updates are complete, you'll get prompted if you'd like to prune dangling images.

### `-r flag` :warning: disclaimer and warning:
**Wont auto-update the containers, only their images. (compose is recommended)**   
`docker run` dont support using new images just by restarting a container.  
Containers need to be manually stopped, removed and created again to run on the new image.

### :hammer: Known issues
- No detailed error feedback (just skip + list what's skipped) .
- Not respecting `--profile` options when re-creating the container.

## `dc_brief.sh`
Just a brief, slimmed down version of the script to only print what containers got updates, no updates or errors.

# License
dockcheck is created and released under the [GNU GPL v3.0](https://www.gnu.org/licenses/gpl-3.0-standalone.html) license.
___

## Check out a spinoff brother-project:
### [Palleri/dockcheck-web](https://github.com/Palleri/dockcheck-web) for a WebUI-front!

## Special Thanks:
- :bison: [t0rnis](https://github.com/t0rnis)   
- :leopard: [Palleri](https://github.com/Palleri)
