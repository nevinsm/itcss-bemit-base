# ITCSS Triangle
```
__________________________________
\            Settings            /
 \______________________________/
  \           Tools            /
   \__________________________/
    \        Generic         /
     \______________________/
      \      Elements      /
       \__________________/
        \    Objects     /
         \______________/
          \ Components /
           \__________/
            \ Trumps /
             \      /
              \    /
               \  /
                \/
```

### ITCSS Layers
* __Settings__ — used with preprocessors and contain font, colors definitions, etc.
* __Tools__ — globally used mixins and functions. It’s important not to output any CSS in the first 2 layers.
* __Generic__ — reset and/or normalize styles, box-sizing definition, etc. This is the first layer which generates actual CSS.
* __Elements__ — styling for bare HTML elements (like H1, A, etc.). These come with default styling from the browser so we can redefine them here.
* __Objects__ — class-based selectors which define undecorated design patterns, for example media object known from OOCSS
* __Components__ — specific UI components. This is where majority of our work takes place and our UI components are often composed of Objects and Components
* __Trumps__ — utilities and helper classes with ability to override anything which goes before in the triangle, eg. hide helper class

### Partials
Each layer contains a series of partials using the naming convention \_\<layer\>.\<partial\>.scss (for example:
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

## Naming Convention (BEMIT)
We are using a combination of BEM and the additional information structure of ITCSS to provide a class naming convention
that enables us to know where it applies in the cascade, and at what screen widths.

### BEM Basics
BEM (Block, Element, Modifier)

naming convention pattern:
```
.block {}
.block__element {}
.block--modifier {}
```
We use spinal-case e.g. this-awesome-name for multiple words in the block, element, or modifier name.

* `.block` — represents the higher level of an abstraction or component.
* `.block__element` — represents a descendent of `.block` that helps form `.block` as a whole.
* `.block--modifier` — represents a different state or version of `.block`.

for more information about BEM see http://getbem.com

### Informational Additions to BEM
In addition to the BEM naming convention we will also be adding prefixes and suffixes to the class names.

#### The Prefixes
In no particular order, here are the individual namespaces and a brief description.

* __o-__: Signify that something is an Object, and that it may be used in any number of unrelated contexts to the one you can currently see it in. Making modifications to these types of class could potentially have knock-on effects in a lot of other unrelated places. _Tread carefully_.
* __c-__: Signify that something is a Component. This is a concrete, implementation-specific piece of UI. All of the changes you make to its styles should be detectable in the context you’re currently looking at. Modifying these styles should be safe and have no side effects.
* __u-__: Signify that this class is a Utility class. It has a very specific role (often providing only one declaration) and should not be bound onto or changed. It can be reused and is not tied to any specific piece of UI.
* __t-__: Signify that a class is responsible for adding a Theme to a view. It lets us know that UI Components’ current cosmetic appearance may be due to the presence of a theme.
* __s-__: Signify that a class creates a new styling context or Scope. Similar to a Theme, but not necessarily cosmetic, these should be used sparingly — they can be open to abuse and lead to poor CSS if not used wisely.
* __is-,has-__: Signify that the piece of UI in question is currently styled a certain way because of a state or condition. This stateful namespace is gorgeous, and comes from __SMACSS__. It tells us that the DOM currently has a temporary, optional, or short-lived style applied to it due to a certain state being invoked.
* __\___: Signify that this class is the worst of the worst—a hack! Sometimes, although incredibly rarely, we need to add a class in our markup in order to force something to work. If we do this, we need to let others know that this class is less than ideal, and hopefully temporary (i.e. do not bind onto this). _For something to be considered a “hack” __it must apply its styles only to the browser(s) being targeted while all other browsers ignore it__._
* __js-__: Signify that this piece of the DOM has some behaviour acting upon it, and that JavaScript binds onto it to provide that behaviour. If you’re not a developer working with JavaScript, leave these well alone.
* __qa-__: Signify that a QA or Test Engineering team is running an automated UI test which needs to find or bind onto these parts of the DOM. Like the JavaScript namespace, this basically just reserves hooks in the DOM for non-CSS purposes.

#### Responsive Suffixes
We are using responsive suffixes preceeded with an \@ to indicate classes that only apply at a specific screen size via media queries

For example `.c-block\@sm` would indicate a reusable style that only applies on small screens, and we don't want to be global to that component (particularly useful when there are multiples of a component on a page, and only one needs tweaked at a specific resolution). We can also use `.c-block\@print` to indicate purely print related styles.

please note: the \@ symbol needs to be escaped in css `.prefix-block\@suffix` but not in the html `class="prefix-block@suffix"`
