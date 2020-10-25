# ink2pdf
Inkscape to Multi-Page PDF Document

**INITIAL PROJECT - DRAFT - NOT YET READY**

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

What to do? The trick many Inkscape users have figured out is to 
use layers as pages.  It's not the most elegant workaround but
it *is* easy to do.  Except when it comes time to produce
a PDF document from all those layers -- hence this tool: `ink2pdf`

## How's it work?

### A Layer is a Page

By default, each layer in your Inkscape document becomes a page
in the PDF output.  You can also specify on the command line that
one of the layers is a background for all the other pages. 
For more detailed control over the layer-to-page mappings,
use *page prefixes*.

### Sublayers?

Inkscape sublayers are left as-is.  If the parent layer is mapped
to a page, then the sublayers, if any, are also mapped if they are
set visible in the Inkscape file.

### The Page Prefix

For more control over how pages are mapped from layers, use *page prefixes*.

This works by prefixing each layer's name with a page number, using a special format.
For example, consider these layer names:

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

### Repeated and Background Layers

You can assign multiple layers to a page; and the same layer can be used on multiple pages:

    p*)        Background layer, goes on all pages
    p2-)       Applies to pages 2 and up
    p9-13)     Used for pages 9 thru 13
    p-9)       Same as p1-9) but considered "incomplete"
    p2,4,6-10) Only pages 2, 4, and 6 through 10
    p3.14)     Page numbers need not be integers
    pL)        Left-side (odd) pages
    pR)        Right-side (even) pages 
    b5)        Background for page 5, *if* there's a page 5 - note the "b"
    
Layers with incomplete page prefixes, such as `p*)`, `p2-)`, or `p>` --
or backgrounded pages that use "b" instead of "p" as the first character --
won't be shown unless there's at least one other layer assigned
to a matching page.

### Conditional Tags

Layers may be tagged with short identifiers, called *conditional tags* (ctags).
When running `ink2pdf` the ctags may be specified with the `--tags` (-t) option.
Then layers are skipped if their ctags do not match.

Consider for example an Inkscape drawing that's a cartoon or graphic novel, 
and you want the related text to be in either English or German.
You can tag english layers with "en" and german layers with "de":

    p5) Superman Leaps Tall Bulding
    p5(en) Bubble Text
    p5(de) Bubble Text


## INSTALLATION

### Dependencies

* pdfunite
* Perl module XML::LibXML
* Perl module XML::LibXML::XPathContext

## ink2pdf Usage
```
ink2pdf [options] inkscape-file

-k, --keep           Don't unite the pages into one PDF; keep each page as it's own numbered .pdf file
-i, --insert N:file  Insert existing PDF file into document at page N; can use multiple times
-o, --output PDFFILE Specify the output PDF file name; default is to replace ".svg" suffix of input
                       file name with ".pdf"; if no ".svg" suffix then ".pdf" is appended.
-t, --tags TAGS      Conditional tags in effect; separate multiple tags with commas
```

## inklayers Usage
```
inklayers [options] inkscape-file

With no options, displays the layer names (the "labels") of all layers in the file.
Use --verbose (-v) to show even more information.

The --show LAYER option unhides the named layer, and hides all others unless the --keep option is also specified.
The --show option may be used multiple times to make multiple layers visible.
Use --show * to set all layers visible.

The --hide LAYER option hides the named layer.  This option may be repeated.

```

## inkpages Usage
```
inkpages [options] inkscape-file

Shows the page structure that ink2pdf would use when parsing the inkscape file.

```

## Credits

Thanx to https://graphicdesign.stackexchange.com/questions/54615/inkscape-scripting-how-to-show-hide-a-layer-and-export#54990 for the parsing hint.
