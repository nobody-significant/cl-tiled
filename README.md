# cl-tiled

[Tiled](http://www.mapeditor.org) TMX/TSX and JSON map/tileset loader

## About

**cl-tiled** is a Common Lisp library for importing [TMX/TSX](http://doc.mapeditor.org/reference/tmx-map-format/) and [JSON](https://github.com/bjorn/tiled/wiki/JSON-Map-Format) tilemaps and tilesets generated by [Tiled](http://www.mapeditor.org).
It aims to fill the same role as [other](https://github.com/marshallward/TiledSharp) [engine-agnostic](https://github.com/bitcraft/PyTMX) Tiled [importers](https://github.com/baylej/tmx/). Meaning it is not a renderer nor does it provide integration with renderers on its own. Instead it aims to provide an easy, logical way to read map data so it may be imported/rendered in whatever way you wish.

## Status

alpha quality. API changes to come. Mostly additions to missing features.

Note that as a current goal, this library aims to be feature complete, not fast nor space efficient.

Current:

* Full TMX/TSX and JSON reading support
* Support for embedded and external tilesets
* Support for embededd and external images
* API support for all layer, tile, object, and terrain types
* Full support for property objects with distinct data-types (string, number, bool, color, pathname)
* Orthogonal map support

Planned:

* Defining API for isometric, staggred, and hexagonal maps
* Make the library more efficient
* Modifying map and writing TMX/TSX and JSON files (if enough demand for this)

Please post any requests/bugs on the issues page.

## Dependencies

* [alexandria](https://gitlab.common-lisp.net/alexandria/alexandria) - general utilities
* [asdf](https://common-lisp.net/project/asdf/) - building
* [chipz](https://github.com/froydnj/chipz) - decompressing tileset and image data
* [cl-base64](http://quickdocs.org/cl-base64/api) - Decode base64 binary data in XML text
* [cl-json](https://github.com/hankhero/cl-json) - JSON parser
* [nibbles](https://github.com/froydnj/nibbles) - Decode encoded/compressed tile data
* [parse-float](https://github.com/soemraws/parse-float) - parsing floats (xml data)
* [split-sequence](http://cliki.net/split-sequence) - split sequences (CSV type XML data)
* [uiop](https://github.com/fare/asdf/tree/master/uiop) - pathname handling
* [xmls](https://www.common-lisp.net/project/xmls/) - XML parser

## Example

``` common-lisp
(use-package #:cl-tiled)

(defgeneric render-layer (layer))

(defmethod render-layer ((layer tile-layer))
  (dolist (cell (layer-cells layer))
    ;; cell-x and cell-y for pixel positions
    ;; cell-tile for `tile' information
    ;;  what image, which row/column within image
    ))

(defmethod render-layer ((layer image-layer))
  ;;layer-image gets you the relevant image to render
  )

(defmethod render-layer ((layer object-layer))
  (dolist (object (object-group-objects layer))
    ;; render each object according to type
    ))

(defmethod render-layer ((layer group-layer))
  ;;Render each sub-layer
  (dolist (layer (group-layers layer))
    (render-layer layer)))

(let ((tiled-map (load-map "assets/map.tmx")))
  (dolist (layer (map-layers tiled-map))
    (render-layer layer)))

```

## Contact

Wilfredo Velázquez-Rodríguez <zulu.inuoe@gmail.com>