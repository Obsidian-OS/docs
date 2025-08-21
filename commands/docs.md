---
title: obsidianwall
nav_order: 4.4
parent: Commands
---
# `obsidianwall` - ObsidianOS Image Compositor

`obsidianwall` is a Bash script that leverages ImageMagick 7+ for comprehensive image composition and manipulation. It provides a command-line interface to apply various effects, transformations, and overlays to images.

## Usage

The `obsidianwall` utility is invoked with the following syntax:

```bash
obsidianwall [OPTIONS] [INPUT] [OVERLAY] [OUTPUT]
```

*   `INPUT`: Path to the background image. Default: `photo.jpg`
*   `OVERLAY`: (Optional) Path to the overlay image. Default: `/usr/share/pixmaps/obsidianos.png`
*   `OUTPUT`: Desired path for the output image. Default: `obsidian-wallpaper.png`

If positional arguments for `INPUT`, `OVERLAY`, or `OUTPUT` are not provided, the respective default values are utilized.

```bash
obsidianwall input.jpg overlay.png output.png
```

## Options

The following options control the behavior and effects applied by `obsidianwall`:

### Basic Options
*   `-i, --input FILE`: Specifies the input image file. Default: `photo.jpg`
*   `-o, --overlay FILE`: Specifies the overlay image file. Default: `/usr/share/pixmaps/obsidianos.png`
*   `-O, --output FILE`: Specifies the output image file. Default: `obsidian-wallpaper.png`
*   `-s, --scale PERCENT`: Sets the overlay scale percentage. Default: `30`
*   `-g, --gravity POSITION`: Defines the overlay position. Default: `center`
*   `-r, --resolution WxH`: Forces the output resolution (e.g., `1920x1080`).
*   `-q, --quality PERCENT`: Sets the JPEG quality. Default: `95`
*   `-f, --format FORMAT`: Specifies the output format (auto-detected from extension, e.g., `png`, `jpg`).

### Positioning Options
*   `--offset-x PIXELS`: Horizontal offset from the gravity point. Default: `0`
*   `--offset-y PIXELS`: Vertical offset from the gravity point. Default: `0`
*   `--gravity-list`: Displays all available gravity positions.

### Overlay Transformations
*   `--rotation DEGREES`: Rotates the overlay. Default: `0`
*   `--skew-x DEGREES`: Skews the overlay horizontally. Default: `0`
*   `--skew-y DEGREES`: Skews the overlay vertically. Default: `0`
*   `--opacity PERCENT`: Sets the overlay opacity. Default: `100`
*   `--blur RADIUS`: Applies blur to the overlay. Default: `0`
*   `--shadow RADIUS`: Adds a shadow to the overlay. Default: `0`

### Background Effects
*   `--brightness PERCENT`: Adjusts the brightness. Default: `100`
*   `--contrast PERCENT`: Adjusts the contrast. Default: `100`
*   `--saturation PERCENT`: Adjusts the saturation. Default: `100`
*   `--hue DEGREES`: Shifts the hue. Default: `0`
*   `--gamma VALUE`: Applies gamma correction. Default: `1.0`
*   `--vignette STRENGTH`: Adds a vignette effect. Default: `0`
*   `--noise LEVEL`: Adds noise. Default: `0`
*   `--sharpen RADIUS`: Sharpens the image. Default: `0`

### Artistic Effects
*   `--emboss RADIUS`: Applies an emboss effect. Default: `0`
*   `--sepia THRESHOLD`: Applies a sepia tone effect. Default: `0`
*   `--polaroid ANGLE`: Applies a polaroid effect. Default: `0`
*   `--sketch RADIUS`: Applies a sketch effect. Default: `0`
*   `--charcoal RADIUS`: Applies a charcoal effect. Default: `0`
*   `--oil-paint RADIUS`: Applies an oil painting effect. Default: `0`
*   `--edge-detect RADIUS`: Performs edge detection. Default: `0`
*   `--solarize THRESHOLD`: Applies a solarization effect. Default: `0`
*   `--posterize LEVELS`: Applies a posterize effect. Default: `0`

### Distortion Effects
*   `--wave-amplitude PIXELS`: Sets the wave distortion amplitude. Default: `0`
*   `--wave-length PIXELS`: Sets the wave distortion length. Default: `0`
*   `--swirl DEGREES`: Applies swirl distortion. Default: `0`
*   `--implode AMOUNT`: Applies implode distortion. Default: `0`
*   `--spread PIXELS`: Spreads pixels. Default: `0`

### Borders and Frames
*   `--border-width PIXELS`: Sets the border width. Default: `0`
*   `--border-color COLOR`: Sets the border color. Default: `black`
*   `--background COLOR`: Specifies the background color.
*   `--matte-color COLOR`: Specifies the matte color for transparent areas.

### Color Adjustments
*   `--tint COLOR`: Applies a tint color overlay.
*   `--threshold PERCENT`: Sets the black/white threshold. Default: `0`
*   `--levels "black,gamma,white"`: Adjusts color levels.
*   `--colorspace SPACE`: Converts the colorspace.
*   `--modulate "brightness,saturation,hue"`: Modulates color values.
*   `--channel-fx EXPRESSION`: Applies channel effects.
*   `--color-matrix MATRIX`: Applies a color matrix.

### Geometry and Cropping
*   `--crop GEOMETRY`: Crops the image to a specified geometry (WxH+X+Y).
*   `--trim`: Removes transparent or uniform borders.
*   `--chop GEOMETRY`: Removes pixels from the image.
*   `--splice GEOMETRY`: Inserts pixels into the image.
*   `--extent GEOMETRY`: Extends the image to a specified geometry.
*   `--liquid-rescale GEOMETRY`: Performs content-aware resizing.

### Transformations
*   `--flop`: Mirrors the image horizontally.
*   `--flip`: Mirrors the image vertically.
*   `--transpose`: Transposes the image (flip + 90° rotation).
*   `--transverse`: Transverses the image (flop + 90° rotation).
*   `--roll GEOMETRY`: Rolls image pixels.

### Blur and Sharpen
*   `--adaptive-blur RADIUS`: Applies adaptive blur.
*   `--adaptive-sharpen RADIUS`: Applies adaptive sharpen.
*   `--bilateral-blur GEOMETRY`: Applies bilateral blur (edge-preserving).
*   `--motion-blur GEOMETRY`: Applies motion blur (angle x radius).
*   `--radial-blur ANGLE`: Applies radial blur. Default: `0`
*   `--selective-blur GEOMETRY`: Applies selective blur.
*   `--unsharp GEOMETRY`: Applies an unsharp mask (radius x sigma+gain+threshold).

### Advanced Options
*   `--composite-method METHOD`: Sets the composite method. Default: `over`
*   `--compression TYPE`: Sets the compression type. Default: `none`
*   `--depth BITS`: Sets the color depth. Default: `8`
*   `--profile PATH`: Applies an ICC color profile.
*   `--strip-profiles`: Strips all color profiles.
*   `--interlace TYPE`: Sets the interlacing type. Default: `none`
*   `--filter TYPE`: Sets the resize filter. Default: `lanczos`
*   `--virtual-pixel TYPE`: Sets the virtual pixel method. Default: `edge`
*   `--interpolate TYPE`: Sets the interpolation method. Default: `average`
*   `--alpha-channel TYPE`: Handles the alpha channel. Default: `activate`

### Annotation and Watermarks
*   `--annotate TEXT`: Adds text annotation.
*   `--annotate-font FONT`: Sets the font for annotation.
*   `--annotate-size SIZE`: Sets the font size. Default: `12`
*   `--annotate-color COLOR`: Sets the font color. Default: `white`
*   `--annotate-position POS`: Sets the text position. Default: `south`
*   `--watermark FILE`: Specifies an additional watermark image.
*   `--watermark-gravity POS`: Sets the watermark position. Default: `southeast`
*   `--watermark-opacity PERCENT`: Sets the watermark opacity. Default: `50`

### Enhancement Options
*   `--despeckle`: Removes speckle noise.
*   `--enhance`: Enhances the image.
*   `--equalize`: Performs histogram equalization.
*   `--normalize`: Normalizes contrast.
*   `--auto-level`: Auto-adjusts levels.
*   `--auto-gamma`: Auto-adjusts gamma.
*   `--linear-stretch "black,white"`: Performs linear stretch.
*   `--sigmoidal-contrast "contrast,midpoint"`: Applies sigmoidal contrast.

### Advanced Effects
*   `--morphology "method:kernel"`: Performs morphological operations.
*   `--distort "method:args"`: Performs distortion operations.
*   `--fx EXPRESSION`: Applies mathematical operations.
*   `--evaluate "operator:value"`: Evaluates a mathematical expression.
*   `--function "name:parameters"`: Applies a function.

### Utility Options
*   `-h, --help`: Displays the help message.
*   `-v, --verbose`: Enables verbose output.
*   `--version`: Displays the script version.
*   `--list-formats`: Lists supported image formats.
*   `--list-filters`: Lists available resize filters.
*   `--list-composite-methods`: Lists available composite methods.
*   `--dry-run`: Shows commands without executing them.

## Dependencies

*   **ImageMagick 7+**: The script requires the `magick` command-line tool.
    *   **Installation (ObsidianOS/Arch GNU/Linux):** `sudo pacman -S imagemagick`

## Installation

The `obsidianwall` script can be obtained and made executable via the following methods:

### AUR Package

```bash
yay -S obsidianwall-git  # or paru
```

### Source Compilation

1.  **Repository Cloning:**
    ```bash
    git clone https://github.com/Obsidian-OS/obsidianwall
    cd obsidianwall
    ```
2.  **Executable Permissions:**
    ```bash
    chmod +x obsidianwall
    ```
3.  **PATH Integration (Optional):** To enable direct execution from any directory, move the script to a directory included in the system's PATH, e.g., `/usr/local/bin`:
    ```bash
    sudo mv obsidianwall /usr/local/bin/
    ```

## Examples

*   **Basic composition:**
    ```bash
    obsidianwall input.jpg overlay.png output.png
    ```
*   **Scale overlay, position it, and add a shadow:**
    ```bash
    obsidianwall -s 50 --gravity northeast --shadow 10 input.jpg overlay.png
    ```
*   **Apply sepia tone, vignette, and a gold border:**
    ```bash
    obsidianwall --sepia 80 --vignette 30 --border-width 5 --border-color gold input.jpg
    ```
*   **Apply wave distortion, swirl, and compose:**
    ```bash
    obsidianwall --wave-amplitude 10 --wave-length 100 --swirl 45 input.jpg overlay.png
    ```
*   **Adjust brightness, contrast, and saturation:**
    ```bash
    obsidianwall --brightness 120 --contrast 110 --saturation 130 input.jpg overlay.png
    ```
