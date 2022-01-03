---
---

Most unix-like operating systems have libvips packages, check your package 
manager. For macOS, there are packages in Homebrew, MacPorts and Fink. For
Windows, there are pre-compiled binaries in the [Download area]({{
site.github.releases_url }}).

## Installing on macOS with homebrew

Install [homebrew](https://brew.sh/), then enter:

	brew install vips

That will install vips with most optional add-ons included. 

## Installing the Windows binary

Download `vips-dev-w64-web-x.y.z.zip` from the [Download area]({{ 
site.github.releases_url }}) and unzip somewhere. At the command-prompt, `cd`
to `vips-x.y.z/bin` and run (for example):

	vips.exe invert some/input/file.jpg some/output/file.jpg

If you want to run `vips.exe` from some other directory on your PC, 
you'll need to set your `PATH`.

The zipfile includes all the libraries and headers for developing with C with
any compiler. For C++, you must build with `g++`, or rebuild the C++ API 
with your compiler, or just use the C API. 

The `vips-dev-w64-web-x.y.z.zip` is built with a small set of relatively secure
file format readers and can be used in a potentially hostile environment. The
much larger `vips-dev-w64-all-x.y.z.zip` includes all the file format readers
that libvips supports and care should be taken before public deployment.

The Windows binary is built
by [build-win64](https://github.com/jcupitt/build-win64). This is a
containerised mingw build system: on any host, install Docker, 
clone the project, and type `./build.sh 8.5`. The README has notes.

## Building libvips from a source tarball

If the packaged version is too old, you might need to build from source. 

Download `vips-x.y.z.tar.gz` from the [Download area]({{
site.github.releases_url }}), then:

	tar xf vips-x.y.z.tar.gz
	cd vips-x.y.z
	./configure

Check the summary at the end of `configure` carefully.  libvips must have
`build-essential`, `pkg-config`, `libglib2.0-dev`, `libexpat1-dev`.

You'll need the dev packages for the file format support you want. For basic
jpeg and tiff support, you'll need `libtiff5-dev`, `libjpeg-turbo8-dev`,
and `libgsf-1-dev`.  See the **Dependencies** section below for a full list
of the things that libvips can be configured to use.

Once `configure` is looking OK, compile and install with the usual:

	make
	sudo make install
	sudo ldconfig

By default this will install files to `/usr/local`.

We have detailed guides on the wiki for [building for
Windows](https://github.com/jcupitt/libvips/wiki/Build-for-Windows) and
[building for macOS](https://github.com/jcupitt/libvips/wiki/Build-for-macOS).

## Building libvips from git

Checkout the latest sources with:

	git clone git://github.com/jcupitt/libvips.git

Building from git needs more packages, you'll need at least `gtk-doc` 
and `gobject-introspection`, see the dependencies section below. 

Then:

	./autogen.sh
	make
	sudo make install

And perhaps also:

	sudo ldconfig

## Dependencies 

libvips has to have `libglib2.0-dev` and `expat`. Other dependencies are
optional, see below.

## Optional dependencies

If suitable versions are found, libvips will add support for the following
libraries automatically. See `./configure --help` for a set of flags to
control library detection. Packages are generally found with `pkg-config`,
so make sure that is working.

Libraries like giflib do not usually use `pkg-config` so libvips looks for
them in the default path and in `$prefix`. If you have installed your own
versions of these libraries in a different location, libvips will not see
them. Use switches to libvips configure like:

	./configure --prefix=/Users/john/vips \
		--with-giflib-includes=/opt/local/include \
		--with-giflib-libraries=/opt/local/lib 

### libjpeg

The IJG JPEG library. Use the `-turbo` version if you can. 

### libexif

If available, libvips adds support for EXIF metadata in JPEG files.

### giflib

The standard gif loader. If this is not present, vips will try to load gifs
via imagemagick instead.

### librsvg

The usual SVG loader. If this is not present, vips will try to load SVGs
via imagemagick instead.

### PDFium

If present, libvips will attempt to load PDFs via PDFium. This library must be
packaged by https://github.com/jcupitt/docker-builds/tree/master/pdfium

If PDFium is not detected, libvips will look for poppler-glib instead.

### poppler-glib

The Poppler PDF renderer, with a glib API. If this is not present, vips
will try to load PDFs via imagemagick.

### libgsf-1

If available, libvips adds support for creating image pyramids with `dzsave`. 

### libtiff

The TIFF library. It needs to be built with support for JPEG and
ZIP compression. 3.4b037 and later are known to be OK. 

### fftw3

If libvips finds this library, it uses it for fourier transforms. 

### lcms2

If present, `vips_icc_import()`, `vips_icc_export()` and `vips_icc_transform()`
are available for transforming images with ICC profiles. 

### libpng

If present, libvips can load and save png files. 

### libimagequant

If present, libvips can write 8-bit palette-ised PNGs.

### ImageMagick, or optionally GraphicsMagick

If available, libvips adds support for loading all libMagick-supported
image file types. Use `--with-magickpackage=GraphicsMagick` to build against 
graphicsmagick instead.

Imagemagick 6.9+ needs to have been built with `--with-modules`. Most packaged
IMs are, I think.

If you are going to be using libvips with untrusted images, perhaps in a
web server, for example, you should consider the security implications of
enabling a package with such a large attack surface. 

### pangoft2

If available, libvips adds support for text rendering. You need the
package pangoft2 in `pkg-config --list-all`.

### orc-0.4

If available, vips will accelerate some operations with this run-time
compiler.

### matio

If available, vips can load images from Matlab save files.

### cfitsio

If available, vips can load FITS images.

### libwebp

If available, vips can load and save WebP images.

### libniftiio

If available, vips can load and save NIFTI images.

### OpenEXR

If available, libvips will directly read (but not write, sadly)
OpenEXR images.

### OpenSlide

If available, libvips can load OpenSlide-supported virtual slide
files: Aperio, Hamamatsu, Leica, MIRAX, Sakura, Trestle, and Ventana.

### libheif

If available, libvips can load and save HEIC images. 

