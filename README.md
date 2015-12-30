# logwatch
A simple python logwatcher ....


## Installation

The python dependencies can be installed globally with the command:
```
sudo pip install -r requirements.txt
```

You will need pip installed before attempting this, on Debian / Ubuntu the following should be run first:

```
sudo apt-get install python-pip
```

Quite convuloted, will need to revisit this, this is not production ready code, just a proof of concept. Feel free to use and modify as you please, copyright gnu/gplv3


## How it Works

Basic concept is this:


Read all files in a given directory and constantly monitor that directory for new files --> pass this off to a file scanner to read in the files as they come in and to read the last line of the file (I am having some problems with doing a tail due to the fact that it needs to seek to the bottom of the file and then track that location, while trying to track new files).

Each time a file is scanned its forked off using the threads library so that its processed seperately and so that we can have multiple files open at the same time, any time any of the conditions are matched as set in the logwatch.yaml then this should trigger a write to the log file for logwatcher.

Each thread is daemonized and the main process itself is also daemonized, each thread in turn has 5 seconds to do its scans (it needs time to actually track the end of the file - its a poor mans find) then the threads are forcibly joined back to the main process where the whole folder structure is re-scanned again for new files which again kicks off the file scan process and the threads to scan the various files, during this time there are black outs. There has to be a better way of tracking changes to the end of the file, which I havent found yet (ive only spent around a day knocking this up), but if the end of the file can easily be tracked this script should just work even if pointed directly to /var/log and given any error condition or error message to track and display.





