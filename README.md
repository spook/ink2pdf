# ink2pdf
Inkscape to Multi-Page PDF Document

**INITIAL PROJECT - NOT YET READY**

Generates a multi-page PDF document from your Inkscape document.
Includes other Inkscape layer tools:

-  ink2png   - Like ink2pdf but output a set of numbered PNG files
-  ink2jpg   - Like ink2pdf but output a set of numbered JPEG files
-  inklayers - List, show (set visible, unhide), and hide layers
-  inkpages  - Display the page structure in an Inkscape document.

## Multiple Pages.. In Inkscape?

Inkscape is a drawing tool, right?  

Yes, but it's much, much more.  It's one of my main CAD design tools;
I use it for architecture, drafting, plansets, landscape design, and
so on.  But it's always been missing one thing... multi-page support.

What to do? The trick may Inkscape users have figured out is to 
use layers as pages.  It's not the most elegant workaround but
it's relatively easy to do.  Except when it comes time to produce
a PDF document form all those layers.  Hence this tool: `ink2pdf`

## How's it work?

The trick-within-a-trick is to put a page number in each of
your layers.  For example, consider these layer names:

    p1) Introduction
    p2) The Problem
    p3) The Solution
    p4) Conclusion

The "p1)" is part of the layer name.  I call it the *page prefix*.
This tells the `ink2pdf` tool how to assign layers to the generated output PDF pages.
You, as the author of the Inkscape document, put that prefix into the layer's name.

The page numbers do not have to be in order, nor do they have to be sequential.
The following layer names produce the same PDF output as the first example above:

    p00) Introduction
    p50) The Solution
    p21) The Problem
    p999) Conclusion

The page prefixes in the layer names indicate the output page sequence, not the actual PDF page number.  
In the above example, p00 becomes output page number 1, p21 becomes page 2,
p50 becomes page 3, and p999 becomes page 4.

Layers without a page prefix are ignored.

### Background Layers

You can assign multiple layers to a page; and the same layer can 
be used on multiple pages:

    p*)        Background layer, goes on all pages
    p2-)       Applies to pages 2 and up
    p9-13)     Goes on pages 9 thru 11
    p-9)       Same as p1-9)
    p2,4,6-10) Only pages 2, 4, and 6 through 10
    p3.14)     Page numbers need not be integers
    p<)        Left-side (odd) pages
    p>)        Right-side (even) pages 

# More TBD...
