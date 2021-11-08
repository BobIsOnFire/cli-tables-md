# Grid
Grid is a set of constraints that each cell should use during render. Grid allows the cells in table to be aligned with each other, set relative size, allows to add headers, footers and other elements which differ dynamic table from a static collection of cells.

The closest example could be the grid in Excel. Considered such table:

![Excel table.png](Excel%20table.png)

Cells in this table have different relative and absolute sizes, but they still need to be aligned with each other. Excel has a grid for its tables - vertical constraints are represented with capital letters, horizontal constraints - with numbers.

Each cell knows exactly on which pieces of grid it is located, and is rendered depending on the sizes of those pieces.

In case of Excel, grid is a prerequisite and table is created manually on top of that grid. In case of our cli-tables, we want to generate a grid automatically depending on the structure of the table specified by user.
