# xspng

a simple library for drawing png files

### Motivation

For various reasons, I've decided to reinvent the wheel when it comes to
writing png files. Primarily, I've found version differences within libpng
stopping my code's ability to be compiled on different linux computers. It's
also a pretty fun learning experience.

### How to Use It

`xspng` provides a simple api for drawing png files. It assumes you want an
`rgb` or an `rbga` colored image, and that you want your image interlaced.
`xspng` does not allow you to change these settings for ease of use (and
implementation). When you `#include <xspng.h>` in a C++ context, all the
functions are dropped into the `xspng` namespace.

#### Typedefs

Png files use 32 bit integers and 8 bit characters. Integers only use the
least significant 31 bits, though. We define these types for you as `xspng_int`
and `xspng_byte`. The image data sits in a struct of type `xspng_image`, though
I recommend using the pointer type `xspng_imagep`.

#### The API

The only functions you should need are the following:

    //allocate a new xspng_image of with the right buffer size
    xspng_imagep xspng_image_new(xspng_int width, xspng_int height);
    
    //free a xspng_imagep and it's buffer
    void xspng_image_free(xspng_imagep img);
    
    //write a xspng_imagep to file
    void xspng_image_write(xspng_imagep img, const char * filename);
    
    //set a pixel to the given rgb sample values
    void xspng_image_set_rgb(
        xspng_imagep img,
        xspng_int x, xspng_int y,
        xspng_byte r, xspng_byte g, xspng_byte b
      );
    
    //set a pixel to the given rgba sample values
    void xspng_image_set_rgba(
        xspng_imagep img,
        xspng_int x, xspng_int y,
        xspng_byte r, xspng_byte g, xspng_byte b, xspng_byte a
      );

#### Example code

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
          b = 0xff & ((x + y) / 2)
          xspng_image_set_pixel(imgp,x,y,r,g,b);
        }
      }
      
      //write to file
      xspng_image_write(imgp, "output.png");
      
      return 0;
    }
