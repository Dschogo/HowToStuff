# ES2DR - Epidemic Sound to DaVinci Resolve

## What is ES2DR?

Its a simple tool (Direfox Extension + Python middle man) that allows you to import Epidemic Sound tracks direclty into DaVinci Resolve + saves them into a folder of your choice.

!!!info WORK IN PROGRESS
    This is heavily work in progress, so expect bugs and issues and missing features.

## How to install it?

Download the latest release from the [releases page](https://github.com/Dschogo/ES2DR/releases).
You only need the `ES2DR.xdi` for now.
Simply drag and drop it onto your browser and it will prompt you to install it.

Now download the `server.zip` from the same page and extract it somewhere.

!!!info
    Required is Python3 and the `websockets` module. You can install it with `pip install websockets`.

Before running the server, you need to edit the `ws.py` file. (Will be changed in the future).
Open the file and change the `path` variable to the path of the desired folder where the tracks should be saved. (Like an audio database - the files will remain after closing the script/davinci/project)

Now you can run the `ws.py` file. Its still a lillte buggy, maybe you have to go to the Firefox extention settings and restart it, to make it connect. (Or click a few times the extention icon to start downloading, it should try to reconnect and start downloading/importing)

## How to use it?

Go to any Track page or playlist/album page on Epidemic Sound and click the EStDR icon in your browser. IT will download all Tracks into the folder and imports them into the mediapool.

!!!info
    You can also rightlick a track link (the name), to only download that one.

## Planned features

- [ ] some sort settings - ui? - in the browser/external window?
- [ ] directly import into timeline?
- [ ] Better docs -> short gif?
- [ ] Better error handling
- [ ] Auto updates
