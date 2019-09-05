[Home](./index.md) / css-grid

# CSS Grid Notes

  - [Learning Resources](#learning-resources)
  - [About](#about)
  - [Important Terminology](#important-terminology)
  - [Codepen demos](#codepen-demos)
## Learning Resources
1. [CSS Grid Introduction-Css Tricks](https://css-tricks.com/snippets/css/complete-guide-grid/#grid-introduction)

## About
CSS grid is a two dimensional model to design layouts and while flex box is a one dimensional model.

## Important Terminology
- **Grid Container** - The element on which `display:grid` property is applied. It's *direct* parent of all the grid items.
- **Grid Item** - The children of grid container.
- **Grid line** - The dividing line that makes the structure of the grid
  + **column grid lines** - vertical grid lines
  + **row grid lines** - horizontal grid lines
- **Grid track** - The space between two *adjacent* grid lines.
- **Grid cell** - The space between two adjacent rows and two adjacent column grid lines. It's a single unit of the grid.
- **Grid area** - The total area surrounded by four grid lines.

## Codepen demos
1. Simple Implementation
  <p class="codepen" data-height="497" data-theme-id="dark" data-default-tab="css,result" data-user="aditya81070" data-slug-hash="ZEzvWmo" data-preview="true" style="height: 497px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="CSS Grid">
    <span>See the Pen <a href="https://codepen.io/aditya81070/pen/ZEzvWmo/">
    CSS Grid</a> by Aditya Agarwal (<a href="https://codepen.io/aditya81070">@aditya81070</a>)
    on <a href="https://codepen.io">CodePen</a>.</span>
  </p>
  <script async src="https://static.codepen.io/assets/embed/ei.js"></script>