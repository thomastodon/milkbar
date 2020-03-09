## OS X

### setup

install things
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew cask install sabnzbd sonarr plex-media-server
brew install mono
curl -L https://github.com/Radarr/Radarr/releases/download/v0.2.0.1358/Radarr.develop.0.2.0.1358.osx.tar.gz | tar -zx
```

start all the stuff
```
/Users/thomasshouler/Radarr/Radarr; exit;
mono --debug /Applications/Sonarr.app/Contents/MacOS/NzbDrone.exe
chmod +x /Applications/Sonarr.app/Contents/MacOS/Sonarr
/Applications/Sonarr.app/Contents/MacOS/Sonarr; exit;
```

access things

| service | url                 |
|---------|---------------------|
| plex    | milkbar.local:32400 |
| sonarr  | milkbar.local:8989  |
| radarr  | milkbar.local:7878  |
| sabnzbd | milkbar.local:8080  |
| nzbgeek | nzbgeek.info        |
