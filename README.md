# ITCSS Triangle

__________________________________
\\            Settings            /
 \\______________________________/
  \\           Tools            /
   \\__________________________/
    \\        Generic         /
     \\______________________/
      \\      Elements      /
       \\__________________/
        \\    Objects     /
         \\______________/
          \\ Components /
           \\__________/
            \\ Trumps /
             \\      /
              \\    /
               \\  /
                \\/

### ITCSS Layers
* __Settings__ — used with preprocessors and contain font, colors definitions, etc.
* __Tools__ — globally used mixins and functions. It’s important not to output any CSS in the first 2 layers.
* __Generic__ — reset and/or normalize styles, box-sizing definition, etc. This is the first layer which generates actual CSS.
* __Elements__ — styling for bare HTML elements (like H1, A, etc.). These come with default styling from the browser so we can redefine them here.
* __Objects__ — class-based selectors which define undecorated design patterns, for example media object known from OOCSS
* __Components__ — specific UI components. This is where majority of our work takes place and our UI components are often composed of Objects and Components
* __Trumps__ — utilities and helper classes with ability to override anything which goes before in the triangle, eg. hide helper class

### Partials
Each layer contains a series of partials using the naming convention \_<layer>.<partial>.scss (for example:
\_settings.colors.scss, \_elements.headings.scss, \_components.tabs.scss) and listing the imports for each layer in their respective \_index.scss.
We use the naming convention along with the folder tree to eliminate any uncertainty, and preclude the potential of an incorrectly located file.

These partials should be kept as small and granular as possible, with each one containing only as much CSS as it needs to
fulfill its role. So \_elements.headings.scss would contain only the rules h1 to h6 and nothing more. If you have, for example, a
Page Title component that makes a main heading (e.g. h1) and a subheading (e.g. h2) look a certain way, you would create a
\_components.page-title.scss partial in the Components layer and bind onto classes (e.g. .page-title, .page-title-sub), not onto
HTML elements.

This is how ITCSS works: we do not place all of our heading-related styles together. Instead, we place all of our element-
based rules together, and all of our class-based rules together. We're now ordering the project based on useful CSS metrics,
and not creating awkward specificity and cascade groupings by ordering the project in thematic chunks.

### Layer Ordering
For this approach to have the maximum amount of effectiveness, the layers must be imported in the order specified by the inverted
triangle.

We also ensure that each layer contains CSS of:
* A similar specificity: All element-based selectors, or all class-based selectors, or utility classes carrying !important
* A similar explicitness: Styling all your bare HTML elements, or styling UI components, or styling specific helper classes
* A similar reach: Ability to affect all of the DOM (e.g. * {}), a subset of the DOM (e.g. a {}), a section of the DOM (e.g.
.carousel {}) or a specific DOM node (e.g. .clear x {})

This drill-down approach gives us a much more manageable CSS architecture. Now we know that everything we add should
be an addition to whatever has gone before it. We know where each type of rule will live and where to put any new styles,
and we have the confidence that all our different selectors will play nicely alongside each other.
