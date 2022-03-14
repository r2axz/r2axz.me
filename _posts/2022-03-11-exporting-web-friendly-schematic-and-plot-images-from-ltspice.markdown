---
layout: single
title:  "Exporting Images From LTspice"
date:   2022-03-07 11:00:00 +0300
categories: electronics
---

As soon as I started this blog, I quickly realized I needed to find a way to add
LTspice schematics and plots to my posts. LTspice itself has extremely limited
capabilities in this regard. Basically, the only thing you can do within the app
is to copy bitmaps to the clipboard, and even that is not available on macOS
where I use it.

Of course, I can make screenshots using the built-in OS tools, but it's still a
mess. First, I need to make sure screenshots have the same size. Second, the
default LTspice color scheme looks archaic and isn't really compatible with
pretty much anything. Third, I don't think bitmaps is a way to go anyway. Not
only they are relatively large, but also they are a huge pain when it comes to
responsiveness. So, no to bitmaps! But is there any other option left? Turns out
there is!

It is possible to convert the LTspice schematic or plot to an SVG vector image
within a couple of minutes.

We'll need:

- LTSpice :)
- A PDF Printer;
- Adobe Illustrator or similar software;

Printing to PDF is available by default in macOS, but other operating systems
may require additional software. Google suggests there are plenty of solutions
for that.

I use Adobe Illustrator not because of its special features but because I have
it installed on my computer. I'm sure any other vector editor will do the job
equally well.

#### Exporting Schematics

1. Open your schematic in LTspice and hit Space so that the schematic fits the
   entire window (the window size does not matter);
2. Click **Ctrl/Command-P** to bring up the printing dialog;
3. From within the printing dialog, save your schematic to a PDF file;
4. Open the PDF file in Adobe Illustrator;
5. Open the Layers tab, unfold **Layer 1** and **Clip Group**;
6. Select the **Clipping Path** element and delete it;
7. Optionally, select All and click **Edit --> Edit Colors --> Convert to
   Grayscale** to discard color information;
8. Click **Object --> Artboards --> Fit to Artwork Bounds**;
9. Click **File --> Save As** and save the file as SVG. Make sure **Responsive** is
   checked in the SVG options;

Enjoy the result!

![Exported SVG](/assets/2022-03-11-exporting-web-friendly-schematic-and-plot-images-from-ltspice/LTC6991_F14.svg){: .post-figure }
*Figure 1*

#### Exporting Plots

Exporting plots is a little bit more involved. LTspice always tries to fill the
entire available media height when printing plots, so the plots look stretched
in the vertical direction. I didn't find any way around that. Fortunately, it is
possible to adjust the plots aspect ratio in Illustrator.

1. Run the simulation and open the plot window;
2. Depending on the plot, you may want to lower the vertical ticks count by
   adjusting the vertical axis settings (right-click on the axis, adjust
   **Ticks**);
3. Click **Ctrl/Command-P** to bring up the printing dialog and save the file as
   PDF. At this point, it'll look really stretched. No worries, we'll fix that;
4. Open the file in Illustrator, make sure the **Selection Tool (V)** is active,
   select All, and drag any corner of the outermost path to set the desired
   aspect ratio. Alternatively, you may set the exact dimensions using the
   **Properties** panel on the right;
5. Click **Select --> Objects --> All Text Text Objects**;
6. Click **Character** in the upper toolbar to bring up the character panel;
7. Inside the **Character** panel, copy the horizontal scale value (on the
   right) to the vertical scale value (on the left) to restore text proportions.
   As a result, both values should be greater than 100%;
8. Click **Object --> Artboards --> Fit to Artwork Bounds**;
9. If you see any clipping issues with text in the plot legend, open the Layers
   tab and delete **Clipping Path** of every clip group;
10. If you need to restore the default black background, add a new layer
    underneath **Layer 1** and create a black rectangle with the dimensions of
    the plot. Personally, I don't bother with that and just use CSS
    ```background-color: black``` for the plot image CSS class;
11. Save the image as SVG;

![Exported SVG](/assets/2022-03-11-exporting-web-friendly-schematic-and-plot-images-from-ltspice/LTC6991_F14_plot.svg){: .post-figure .ltspice-plot}
*Figure 2*

Although my method of exporting images from LTspice requires some manual work,
it gives me by far the best end result and overall control over the export
process. For the sake of completeness, I would also like to mention that I found
two open source solutions for this task.

The first one is [ltspice2svg](https://pypi.org/project/ltspice2svg/), which seemed to work on
a basic level but could not export parasitic parameters of a voltage source in
my schematic. Also, it does not support plots.

The second one is [lt2circuitikz](https://github.com/ckuhlmann/lt2circuitikz),
which actually exports to LaTeX and generates pgf/TikZ picture of the schematic,
using the CircuiTikZ package to draw the symbols. Again, not exactly what I want
and missing support for plots.

Do you have your own favorite way of exporting images from LTspice? Please share
a comment if you do.
