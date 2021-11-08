# Grid Generation
### Implementation
Size of table grid is taken from the sizes requested by individual cells. Containers perform the merge of individual sizes:
* For vertical-aligned container (cell column), all cell heights are summarized, width is calculated as an LCM (least common multiplier) of all cells widths.
* For horizontal-aligned container (cell row), cell widths are summarized, height is calculated as an LCM.

### Sample table
![Excel table.png](Excel%20table.png)

### User input
This is how user would describe the structure of the table above:
```rust
col![ { border = Heavy },
  textcell!("Request differences", { border = Heavy, pt_height = 3 }),
  row![
    col![ { border = Heavy },
      textcell!("Request via JSON", { border = Heavy }),
      textcell!("Easier object notation"),
      textcell!("Fast and asynchronous"),
      textcell!("Multiple methods available"),
    ],
    col![ { border = Heavy },
      textcell!("Request via HTML", { border = Heavy }),
      textcell!("Stricter structure"),
      textcell!("Synchronous"),
      textcell!("Only GET and POST"),
    ],
  ],
  textcell!("Benchmarks (50%, 90%, 99%)", { border = Heavy, pt_height = 3 }),
  row![
    textcell!("Frontend pre-\nprocessing"),
    col![
      textcell!("52 ms"),
      textcell!("88 ms"),
      textcell!("107 ms"),
    ],
    textcell!("Backend\nprocessing"),
    col![
      textcell!("300 ms"),
      textcell!("312 ms"),
      textcell!("380 ms"),
    ],
    textcell!("Frontend post-\nprocessing"),
    col![
      textcell!("115 ms"),
      textcell!("143 ms"),
      textcell!("170 ms"),
    ],
  ]
]
```

Now, breaking down individual containers:

```text
single text-cell - grid 1x1
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                                                                            ┃
┃                                    Request differences                                     ┃
┃                                                                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛

A column of 4 text-cells, 1x4                          A column of 4 text-cells, 1x4
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓ ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                  Request via JSON                  ┃ ┃           Request via HTML            ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫ ┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┫
┃               Easier object notation               ┃ ┃          Stricter structure           ┃
┠────────────────────────────────────────────────────┨ ┠───────────────────────────────────────┨
┃               Fast and asynchronous                ┃ ┃              Synchronous              ┃
┠────────────────────────────────────────────────────┨ ┠───────────────────────────────────────┨
┃             Multiple methods available             ┃ ┃           Only GET and POST           ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛ ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
These columns are merged into row - 2x4 to contain both


single text-cell - grid 1x1
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                                                                            ┃
┃                                 Benchmarks (50%, 90%, 99%)                                 ┃
┃                                                                                            ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛

 1x1                       1x3       1x1                 1x3     1x1                        1x3
┏━━━━━━━━━━━━━━━━━━━━━━━━┑┍━━━━━━━━┑┍━━━━━━━━━━━━━━━━━━┑┍━━━━━━┑┍━━━━━━━━━━━━━━━━━━━━━━━━━┑┍━━━━━━┓
┃                        ││ 52 ms  ││                  ││300 ms││                         ││115 ms┃
┃     Frontend pre-      │├────────┤│     Backend      │├──────┤│     Frontend post-      │├──────┨
┃       processing       ││ 88 ms  ││    processing    ││312 ms││       processing        ││143 ms┃
┃                        │├────────┤│                  │├──────┤│                         │├──────┨
┃                        ││ 107 ms ││                  ││380 ms││                         ││170 ms┃
┗━━━━━━━━━━━━━━━━━━━━━━━━┙┕━━━━━━━━┙┕━━━━━━━━━━━━━━━━━━┙┕━━━━━━┙┕━━━━━━━━━━━━━━━━━━━━━━━━━┙┕━━━━━━┛
Merging these cells into a single row gives us a 6x3 grid (LCM(1, 3) = 3)

```

Finally, merging all cells in the column to get the grid of final table:

```text
1x1
2x4
1x1
6x3

LCM(1, 2, 1, 6) = 6
SUM(1, 4, 1, 3) = 9
```

Total grid - 6x9 ! Same as in Excel table above.
