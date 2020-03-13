# Border Trimmer
## Function Description
Assume you are given an image that consists of the actual image content which is contained inside a frame/set of borders of a **known RGB** color but **unknown widths**.  The borders are all a **single color** and **perfectly row/column aligned**. There is an example image attached but please note that this should work with any image that meets the above specifications.

Write a program in C/C++ that takes an image file (e.g. a PNG) and trims the borders, then saves off the image back to an image format again (e.g. a PNG). You may not use any helper functions to do the actual trimming; your function must do this pixel by pixel yourself.

## Border Detailed Description
- known RGB, single color
- unknown widths
- perfectly row/column aligned
- might not consider alpha channel

## Thoughts
- Different image types might need different libraries.
- RGB to gray scale (if use the helper function in OpenCV directly, might cause problems with conversion precision.)
- What if the border is fully transparent.

## Read Image Approaches
(only consider the most commonly used image types)
#### 1. Use "libpng" and "libjpeg" for png and jpeg
#### 2. Use "opencv" for png, jpeg and webP
In both approaches, use "libmagic" to detect the image type by the magic number at the beginning of image files

## How to run
#### borderTrimmer.cpp
```
clang++ -lz -lpng16 -lmagic -ljpeg borderTrimmer.cpp

./a.out jpeg_i.jpeg jpeg_o.jpeg
./a.out png_8_i.png png_8_o.png
./a.out png_i.png png_o.png
./a.out png_RGBA_i.png png_RGBA_o.png
./a.out png_up_i.png png_up_o.png
```
#### borderTrimmer_cv.cpp
```
g++ $(pkg-config --cflags --libs opencv4)  -lmagic -std=c++11 borderTrimmer_cv.cpp -o cv.out

./cv.out jpeg_i.jpeg jpeg_o_cv.jpeg
./cv.out png_8_i.png png_8_o_cv.png
./cv.out png_i.png png_o_cv.png
./cv.out png_RGBA_i.png png_RGBA_o_cv.png
./cv.out png_up_i.png png_up_o_cv.png
./cv.out webP_i.webp webP_o_cv.webp
```

## Test images
| File Name | Type | Notes |
| ------ | ------ | ------ |
| jpeg_i | jpeg |Created in PS. Border has noise|
| jpeg_lr_i | jpeg |Created in PS. Border has noise. Only has left and right borders.|
| png_8_i | png8 ||
| png_i | png ||
| png_RGBA_i | png |Fully transparent border.|
| png_up_i | png |Only has top border.|
|webP_i|webp|Boder width is 0, don't have tool to edit (can edit it use OpenCV). Take as 0-width border teat case.|

## Limitations
#### Didn't cover all the image types
- gif image can't be read by OpenCV directly, but can be read by "FreeImage" or "gif_lib"
#### Trimmed image might have bigger size than original one

## Test case input and output images
[origin, lib output, opencv output]
#### jpeg_i.jpeg
<div class="display: flex;">
  <div style="flex: 33.33%;padding: 5px;">
    <img src="https://backto1995.com/temp_img/jpeg_i.jpeg">
  </div>
  <div style="flex: 33.33%;padding: 5px;">
    <img src="https://backto1995.com/temp_img/jpeg_o.jpeg">
  </div>
  <div style="flex: 33.33%;padding: 5px;">
    <img src="https://backto1995.com/temp_img/jpeg_o_cv.jpeg">
  </div>
</div>

<img src="https://backto1995.com/temp_img/jpeg_i.jpeg" width="30%"/>
<img src="https://backto1995.com/temp_img/jpeg_o.jpeg" width="30%" style="float:left"/>
<img src="https://backto1995.com/temp_img/jpeg_o_cv.jpeg" width="30%" style="float:left"/>
