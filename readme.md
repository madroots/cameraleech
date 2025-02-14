# Cameraleech CCTV recorder [![Go Report Card](https://goreportcard.com/badge/github.com/phsm/cameraleech)](https://goreportcard.com/report/github.com/phsm/cameraleech)
Its a simple video recorder leveraging FFMPEG to retrieve video streams from surveillance cameras and store them on disk. 
In a nutshell, its nothing but a smart ffmpeg launcher

## Quick Installation
`curl -fsSL https://github.com/HDTS1/cameraleech/raw/master/compiled/install.sh | sh`

## Fork-specific changes:
- Easy to use install script
- Recordings in mp4 format (easy to play in browser)
- Minor fixes (fixed config name, fixed ffmpeg log level..)

Check original repo here: https://github.com/phsm/cameraleech

The idea behind of cameraleech is simple: read the config with camera names and URLs, launch ffmpeg process per camera, restart if it crashes and collect its statistics.
For convenience, records are stored in segments.

Each camera record is stored in "storagePath/_cameraname_/_YYYY-MM-DD_/_segment start time_.mkv"

Tested on Linux. Work on other OSes isn't guaranteed.

## Storage performance
By default Linux stores written data in so-called "dirty pages" for 30 seconds before forcibly committing them on disk. You can tune dirty pages writeback behavior to keep them in RAM little more in order to accumulate writes. There are 2 sysctl parameters you can tune:
- `sysctl -w vm.dirty_ratio=80` - percentage of your RAM which can be left unwritten to disk.
- `sysctl -w vm.dirty_background_ratio=50` - percentage of yout RAM when background writer have to kick in and start writes to disk. Make it way above the value you see in `/proc/meminfo|grep Dirty` so that it doesn't interefere with dirty_expire_centisecs explained below
- `sysctl -w vm.dirty_expire_centisecs=$(( 10*60*100 ))` - allow page to be left dirty no longer than 10 mins. If unwritten page stays longer than time set here, kernel starts writing it out.

_The sysctl parameters descriptions are borrowed [here](https://github.com/lomik/go-carbon#os-tuning)_
