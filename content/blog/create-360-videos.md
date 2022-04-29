---
title: Create 360 Videos
date: 2019-08-15T15:42:11.000+00:00
draft: true

---
### Getting Started

* Install [Insta360 Studio](https://www.insta360.com/download/insta360-onex#studio2019_download)
* Install [Adobe Premiere Pro](https://www.adobe.com/products/premiere.html)
* Test

### Convert Files

* Import videos to Insta360 Studio
* Select all and Batch Export
* Choose presets as follows:
  * Target Resolution: Original
  * Bitrate (Mbps): Original bitrate
  * Encoding format: H.264
  * Process:
    * \[X\] FlowState Stabilization
    * \[X\] Lock direction
    * \[ \] Use Nadir Logo

### Edit Clips

* Open Premiere Pro
* Add all mp4 equirectangular videos to Library
* Select all videos and Create Proxies
* Add Toggle Proxies button to editor view (turn on proxies)
* Add Effects view to panel of choice
* Select all video clips and apply VR Sphere Rotation effect
* Add video clips to Sequence view and trim where necessary
* Click on a video clip and switch to Effect Editor view
* Expand VR Sphere Rotation folder
* Click on stopwatch indicators for Roll (X), Pan (Y), and Tilt (Z)
* Play back video and pause to add keyframes (automatically added when X, Y, or Z are changed at paused position)
* Repeat for all video clips

### Export Video

* Click inside Sequence view
* File > Export

### Inject VR Metadata

* Clone or download `spatial-metadata` [repo on GitHub](https://github.com/google/spatial-media/)
* Install `pyinstaller` via pip
* Run the following to install from source:

```bash
cd spatial-media/spatialmedia
pyinstaller spatial_media_metadata_injector.spec
```

* Open Spatial Media Metadata Injector (can use Spotlight on Mac)
* Click Open and select exported video
* Check "My video is spherical (360)"
* Click "Inject metadata" and save as separate file

### Publish Video

* Upload new video to YouTube