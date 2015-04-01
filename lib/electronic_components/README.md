# Electronic Components OpenSCAD Library

**This is the section for electronic components in scadlib.**

Note for fzz2scad users: The first part of this file is kept generic
for modules for all applications. fzz2scad specific information is in
the second part of this file.

The structure of this library sticks to the classification of electronic
components in [http://en.wikipedia.org/wiki/Electronic_component](http://en.wikipedia.org/wiki/Electronic_component)

Arduinos et al. can't be classified as components. Instead they are
classified as [modules](http://en.wikipedia.org/wiki/Computer_module).

# Standards for Parts

## Meta Data
Of course the [standards for meta data](../../SCADDOC.md) apply.

Never forget the `-dependency`, `param` and `return` tags (if appropriate)!

Remember to use @adopt as this may save LOTS of code.

## electronic_component specific tags
These are the tags that should be used for electronic components.

### `@source`
Links to the data that was used to create this model.

  * Datasheets
  * Fritzing Parts
  * EAGLE Files
  * Websites
  * etc.

### `@manufacturer`
The name of the manufacturer.
(Especially interesting if there is only one.)

### `@manufacturer-product`
The "catalog" name the manufacturer uses to identify this product.
(Especially interesting if there is only one manufacturer.)

### `@note: fzz2scad-compatible, see https://github.com/htho/fzz2scad`
Gives a hint that this module is fzz2scad compatible. Anyway,
"fzz2scad-compatible" should be in the `tag-list`.

### A note on `@category-list`
It is probably the best to derive the categories from the classification
(which also determines the path to the file in the library)

Note that the path uses the plural of the classification word, while
the category is singular ("led" vs. "leds").

#### Special tags for the tag-list

##### `fzz2scad-compatible`
Part is designed in a way that it can be used with fzz2scad

##### `hq`
A high quality model that is as detailed as possible.

##### `lq`
A low quality model that gives an idea of what the part may look like.

##### `do`
A model that only defines the space above a part that is needed for a
drill in a casing so we may access that part.

##### `proto`
This module is a prototype that will be used by modules to create models.


# fzz2scad specific remarks

## Requirements for modules
  1. modules have their x and y origin in `connector0`
  (The center of `connector0` is  `x = 0` and `y = 0`)
  2. modules have their z origin on the bottom of the part.
  That is where the part (almost) touches the PCB.  
  If the distance is variable, the module has a `distanceFromPCB` parameter. 
  2. modules have the same orientation as the parts in Fritzing

### Reasons for the requirements
#### Origin in `connector0`
This sounds a little odd at first. But one has to know, that coordinates
in Fritzing are unintuitive. This counts at least for the coordinates
that are shown in the right panel and stored in the fz file.
These coordinates point at the edges of the SVG-Image that contains
silkscreen and the connector pads. This is odd when modeling a part
using a datasheet.

An Example:
An standard 5mm LED like [this](https://cdn-reichelt.de/documents/datenblatt/A500/LED_5MM_GE.pdf) 
has a diameter of 5mm (obviously). Where? At the Part with the dome!

There is a socket below that. It has a diameter of 5.8mm and is flattened
on the side with the cathode down to the main cylinder.

As the the first maximal dimension is 5.8mm and the second maximal 
dimension is 5.4mm, one might think the module should have a ground area
of 5.8mmm times 5.4mm. The truth is: Fritzings part has a ground area of
6.20014mm times 6.20014mm (=0.2441in according to [`svg.pcb.5mm_LED.svg`](https://github.com/fritzing/fritzing-parts/blob/master/svg/core/pcb/5mm_LED.svg))

BUT: The LED isn't even centered on the image! It is rotated so the
flattened side is on the upper side of the image and at the edge of the
canvas. There is a lot of Space below the LED.

*Connectors are universal!*  
When modeling a part using the datasheet, the positions of the connectors
are at hand, size of the svg and position on it are not. So we use the
connectors as common ground.

#### Rotations
Parts are most likely modeled with the datasheet in hand.
A certain orientation seems natural. But the parts in Fritzing might have
a different orientation.
(I modeled the LED from the example above with the flattened side
on the right. The Fritzing part has the flattened side on top.)
To get the default orientation simply drag the part on the PCB in Fritzing.
Use the same orientation for your part.
