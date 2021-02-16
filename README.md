AssRender is an AviSynth plugin that renders ASS/SSA and SRT (without the HTML-like markup) subtitles. It uses libass to render the subtitles, which makes it the fastest and most correct ASS renderer for AviSynth.

This also means that it is much more picky about script syntax than VSFilter and friends, so keep that in mind before blaming the filter. Yes, people have reported a lot of errors that were actually the script author’s fault.

# Usage

assrender(clip, string file, [string vfr, int hinting, float scale, float line_spacing, float dar, float sar, int top, int bottom, int left, int right, string charset, int debuglevel, string fontdir, string srt_font, string colorspace])

`string file`

Your subtitle file. May be ASS, SSA or SRT.
	
`string vfr`

Specify timecodes v1 or v2 file when working with VFRaC.
	
`int hinting`

Font hinting mode. Choose between none (0, default), light (1), normal (2) and Freetype native (3) autohinting.
	
`float scale`

Font scale. Defaults to 1.0.
	
`float line_spacing`

Line spacing in pixels. Defaults to 1.0 and won’t be scaled with frame size.
	
`float dar`, `float sar`

Aspect ratio. Of course you need to set both parameters.
	
`int top`, `int bottom`, `int left`, `int right`

Margins. They will be added to the frame size and may be negative.
	
`string charset`

Character set to use, in GNU iconv or enca format. Defaults to UTF-8.
Example enca format: `enca:pl:cp1250`
(guess the encoding for Polish, fall back on `cp1250`)
		
`int debuglevel`

How much crap assrender is supposed to spam to stderr.
	
`string fontdir`

Additional font directory.
Useful if you are lazy but want to keep your system fonts clean.
	
`string srt_font`

Font to use for SRT subtitles.
Defaults to whatever Fontconfig chooses for “sans-serif”.
	
`string colorspace`

The color space of your (YUV) video. Possible values:
 - Rec2020, BT.2020
 - Rec709, BT.709
 - Rec601, BT.601
 
Default is to use the ASS script’s “Video Colorspace” property, else guess based on video resolution (width > 1920 or height > 1080 → BT.2020, then width > 1280 or height > 576 → BT.709).

# Build instructions

## Windows Visual Studio 2019

* Clone repo
  Clone https://github.com/pinterf/assrender from IDE or 
    git clone https://github.com/pinterf/assrender
    git submodule update --init --recursive --remote

  Prequisite: vsyasm integration
    - get VSYASM from https://github.com/ShiftMediaProject/VSYASM
    - run install_script.bat
    
  Prequisite: avisynth.lib versions
    - find in Avisynth+ C API installation
    - copy them to repo_dir\lib\x86-64\ and repo_dir\lib\x86-32\
      32 and 64 bit versions respectively

* Build:
    Open solution file from IDE

## Windows GCC (mingw installed by msys2)

  Prequisite: avisynth.lib versions
    - find in Avisynth+ C API installation
    - copy them to repo_dir\lib\x86-64\ and repo_dir\lib\x86-32\
      32 and 64 bit versions respectively

  Prequisite: libass package (see below)

* Libass package (if not exists)

  List libass versions
  --------------------
  $ pacman -Ss libass

    mingw32/mingw-w64-i686-libass 0.15.0-1
      A portable library for SSA/ASS subtitles rendering (mingw-w64)
    mingw64/mingw-w64-x86_64-libass 0.15.0-1
      A portable library for SSA/ASS subtitles rendering (mingw-w64)

  Get
  ---
  for x64 version:
  $ pacman -S mingw64/mingw-w64-x86_64-libass

    resolving dependencies...
    looking for conflicting packages...
    warning: dependency cycle detected:
    warning: mingw-w64-x86_64-harfbuzz will be installed before its mingw-w64-x86_64-freetype dependency

    Packages (10) mingw-w64-x86_64-fontconfig-2.13.93-1
                  mingw-w64-x86_64-freetype-2.10.4-1
                  mingw-w64-x86_64-fribidi-1.0.10-2
                  mingw-w64-x86_64-glib2-2.66.4-1
                  mingw-w64-x86_64-graphite2-1.3.14-2
                  mingw-w64-x86_64-harfbuzz-2.7.4-1
                  mingw-w64-x86_64-libpng-1.6.37-3  mingw-w64-x86_64-pcre-8.44-2
                  mingw-w64-x86_64-wineditline-2.205-3
                  mingw-w64-x86_64-libass-0.15.0-1

    Total Download Size:    6.92 MiB
    Total Installed Size:  42.31 MiB

    :: Proceed with installation? [Y/n]
    
  Choose Y and wait

* Clone repo
  git clone https://github.com/pinterf/assrender
  NOT needed (we have libass as package): git submodule update --init --recursive --remote

* Build
  from the 'build' folder under project root:

  del ..\CMakeCache.txt
  cmake .. -G "MinGW Makefiles"
  cmake --build . --config Release --clean-first 


## Linux
* Clone repo

        git clone https://github.com/pinterf/assrender
        NOT needed: git submodule update --init --recursive --remote
        cd assrender
        cmake -B build -S .
        cmake --build build --clean-first

* Find binaries at
    
        build/assrender/libassrender.so

* Install binaries

        cd build
        sudo make install

# Licenses
  For all modules: see msvc/licenses

# Change log

- 0.29 (20210216 - pinterf)
  project moved to https://github.com/pinterf/assrender from https://github.com/vadosnaprimer/assrender
  Move to VS2019 - v142 platform toolset
  Add .def module definition file for Avisynth 2.6 std compatibility (function name mangling)
  Update Avisynth C headers
  Check Linux and gcc-MinGW CMake build
  Add build instructions to README

- 0.28 (?)