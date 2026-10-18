# 🌀 fract-ol

An interactive fractal explorer in C that renders the Mandelbrot and Julia sets with the MLX42 graphics library.

*This project was created as part of the 42 curriculum by jpflegha.*

<!-- add screenshot: fractol.png (Mandelbrot set) -->
<!-- add screenshot: fractol_julia.png (e.g. ./fractol julia -0.8 0.156) -->

## Description

fract-ol opens a 1000×800 window and computes every pixel of a fractal:

- Each pixel is mapped to a point on the complex plane (from −2 to +2 on both axes, scaled by the current zoom and shift).
- The program iterates **z = z² + c** up to 42 times (adjustable at runtime).
  - **Mandelbrot:** `z` starts at the pixel's point and `c` is the same point.
  - **Julia:** `z` starts at the pixel's point and `c` is a fixed value given on the command line.
- If |z|² grows larger than 4, the point escapes. Its color depends on how many iterations it took, using a smooth polynomial color gradient (`get_color` in `render.c`). Points that never escape are drawn white.

### Controls

| Input | Action |
|:---|:---|
| Mouse wheel | Zoom in / out at the mouse position |
| Arrow keys | Move the view |
| `+` / `-` (also keypad) | Increase / decrease the number of iterations by 10 (more detail vs. faster rendering) |
| `ESC` or closing the window | Clean up and quit |

## Key concepts

- Complex numbers and iterative functions
- Mapping between screen coordinates and the complex plane
- Zooming towards the mouse position (keeping the point under the cursor fixed)
- Writing pixels directly into an image buffer
- Event handling with hooks (keyboard, scroll, window close)
- Clean shutdown of graphics resources

## Build and run

Requirements: `cc`, `make`, `cmake`, `git` and the **GLFW** library (e.g. `sudo apt install libglfw3-dev` on Debian/Ubuntu). The Makefile clones [MLX42](https://github.com/codam-coding-college/MLX42) (v2.4.2) and my [libft](https://github.com/jpfleghaar/libft) on the first build.

```bash
git clone https://github.com/jpfleghaar/fract-ol.git
cd fract-ol
make
```

Run it:

```bash
./fractol mandelbrot
./fractol julia -0.8 0.156
./fractol julia 0.285 0.01
```

With wrong arguments, the program prints how to use it and exits with status 1:

```
$ ./fractol
Please enter
	"./fractol mandelbrot" or
	"./fractol julia <value_1> <value_2>"
```

Other Makefile targets: `make clean`, `make fclean` (also removes the cloned libraries), `make re`.

## Project structure

```
fract-ol/
├── Makefile       # clones and builds MLX42 and libft, then builds fractol
├── fractol.h      # structs (t_fractol, t_complex), constants, prototypes
├── main.c         # argument check, program start, close hook
├── init.c         # default values, window, image and hook setup
├── render.c       # per-pixel iteration, coloring, drawing
├── events.c       # keyboard and scroll handlers, cleanup
└── math_utils.c   # coordinate scaling, string to double conversion
```

## What I learned

- How a few lines of math (z² + c) create very complex images, and how the Mandelbrot and Julia sets are related.
- Converting between pixel coordinates and a mathematical coordinate system, including zoom and offset.
- Zooming at the mouse position needs the offset to be adjusted together with the zoom factor.
- Working with an external graphics library: its event loop, hooks, and releasing its resources correctly.
- The trade-off between detail (more iterations) and rendering speed.
