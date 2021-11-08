# Cell
Cell is a unit of the table which describes some piece of information by itself. The cell itself is not connected to the rendered table, it only contains the configuration from which cell should be dynamically rendered.

Cell is strongly connected to the table grid - it influences both size of the grid and size of individual grid pieces.

### Cell types
* Unit cell (text cell)
* Container with more cells inside of it
* Nested table

### Cell configuration
* Border type
* Content padding - inner offset from each side of the border
* Relative size in grid pieces
* Absolute size in symbols
* Content color