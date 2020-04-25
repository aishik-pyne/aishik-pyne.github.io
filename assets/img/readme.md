## How to minify images for web optimization

Using imagemin-cli with mozjpeg plugin convert into minified versions into this img folder



## Compressing images command using imagemagick
```bash
magick source.jpg -strip -interlace Plane -gaussian-blur 0.05 -quality 85% result.jpg
```

## Crop and resize image for mobile 

Name it m_<intialname>.ext

To crop from center in an aspect ratio
```bash
magick.exe source.jpg -gravity center -crop 9:16  result.jpg 
```

To crop from center with offset in an aspect ratio in x axis
```bash
magick.exe source.jpg -gravity center -crop 9:16+400  result.jpg 
```

To crop from center with offset in an aspect ratio in x & y axis
```bash
magick.exe source.jpg -gravity center -crop 9:16+400+400  result.jpg 
```
