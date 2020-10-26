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
use *page prefixes* with the --prefixes (-x) option.

### Sublayers?

Inkscape sublayers are left as-is.  If the parent layer is mapped
to a page, then the sublayers, if any, are also mapped if they are
set visible in the Inkscape file.

### The Page Prefix

For more control over how pages are mapped from layers, use *page prefixes*.
Page prefixes are processed only in --prefixes (-x) mode.

This works by prefixing each layer's name with a *sequence number*, using a special format.
For example, consider these layer names:

    p1) Introduction
    p2) The Problem
    p3) The Solution
    p4) Conclusion

The "p1)" is part of the layer name.  I call it the *page prefix*.
This tells the `ink2pdf` tool how to assign layers to the generated output PDF pages.
You, as the author of the Inkscape document, put that prefix into the layer's name.

The numbers do not have to be in order, and there can be gaps in the sequence.
The following layer names produce the same PDF output as the first example above:

    p00) Introduction
    p50) The Solution
    p21) The Problem
    p999) Conclusion

The sequence numbers in the layer names indicate the output order, 
not the resultant PDF page number. 
In the above example, p00 becomes output page number 1, p21 becomes page 2,
p50 becomes page 3, and p999 becomes page 4.

Layers without a page prefix are ignored.

### Content Layers and Background Layers

Depending upon how the prefix is specified, the layer is a *content* layer 
or a *background* layer.

The layers in your Inkscape document are usually content layers.
Content layers begin with the letter 'p' or 'c' (case insensitive).
The number(s) must be specified explicitely; ranges or left/right
indications cannot be used.  These are valid prefixes for content layers:

    p1)       Normal way to indicate page 1
    p003)     Another way to indicate page 3
    p4,8)     This layer is assigned to two pages
    p10.5)    Decimal values allowed; easy way to insert a forgotten page

Background layers begin with the letter 'p' or 'b'.
They can have a single number like a content page, or a list of numbers,
or they can indicate a range (2-5), or they can be a special character as
follows:

    b1)     Background for page 1 - note the "b"
    b4,8)   Background layer for pages 4 and 8 - lists are allowed - note the "b"
    p2-10)  Background for pages 2 thru 10 inclusive.  The use of a range forces this to be a background layer.
    p2-)    Background for page 2 and up.
    p-50)   Background for all pages up to and including 50
    p*)     Background layer for all pages
    pL)     Background layer for left-side (even) output pages - does not include page 1 (intentional)
    pR)     Background layer for right-side (odd) output pages
    
A background layer is used with a content layer; if there's no matching
content layer then the background layer is ignored.

### Conditional Tags

Layers may be tagged with short identifiers, called *conditional tags* (ctags).
When running `ink2pdf` the ctags may be specified with the `--tags` (-t) option.
Ctags are used to enable and disable layers on output.  If a layer has a ctag,
but the tag is not specified in the `--tags` option, then that layer is ignored.

Consider for example an Inkscape drawing that's a cartoon or graphic novel, 
and you want the related text to be in either English or German.
You can tag english layers with "en" and german layers with "de":

    p5) Superman Leaps Tall Bulding
    p5(en) Bubble Text
    p5(de) Bubble Text

CTags are short simple identifiers.  They must consist of only letters, digits, and the dash.
Multiple ctags are allowed in the prefix:

    p102(en,fr) Superman meets Lois

Conditional tags are processed only in --prefixes (-x) mode.

## INSTALLATION

### Prerequisites

* pdfunite
* Perl module XML::LibXML
* Perl module XML::LibXML::XPathContext

## ink2pdf Usage
```
     ink2pdf [options] inkscape-file

     Options:
      -b  --background L   Use layer L as a backround layer (cannot use with -x)
      -h  --help           Usage summary
      -i  --insert N:file  Insert existing PDF file into document at page N; 
                             can use multiple times
      -k  --keep           Don't unite the pages into one PDF; keep each page 
                             as it's own numbered .pdf file
      -l  --show-layers    Show layer names and exit
      -o  --output PDFFILE Specify the output PDF file name; default is to 
                             replace the ".svg" suffix of the input file name 
                             with ".pdf"; if no ".svg" suffix then ".pdf" is 
                             appended.
      -t  --tags TAGS      Conditional tags in effect; separate tags with commas
      -x  --prefixes       Parse prefixes in layer names
      -v  --verbose        Verbose mode
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
