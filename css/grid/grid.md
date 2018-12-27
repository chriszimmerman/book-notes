# CSS Grid

## [Grid Garden](https://cssgridgarden.com)

`grid-column-start` - defines where to place a grid item
    - can use `span #` to define the column width relative to the end position
`grid-column-end` - allows an item to span multiple columns
    - can use `span #` to define the column width

`grid-column` - shorthand for `grid-column-*` properties above
    - ex. `grid-column: 2 / 4;`

`grid-row-start` - works like `grid-column-start`

`grid-row` - works like `grid-column`

`grid-area` - even more shorthand
    - order: `grid-row-start`, `grid-column-start`, `grid-row-end`, `grid-column-end`
    - example: `grid-area: 1 / 1 / 3 / 6;`

`order` - can order individual items
    - default value is 0

`grid-template-columns` - defines widths for columns in grid
    - can use `repeat` function
        - ex. `grid-template-columns: repeat(8, 12.5%)`
    - can use `fr` fractional unit
        - ex. `grid-template-columns: 1fr 5fr` - first column is 1/6 and second is 5/6 of grid

`grid-template-rows` - defines widths for rows in grid

`grid-template` - shorthand for `grid-template-columns` and `grid-template-rows`
