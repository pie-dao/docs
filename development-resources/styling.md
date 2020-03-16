# Styling

[https://github.com/pie-dao/tailwind/blob/master/src/utilities/content.js](https://github.com/pie-dao/tailwind/blob/master/src/utilities/content.js)CSS styling for PieDAO frontend apps uses [tailwindcss](https://tailwindcss.com/docs) as a framework. In addition to the default style options, which are searchable on the [excellent tailwindcss docs site](https://tailwindcss.com/docs), we have included several additional options outlined in this section.

## Screens \(@media breakpoints\)

We use the following media sizes for responsiveness.

* sm: 640px
* md: 768px
* lg: 960px
* xl: 1200px
* xxl: 1600px

When creating your base style, use a "smallest first" approach. Your style sheet should define how elements will look at 480px width \(the implied smallest width\). Individual styles can be overridden by using a [`@media <size> { ... }`](https://tailwindcss.com/docs/functions-and-directives/#screen) block. 

`/* Stylesheet */  
div.label {  
  @apply w-100pc bg-blue rounded;  
}  
  
@screen lg {  
  div.label {  
    @apply w-80px;  
  }  
}`

`<!-- DOM -->  
<div className="label">...</div>`\`\`\`

In the example above, the `div` would always have a blue background and rounded edges. It would be 100 percent width until the screen is 960px wide or more, at which point it would decrease to 80 percent width.

## **Style code style**

Convention is critical to maintaining a clean codebase. We ask you to follow the practices outline in this section is you write your CSS. 

**Soft tabs, two spaces**

![Richard Hendricks can fuck right off.](../.gitbook/assets/tabs.jpg)

**Wrap your components in a class named \`.&lt;component&gt;-container\`.** 

`const Footer = () => (  
  <div className="footer-container">  
    <!-- ... -->  
  </div>  
);`

**Group your styles by component**  
  
`/* Global */  
  
body { ... }  
h1 { ... }  
  
/* Header */  
  
.header-container { ... }  
.header-container .left { ... }  
.header-container .right { ... }  
  
/* Footer */  
  
.footer-container { ... }`  
  
**Prefer \`@apply\` over inline classes to achieve maximum separation of concerns.**  
We do not want a bunch of classes polluting the JSX files.

`<img src="/assets/img/aragon.svg" className="aragon lg:ml-20px lg:mt-0" />`

`.footer-container .aragon {  
  @apply h-30px mt-10px;  
}`

**You can override the default @media breakpoints when it makes sense, but use sparingly. When you do, use one override for an entire section of CSS.**

`/* good */  
@media (max-width: 800px) {  
  .footer-container {  
    flex-direction: column;  
  }  
  
  .footer-container .left {  
    flex-direction: column;  
  }  
  
  .footer-container .right {  
    flex-direction: column;  
  }  
}`

`/* bad */  
@media (max-width: 800px) {  
  .footer-container {  
    flex-direction: column;  
  }  
}  
  
/* ... other styles ... */  
  
@media (max-width: 800px) {  
  .footer-container .left {  
    flex-direction: column;  
  }  
}  
  
/* ... other styles ... */  
  
@media (max-width: 800px) {  
  .footer-container .right {  
    flex-direction: column;  
  }  
}`

## Colors

We have a core set of colors, several variants of grey, and a core set of label colors. We will be working to standardize these a bit further in the future. All of these colors are also available for [background](https://tailwindcss.com/docs/background-color/#app), [border](https://tailwindcss.com/docs/border-color/#app), [placeholder](https://tailwindcss.com/docs/placeholder-color/#app), and [text](https://tailwindcss.com/docs/text-color/#app) use.

### Core

`.text-black { color: #000000; }  
.text-blue { color: #7d78d1; }  
.text-green { color: #2db400; }  
.text-grey { color: #b7b7b7; }  
.text-pink { color: #f40a50; }  
.text-red { color: #ff0000; }  
.text-white { color: #ffffff; }`

### Greys

`.border-grey-10 { border-color: #0a0a0a; }  
.border-grey-51 { border-color: #333333; }  
.border-grey-115 { border-color: #737373; }  
.border-grey-204 { border-color: #cccccc; }  
.border-grey-243 { border-color: #f3f3f3; }  
.border-grey-246 { border-color: #f6f6f6; }`

### Labels

`.bg-label-blue { background-color: #305cee; }  
.bg-label-cyan { background-color: #1ec0ff; }  
.bg-label-gradient { background-color: linear-gradient(to right, #f10096 0%, #21d7ff 100%); }  
.bg-label-green { background-color: #2db400; }  
.bg-label-pink { background-color: #fc02a7; }  
.bg-label-purple { background-color: #9080dc; }  
.bg-label-red { background-color: #ff0053; }  
.bg-label-teal { background-color: #79f2c3; }  
.bg-label-yellow { background-color: #f8e71c; }  
.bg-label-yellow-alt { background-color: #ffcd1d; }`

## Spacing

In the default tailwindcss configuration, spacing scale is shared by [padding](https://tailwindcss.com/docs/padding), [margin](https://tailwindcss.com/docs/margin), [width](https://tailwindcss.com/docs/width), and [height](https://tailwindcss.com/docs/height). We extend that to include [min](https://tailwindcss.com/docs/min-width/#app) and [max](https://tailwindcss.com/docs/max-width/#app) width and [min](https://tailwindcss.com/docs/min-height/#app) and [max](https://tailwindcss.com/docs/max-height/#app) height.

### REM

REM spacing is relative to the font size of the html element \(which for most browsers is `16px` by default\). As tailwindcss mentions in their [section on the default spacing scale](https://tailwindcss.com/docs/customizing-spacing/#app), one spacing unit is equal to `0.25rem`, which will usually equal `4px`. Tailwind provides intermittent intervals up to `16rem`, we extend that to `60rem` and provide every increment.

`.h-0 { height: 0; }  
.h-1 { height: 0.25rem; }  
.h-2 { height: 0.5rem; }  
.h-3 { height: 0.75rem; }  
.h-4 { height: 1rem; }  
/* ... */  
.h-237 { height: 59.25rem; }  
.h-238 { height: 59.5rem; }  
.h-239 { height: 59.75rem; }  
.h-240 { height: 60rem; }`

### Pixel

Because REM spacing is relative, we also provide pixel values for more exact use cases. Specifically, whole number values from `1px` to `100px`, and by tens from `100px` to `1280px`.

`.w-1px { width: 1px; }  
.w-2px { width: 2px; }  
/* ... */  
.w-99px { width: 99px; }  
.w-100px { width: 100px; }  
.w-110px { width: 110px; }  
.w-120px { width: 120px; }  
/* ... */  
.w-1260px { width: 1260px; }  
.w-1270px { width: 1270px; }  
.w-1280px { width: 1280px; }`

### Percentage

Classes for values from `1%` to `100%` are also included.

`.p-1pc { padding: 1%; }  
.p-2pc { padding: 2%; }  
/* ... */  
.p-99pc { padding: 99%; }  
.p-100pc { padding: 100%; }`

## Components

### .btn

This class provides our standard button styling. It is not scoped to the `<button>` element. The full style provided can be seen at the [Github repo](https://github.com/pie-dao/tailwind/blob/master/src/components/btn.js) for [`@pie-dao/tailwind`](https://docs.piedao.org/development-resources/getting-started#pie-dao-tailwind).

![   standard                       disabled](../.gitbook/assets/btn.png)

## Utilities

These classes can be used with `@apply`. More details about utilities and creating your own can be found in the [tailwindcss docs](https://tailwindcss.com/docs/adding-new-utilities).

### .content

This class provides the core styles for a component's container. The full style provided can be seen at the [Github repo](https://github.com/pie-dao/tailwind/blob/master/src/utilities/content.js) for [`@pie-dao/tailwind`](https://docs.piedao.org/development-resources/getting-started#pie-dao-tailwind).

`.header-container {  
  @apply content font-secondary;  
}`

### .pointer

This class is the same as [`.cursor-pointer`](https://tailwindcss.com/docs/cursor/#app). We use it often enough that a shorthand version was desired.

`.pointer { cursor: pointer; }`

## Overridden tailwindcss classes

The following default tailwindcss classes have been overridden. You should not trust the official documentation for these sections and instead use the values below.

### Background color

See the [colors section](https://docs.piedao.org/development-resources/styling#colors) above.

### Border radius

We've overridden the defaults and added both pixel and percentage values. Pixel values are provided from 1px to 50px. Percentage values are provided from `1%` to `100%`.

#### Core

`.rounded-none { border-radius: 0; }  
.rounded-sm { border-radius: 1rem; }  
.rounded { border-radius: 2rem; }  
.rounded-md { border-radius: 2.5rem; }  
.rounded-lg { border-radius: 4rem; }`

#### Pixel

`.rounded-1px { border-radius: 1px; }  
.rounded-2px { border-radius: 2px; }  
/* ... */  
.rounded-49px { border-radius: 49px; }  
.rounded-50px { border-radius: 50px; }`

#### Percentage

`.rounded-1pc { border-radius: 1%; }  
.rounded-2pc { border-radius: 2%; }  
/* ... */  
.rounded-99pc { border-radius: 99%; }  
.rounded-100pc { border-radius: 100%; }`

#### Pixel

### Font family

There are two fonts, primary and secondary, that override the [font family configuration shown here](https://tailwindcss.com/docs/font-family/#app).

`.font-primary { font-family: Rubik, sans-serif; }  
.font-secondary { font-family: Roboto, monospace; }`

### Font size

There are nine default sizes and seven mobile sizes. We also provide view window sizes from values `1` to `20`.

#### Default

`.text-xxs { font-size: 0.8rem; }  
.text-xs { font-size: 0.83rem; }  
.text-sm { font-size: 0.9rem; }  
.text-md { font-size: 1rem; }  
.text-base { font-size: 1.1rem; }  
.text-lg { font-size: 1.5rem; }  
.text-xl { font-size: 2.3rem; }  
.text-xxl { font-size: 4rem; }  
.text-xxxl { font-size: 6rem; }  
.text-xxxxl { font-size: 10rem; }`

#### Mobile

`.text-m-xs { font-size: 0.5rem; }  
.text-m-sm { font-size: 0.6rem; }  
.text-m-lg { font-size: 1.3rem; }  
.text-m-xl { font-size: 2rem; }  
.text-m-xxl { font-size: 5rem; }  
.text-m-xxxl { font-size: 7rem; }`

#### View window

`.text-vw1 { font-size: 1vw; }  
.text-vw2 { font-size: 2vw; }  
.text-vw3 { font-size: 3vw; }  
.text-vw4 { font-size: 4vw; }  
.text-vw5 { font-size: 5vw; }  
.text-vw6 { font-size: 6vw; }  
.text-vw7 { font-size: 7vw; }  
.text-vw8 { font-size: 8vw; }  
.text-vw9 { font-size: 8vw; }  
.text-vw10 { font-size: 10vw; }  
.text-vw11 { font-size: 11vw; }  
.text-vw12 { font-size: 12vw; }  
.text-vw13 { font-size: 13vw; }  
.text-vw14 { font-size: 14vw; }  
.text-vw15 { font-size: 15vw; }  
.text-vw16 { font-size: 16vw; }  
.text-vw17 { font-size: 17vw; }  
.text-vw18 { font-size: 18vw; }  
.text-vw19 { font-size: 19vw; }  
.text-vw20 { font-size: 20vw; }`

### Font weight

We've replaced the default nine weights with only four.

`.font-thin { font-weight: 100; }  
.font-normal { font-weight: 300; }  
.font-bold { font-weight: 500; }  
.font-bolder { font-weight: 700; }`

### Opacity

Opacity classes are provided by tenths, leaving namespace room for custom values in the hundreths.

`.opacity-0 { opacity: 0; }  
.opacity-10 { opacity: 0.1; }  
.opacity-20 { opacity: 0.2; }  
.opacity-30 { opacity: 0.3; }  
.opacity-40 { opacity: 0.4; }  
.opacity-50 { opacity: 0.5; }  
.opacity-60 { opacity: 0.6; }  
.opacity-70 { opacity: 0.7; }  
.opacity-80 { opacity: 0.8; }  
.opacity-90 { opacity: 0.9; }  
.opacity-100 { opacity: 1; }`

## Extended tailwindcss classes

The following sections has additions beyond the tailwindcss default values. All of the defaults are included for these sections. The additions below are also available.

### Background size

Percentage classes from `1%` to `100%` have been added to the [three default background size classes](https://tailwindcss.com/docs/background-size/#app).

`.bg-1pc { background-size: 1%; }  
.bg-2pc { background-size: 2%; }  
/* ... */  
.bg-99pc { background-size: 99%; }  
.bg-100pc { background-size: 100%; }`

### Border width

The [default border width settings](https://tailwindcss.com/docs/border-width) provide classes for `2px`, `4px`, and `8px`. We added the missing ones in between. These additions exist for both the [all sides](https://tailwindcss.com/docs/border-width#all-sides) classes and the [individual sides](https://tailwindcss.com/docs/border-width#individual-sides) classes.

`.border-0 { border-width: 0; }  
.border { border-width: 1px; }  
.border-2 { border-width: 2px; }  
.border-3 { border-width: 3px; }  
.border-4 { border-width: 4px; }  
.border-5 { border-width: 5px; }  
.border-6 { border-width: 6px; }  
.border-7 { border-width: 7px; }  
.border-8 { border-width: 8px; }`

### Height

See the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above. Height also contains several classes beyond the ones provided by the spacing settings. You can view these on the [tailwindcss documentation](https://tailwindcss.com/docs/height/#app).

### Line height

In addition to the [tailwindcss default line heights](https://tailwindcss.com/docs/line-height/#app), we've included all REM values up to 60rem as described in the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above.

### Margin

See the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above.

### Max-Height

See the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above. Height also contains several classes beyond the ones provided by the spacing settings. You can view these on the [tailwindcss documentation](https://tailwindcss.com/docs/max-height/#app).

### Max-Width

See the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above. Height also contains many classes beyond the ones provided by the spacing settings. You can view these on the [tailwindcss documentation](https://tailwindcss.com/docs/max-width/#app).

### Min-Height

See the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above. Height also contains several classes beyond the ones provided by the spacing settings. You can view these on the [tailwindcss documentation](https://tailwindcss.com/docs/min-height/#app).

### Min-Width

See the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above. Height also contains several classes beyond the ones provided by the spacing settings. You can view these on the [tailwindcss documentation](https://tailwindcss.com/docs/min-width/#app).

### Padding

See the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above.

### Top / Right / Bottom / Left

We've added a single extra class to the [defaults provided by tailwindcss](https://tailwindcss.com/docs/top-right-bottom-left/#app).

`.inset-1/2 {  
  top: 50%;  
  right: 50%;  
  bottom: 50%;  
  left: 50%;  
}`

### Width

See the [spacing section](https://docs.piedao.org/development-resources/styling#spacing) above. Width also contains many class beyond the ones provided by the spacing settings. You can view these on the [tailwindcss documentation](https://tailwindcss.com/docs/width/#app).

