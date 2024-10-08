### Create container

(antigo)
```bash
SHARE=/home/$USER/Documentos/Mac

docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v "${SHARE}:/mnt/hostshare" \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e GENERATE_UNIQUE=true \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e MASTER_PLIST_URL='https://raw.githubusercontent.com/sickcodes/osx-serial-generator/master/config-custom-sonoma.plist' \
    -e RAM=8 \
    -e CORES=4 \
    sickcodes/docker-osx:sonoma
```

(novo)
```bash
SHARE=/home/$USER/Documentos/Mac

sudo docker run -it \
    --device /dev/kvm \
    -p 50922:10022 \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v "${SHARE}:/mnt/hostshare" \
    -e "DISPLAY=${DISPLAY:-:0.0}" \
    -e GENERATE_UNIQUE=true \
    -e CPU='Haswell-noTSX' \
    -e CPUID_FLAGS='kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on' \
    -e MASTER_PLIST_URL='https://raw.githubusercontent.com/sickcodes/osx-serial-generator/master/config-custom.plist' \
    -e RAM=8 \
    -e CORES=4 \
    seraphix/docker-osx:sonoma
```

##### Open Terminal inside macOS and run the following command to mount the virtual file system
```bash
sudo -S mount_9p hostshare
```
<br>

### Start container
```bash
docker start {container_id}
```

##### Create a alias
```bash
echo -e "\nalias macos='docker start {container_id}'" >> ~/.bash_aliases
```
So, to start the container type ```macos``` on terminal

<br>

### Optimizations

##### Disable spotlight indexing 
```bash
defaults write com.apple.loginwindow autoLoginUser -bool true
```

##### Enable performance mode 
```bash
# check if enabled (should contain `serverperfmode=1`)
nvram boot-args

# turn on
sudo nvram boot-args="serverperfmode=1 $(nvram boot-args 2>/dev/null | cut -f 2-)"

# turn off
sudo nvram boot-args="$(nvram boot-args 2>/dev/null | sed -e $'s/boot-args\t//;s/serverperfmode=1//')"
```

##### Disable heavy login screen wallpaper
```bash
sudo defaults write /Library/Preferences/com.apple.loginwindow DesktopPicture ""
```

##### Reduce Motion & Transparency
```bash
defaults write com.apple.Accessibility DifferentiateWithoutColor -int 1
defaults write com.apple.Accessibility ReduceMotionEnabled -int 1
defaults write com.apple.universalaccess reduceMotion -int 1
defaults write com.apple.universalaccess reduceTransparency -int 1
```

##### Disable screen locking
```bash
defaults write com.apple.loginwindow DisableScreenLock -bool true
```

##### Show a lighter username/password prompt instead of a list of all the users
```bash
defaults write /Library/Preferences/com.apple.loginwindow.plist SHOWFULLNAME -bool true
defaults write com.apple.loginwindow AllowList -string '*'
```

##### Disable saving the application state on shutdown
```bash
defaults write com.apple.loginwindow TALLogoutSavesState -bool false
```
