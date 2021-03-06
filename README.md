## xspng

a simple library for drawing png files

#### Motivation

For various reasons, I've decided to reinvent the wheel when it comes to
writing png files. Primarily, I've found version differences within libpng
stopping my code's ability to be compiled on different linux computers. It's
also a pretty fun learning experience.

#### How to Use It

`xspng` provides a simple api for drawing png files. It assumes you want an
`rgb` or an `rbga` colored image, and that you want your image interlaced.
`xspng` does not allow you to change these settings for ease of use (and
implementation). A separate C++ specific API is also included. Pixels consist
of four channels of 8-bit unsigned integers, one each for red, green, blue,
and alpha.

#### The C API

Use of `xspng` should be relatively obvious from the header file, but you may
find the example code below illuminating otherwise.

###### Example code

![generated by the below example C code](/examples/example.png)

    #include <xspng.h>
    
    int main() {
      
      //set width and height
      xspng_int w = 256, h = 256;
      
      //allocate image buffer
      xspng_imagep imgp = xspng_image_new(w,h);
      
      //iterate through x,y coordinates and set rgb values
      xspng_int x,y;
      xspng_byte r,g,b;
      for (y = 0; y < h; y++) {
        for (x = 0; x < w; x++) {
          r = 0xff & x;
          g = 0xff & y;
          b = 0xff & ((x + y) / 2);
          xspng_image_set_rgb(imgp,x,y,r,g,b);
        }
      }
      
      //write to file
      xspng_image_write(imgp, "output.png");
      
      return 0;
    }

#### The C++ API

Two classes are provided by `xspng.h`: `xspng::Color` and `xspng::Image`. They
should be pretty easy to use.

###### C++ Example Code

![generated by the below code sample](/examples/examplepp.png)

    #include <xspngpp.h>
    
    using namespace xspng;
    
    int main() {
        int w = 256, h = 256;
        Color c;
        Image img(w,h);
        
        for (int y = 0; y < h; y++) {
            for (int x = 0; x < w; x++) {
                c.r = (4 * x) % 256;
                c.g = (2 * y) % 256;
                c.b = (x + y) % 256;
                img.setPixel(x,y,c);
            }
        }
        
        img.write("examplepp.png");
        
        return 0;
    }

#### Dependencies

This code depends on zlib version 1.2.8 to work properly. Other than standard C
and C++ headers, no other libraries are used by this package. I've decided to
ship the header and object files for zlib alongside xspng so that basically any
build environment can compile my code.

