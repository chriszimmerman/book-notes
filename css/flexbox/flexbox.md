# Flexbox

## [Flexbox defense](http://www.flexboxdefense.com/)

Flexbox containers have a main axis and a cross axis
- Main goes left and right
- Cross goes up and down

`display: flex;` for using flexbox

- `justify-content` positions a container's items along the main axis. Accepts these values:
    - `flex-start`: group at the start of the main axis
    - `flex-end`: group at the end of the main axis
    - `center`: group items in the center of the main axis
    - `space-between`: evenly distribute along main axis so that first item is at beginning and last item is at the end
    - `space-around`: evenly distribute along the main axis so that all items have equal space around them

- `align-items` positions a container's items along the cross axis. Accepts these values:
    - `flex-start`: align across the start of a container's cross axis
    - `flex-end`: align across the end of a container's cross axis
    - `center`: align items across the center of the cross axis

- `flex-direction` defines directional layout of the main and cross axes in the flex container. Accepts these values:
    - `row`: lay out the main axis from left to right
    - `row-reverse`: lay out the main axis from right to left
    - `column`: lay out the main axis from top to bottom
    - `column-reverse`: lay out the main axis from bottom to top

- `order` defines the order in which an item appears in the flex container. Accepts both positive and negative integer values. 0 is the default value for all items.

- `align-self`, which takes the same values as `align-items`, vertically positions elements.

## [Flexbox Froggy](https://flexboxfroggy.com)

- `flex-wrap`: allows wrapping of elements. Accepts these values:
    - `nowrap`: every item is fit to a single line
    - `wrap`: items wrap around to additional lines
    - `wrap-reverse`: items wrap around to additional lines in reverse

- `flex-flow`: shorthand property that combines `flex-direction` and `flex-wrap`
    - ex. `flex-flow: row wrap`

- `align-content`: set how multiple lines are spaced apart from each other. Takes the following values:
    - `flex-start` - packed at top of container
    - `flex-end` - packed at bottom of container
    - `center` - packed at vertical center of container
    - `space-between` - equal spacing between each line
    - `space-around` - equal spacing around each line
    - `stretch` - lines are streched to fit the container
