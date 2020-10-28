# TO DO

## Under Consideration

* --pages -p M:N
Add an option that selects a subset of the output pages.
Page generation is slow; if the user wants only certain
pages then this would make it faster.
Consider tho the interaction with the --insert option;
the -p pages get chopped *before* inserting so the -i numbers
would be off.  Unless I get clever and associate the original
page numbers with the @pdf_files list (use a hashmap).  Hmmm....

* --generator -g GENUTIL
Default generator is `inkscape`.  It's good but slow.
Allow others, such as rsvg-convert?
Do this in place of a -I option that gives the full path to
the Inkscape binary?

* Windows install
Yuck... Winderz.  Need instructions to make this work on
Windows, perhaps a VM to test it too.  What about MacOS too?

## Yeah, do it...

* Other output formats: 
PNG, JPG, PS etc.  As a collection of single page images.
Also include option to tar/zip them together?

## What I really want...

Is to build this multi-page support into Inkscape itself.
I'm able to build Inkscape 1.* from source already,
and I've played around with duplicating/changing the Layers
dialog box, but my new UI widgets don't appear.
I need help getting over the hump here...


