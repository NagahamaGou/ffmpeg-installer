# FFmpeg installer

This is a shell script that automatically installs FFmpeg.  
The content of the shell script follows the installation procedure (https://trac.ffmpeg.org/wiki/CompilationGuide/Centos) on the official FFmpeg page.

## Prerequisites
Cmake 3.5 or higher is required to incorporate the AV1 encoder.  
Therefore, if 3.5 or more cmake is not installed, 3.5 or more cmake will be reinstalled by the installation shell script (./bin/ffmpeg-installer.sh).

## Installation / Uninstallation

### Install:
```sh
sh ./bin/ffmpeg-installer.sh;
```

### Uninstall:
```sh
sh ./bin/ffmpeg-uninstaller.sh;
```

## FFmpeg command syntax
```sh
ffmpeg {Input options} -i {Input file name} {Output options} {Output file name};
```

## FFmpeg command example

### Convert mp4 format videos to sequential images

- Output one image every second:  
```sh
ffmpeg -i input.mp4 -vf fps=1 output_%d.png;
```
- Output one image every minute:  
```sh
ffmpeg -i test.mp4 -vf fps=1/60 output_%04d.png
```

- Output one image every 10 minutes:  
```sh
ffmpeg -i test.mp4 -vf fps=1/600 output_%04d.png
```

### Convert serial images to video

- When converting continuous images such as input_0001.png, input_0002.png, ... to mp4 format video:  
The first -r option specifies that the image is 1 fps (one image per second).  
The second -r option specifies to output a video at 60 fps (60 images per second).  
In other words, the output moving image displays the same image 60 times per second.  
If there are 10 images ((input_0001.png to input_0010.png), a 10 second movie will be created.

```sh
ffmpeg -r 1 -i input_%04d.png -vcodec libx264 -pix_fmt yuv420p -r 60 output.mp4;
```

- When the input image is 30 fps and the output video is 60 fps:  
If there are 120 images (input_0001.png to image_0120.png), a 4-second movie is created.

```sh
ffmpeg -r 30 -i input_%04d.png -vcodec libx264 -pix_fmt yuv420p -r 60 output.mp4;
```

- Video creation from images ~ Avoid dropping frames ~:  
If you specify the -r option twice, unintended frame dropping may occur in some cases.  
To prevent dropped frames, replace the first -r option with the -framerate option.  
Reference: https://github.com/yihui/animation/issues/74

```sh
ffmpeg -framerate 1 -i input_%04d.png -vcodec libx264 -pix_fmt yuv420p -r 60 output.mp4;
```

## Node.js examples

- Convert serial images to video

./examples/nodejs/convert-image-to-video.js

## License
MIT - see [License file](LICENSE.txt).

## Author
Twitter: [@TakuyaMotoshima](https://twitter.com/taaaaaaakuya)  
Github: [TakuyaMotoshima](https://github.com/takuya-motoshima)  
mail to: development.takuyamotoshima@gmail.com