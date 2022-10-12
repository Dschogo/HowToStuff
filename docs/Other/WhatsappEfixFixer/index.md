# Whatsapp Efix Fixer

## Uhm What da heck is that even?

Basically every file, has the creation date baked into the file itself. With this information a gallery for example can order Images based on "taken" dat, even if the file name has nothing to do with the date. Whatsapp-backups just backup the file WITHOUT the exif, why? I don't know, these few bytes wouldn't hurt anyone. Maybe you restored a Whatsapp-backup   one day, and noticed that all Images from Whatsapp got created today, and therefore the Gallery is totally fucked up. Luckily whatsapp names the images and videos based on the creation date, so we can just add the exif data back in, with a simple script.

## The script

What does it do excactly? It goes through every image in the "process_images" folder, and adds the exif data back to it. If multiple files got the same date, an incrementing counter will be added to the filename.
To use it, you can either map the working folder to the actual image/video/media folder on you connected phone, or copy the files onto the computer, and then run the script, and copy them back. That way you also have an backup of the original files incase something goes wrong. (The online Backup could become unusable, if it automatically backs up during the process)

``` py title="whatsappexiffixer.py"
from datetime import datetime
import piexif

import os
import time

errors_count = 0
errors_names = []

folder = "./process_images/"


def get_datetime(filename):
    date_str = filename.split("-")[1]
    return datetime.strptime(date_str, "%Y%m%d")


def get_date(filename):
    date_str = filename.split("-")[1]
    date_str2 = filename.split("-")[2][4:6]

    if date_str2 == "00" or int(date_str2) >= 24:
        return datetime.strptime(date_str, "%Y%m%d").strftime("%Y:%m:%d %H:%M:%S")
    else:
        return datetime.strptime(date_str + date_str2, "%Y%m%d%H").strftime("%Y:%m:%d %H:%M:%S")


allowedFileEndings = ["mp4", "jpg", "3gp", "jpeg"]

filenames = [fn for fn in os.listdir(folder) if fn.split(".")[-1] in allowedFileEndings]

num_files = len(filenames)
print("Number of files: {}".format(num_files))

for i, filename in enumerate(filenames):
    try:

        if filename.endswith("mp4") or filename.endswith("3gp"):
            date = get_datetime(filename)
            modTime = time.mktime(date.timetuple())
            os.utime(folder + filename, (modTime, modTime))

        elif filename.endswith("jpg") or filename.endswith("jpeg"):
            exif_dict = {"Exif": {piexif.ExifIFD.DateTimeOriginal: get_date(filename)}}
            exif_bytes = piexif.dump(exif_dict)
            piexif.insert(exif_bytes, folder + filename)

        num_digits = len(str(num_files))
        print(
            "{num:{width}}/{max} - {filename}".format(
                num=i + 1, width=num_digits, max=num_files, filename=folder + filename
            )
        )
    except:
        errors_count = errors_count + 1
        errors_names.append(filename)
        pass
print("\nDone!")
print("\nErroes:" + str(errors_count) + "!")
print(f"\n{errors_names}")
```

!!! danger
    This script worked for me a few months ago, maybe stuff has changed since then. So as always, it comes as is and always read code you found online before you run it. Im not responsible for any damage this script might cause.
