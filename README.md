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
g++ $(pkg-config --cflags --libs opencv4)  -lmagic -std=c++11 borderTrimmer_cv.cpp -o cv

./cv jpeg_i.jpeg jpeg_o_cv.jpeg
./cv png_8_i.png png_8_o_cv.png
./cv png_i.png png_o_cv.png
./cv png_RGBA_i.png png_RGBA_o_cv.png
./cv png_up_i.png png_up_o_cv.png
./cv webP_i.webp webP_o_cv.webp
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
|webP_i|webp|Boder width is 0, don't have tool to edit (can edit it use OpenCV)|

## Limitations
#### didn't cover all the image types
gif image can't be read by OpenCV directly, but can be read by "FreeImage"
















/*
 * A simple libpng example program
 * http://zarb.org/~gc/html/libpng.html
 *
 * Modified by Yoshimasa Niwa to make it much simpler
 * and support all defined color_type.
 *
 * To build, use the next instruction on OS X.
 * $ brew install libpng
 * $ clang -lz -lpng16 libpng_test.c
 *
 * Copyright 2002-2010 Guillaume Cottenceau.
 *
 * This software may be freely redistributed under the terms
 * of the X11 license.
 *
 */

//  Dec  Char                           Dec  Char     Dec  Char     Dec  Char
// ---------                           ---------     ---------     ----------
//   0  NUL (null)                      32  SPACE     64  @         96  `
//   1  SOH (start of heading)          33  !         65  A         97  a
//   2  STX (start of text)             34  "         66  B         98  b
//   3  ETX (end of text)               35  #         67  C         99  c
//   4  EOT (end of transmission)       36  $         68  D        100  d
//   5  ENQ (enquiry)                   37  %         69  E        101  e
//   6  ACK (acknowledge)               38  &         70  F        102  f
//   7  BEL (bell)                      39  '         71  G        103  g
//   8  BS  (backspace)                 40  (         72  H        104  h
//   9  TAB (horizontal tab)            41  )         73  I        105  i
//  10  LF  (NL line feed, new line)    42  *         74  J        106  j
//  11  VT  (vertical tab)              43  +         75  K        107  k
//  12  FF  (NP form feed, new page)    44  ,         76  L        108  l
//  13  CR  (carriage return)           45  -         77  M        109  m
//  14  SO  (shift out)                 46  .         78  N        110  n
//  15  SI  (shift in)                  47  /         79  O        111  o
//  16  DLE (data link escape)          48  0         80  P        112  p
//  17  DC1 (device control 1)          49  1         81  Q        113  q
//  18  DC2 (device control 2)          50  2         82  R        114  r
//  19  DC3 (device control 3)          51  3         83  S        115  s
//  20  DC4 (device control 4)          52  4         84  T        116  t
//  21  NAK (negative acknowledge)      53  5         85  U        117  u
//  22  SYN (synchronous idle)          54  6         86  V        118  v
//  23  ETB (end of trans. block)       55  7         87  W        119  w
//  24  CAN (cancel)                    56  8         88  X        120  x
//  25  EM  (end of medium)             57  9         89  Y        121  y
//  26  SUB (substitute)                58  :         90  Z        122  z
//  27  ESC (escape)                    59  ;         91  [        123  {
//  28  FS  (file separator)            60  <         92  \        124  |
//  29  GS  (group separator)           61  =         93  ]        125  }
//  30  RS  (record separator)          62  >         94  ^        126  ~
//  31  US  (unit separator)            63  ?         95  _        127  DEL
