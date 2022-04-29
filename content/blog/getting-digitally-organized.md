---
title: "Getting Digitally Organized"
date: 2019-03-01T02:44:54-05:00
draft: false
tags: ["organization"]
description: "Tools and tips to organize your computer files and folders."
meta_img: "/img/favicon.png"
hacker_news_id: ""
---

After years of creating "photo dump" and "temp" directories to clear space on devices, I finally decided to make time to organize as many of my digital assets as best as I could. I knew I had hundreds of GB of stuff, probably closer to 2 TB.

## Storage Options

My 1TB external HDD would probably be able to store most, if not all, of my digital junk, but the USB connection has deteriorated significantly over the years, and it's a gamble every time I plug it in as to whether or not the computer will even recognize the hard drive.

There were two major options for my future storage solution:

* Cloud (Amazon Cloud, Google Drive, OneDrive)
* External HD (SSD, HDD)

Cloud storage offers many benefits: the ability to access your files from anywhere, security and data backups, and a typically friendly UI to easily view and organize files.

However, the vast majority of my assets are old and should be archived, certainly not requiring on-the-fly access. Since I feel comfortable dealing with file organization using the command line and Finder, I certainly didn't need a fancy web interface. And while the added security and data backups offer nice insurance, it also means paying money to give my data to someone else. For these reasons, I decided to go with a personal storage device.

Deciding between a hard disk drive (HDD) and a solid state drive (SSD) is a personal decision, primarily based on the following factors:

1. Budget
1. Speed
1. Portability
1. Capacity

HDDs are cheap and can go for the same price as a SSD with 1/8 its capacity. However, they're bulkier, louder, and most importantly, slower.

Since I had the budget for it, and I really liked the idea of quickly storing and accessing files from a very portable device, I opted for the [1TB SanDisk Extreme SSD](https://www.amazon.com/SanDisk-1TB-Extreme-Portable-SDSSDE60-1T00-G25/dp/B078STRHBX) for its portability, durability, and speed.

{{< figure src="https://images-na.ssl-images-amazon.com/images/I/91fygYUinmL._SX679_.jpg" link="https://www.amazon.com/SanDisk-1TB-Extreme-Portable-SDSSDE60-1T00-G25/dp/B078STRHBX" caption="The SanDisk Extreme SSD is water-resistant, dust-resistant, shock-resistant, and achieves transfer speeds up to 550MB/s." target="_blank" >}}

## Setting up the External HD

Choosing how to format the external SSD was fairly straightforward: among the most popular formatting options is `exFAT` which is the only one that supports both Windows and Mac.

##### CAUTION: Formatting a hard drive means wiping all data from it, so be sure to move your files somewhere safe before beginning

| Format | Windows |    Mac    | Linux  |
|:------:|:-------:|:---------:|:------:|
| exFAT  |   yes   |    yes    |  yes   |
| NTFS   |   yes   | read-only |  some  |
| HFS+   |   no    |    yes    |  no    |
| APFS   |   no    |   some    |  no    |
| FAT32  |   no    |   some    |  no    |

One consideration I overlooked while formatting my drive was [allocation unit size](https://superuser.com/a/31690), but there is an alternative solution to keep file transfer speeds fast and disk usage low (read on).

## Moving Files

Before starting the file organization, I downloaded [Daisy Disk](https://daisydiskapp.com/) to visualize my disk space usage. This was invaluable in discovering excessively large directories and files, finding unnecessary system files, and keeping track of overall storage capacity. It also led me to realize the significant difference that allocation unit size can make and to come up with an alternative to reformatting.

I created my root directories to loosely match those of any Windows, Mac, or Linux home directory:

```
/_toOrganize
/Code
/Documents
/Games
/Images
/Music
/Videos
```

Additionally, I kept initial imports organized by source, so my temporary directories (and/or subdirectories) looked something like:

```
/_MacbookPro
/_SurfacePro
/_Android
/_Lenovo
/_GoogleDrive
```

## Organizing and Cleaning

While importing and organizing files, I ran into lots of duplicate files, especially in `Images` subdirectories. Comparing file size and creation date helped identify certain files were indeed identical and verify that duplicates could be safely deleted.

Similarly, I had many "photo dump" directories that I consolidated into one massive directory. After consolidating all these files, I organized them into subdirectories by year (e.g. `/Images/_Phone Dumps/2013`, `/Images/_Phone Dumps/2014`). Sorting each subdirectory by create date (or equivalently, by their datetime-based filenames), allowed me to more quickly group files by event. For example, the last of the `2016` photos as well as the first few in `2017` could be moved into `/Images/New Year's Eve 2017`.

{{< figure src="https://s3.amazonaws.com/dpfrakes/photo-dump-dir.png" caption="Actual file path to a photo from 2016 I was looking for." >}}

Mac Finder and Windows Explorer both have a helpful "merge" feature that allows you to merge directories' contents recursively when the root directories have the same name. This was very handy when consolidating and de-duplicating various media folders, including my `iTunes` music.

As mentioned before, Daisy Disk helped me discover the importance of allocation unit size. As I looked through my code repositories, I discovered that many very small files (< 1 KB) were taking up a minimum of 4.1KB. After a quick Google search, I discovered it was the [minimum size required to store metadata](https://superuser.com/a/142900). To get around this individual file size requirement, I compressed each of my code repositories, saving many MB of storage by storing all files in a code base as [one zip file](#developer-notes).

Another space-saving technique I used was to convert large video files using QuickTime and [Handbrake](https://handbrake.fr/). I have movies and doggy-cam recordings that each take up lots of space, but by reducing video resolution and/or exporting to a more space-efficient file type, I saved a lot more space without having to delete files.

Converting video files from `.avi` to `.mov` reduced file size by about 75%, **but it did cost me my original metadata** (i.e. creation date, last modified date).

## Conclusion

I eventually saved well over 500GB of disk space, maybe even up to 1TB, by removing unnecessary files and compressing large directories containing lots of small individual files.

Most of my purgeable disk space was occupied by code repositories: populated `node_modules` directories, compiled bytecode files, massive `.git` packs, Python virtual environments, and hidden system files.

{{< figure src="https://s3.amazonaws.com/dpfrakes/daisy-disk-results.png" caption="Still a bit of organizing left to do, but after file cleaning and de-duplicating, 560GB isn't too shabby for hosting all my files from 6 devices spanning more than a decade." >}}

## Developer Notes

```bash
# List disk space available on mounted drives
df -h

# Output the number of *.pyc files in directory (recursive)
find . -name "*.pyc" | wc -l

# Find and delete all *.pyc files in a directory (recursive)
find . -name "*.pyc" -delete

# Compress a directory
zip -r mydir.zip mydir
```
