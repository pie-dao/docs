# Styling

CSS styling for PieDAO frontend apps uses [tailwindcss](https://tailwindcss.com/docs) as a framework. In addition to the default style options, which are searchable on the [excellent tailwindcss docs site](https://tailwindcss.com/docs), we have included several additional options outlined in this section.

## @media breakpoints

We use the following media sizes for responsiveness.

* sm: 640px
* md: 768px
* lg: 960px
* xl: 1200px
* xxl: 1600px

When creating your base style, use a "smallest first" approach. Your style sheet should define how elements will look at 640px width. Individual styles can be overridden in the JSX DOM by prefixing a screen size to the class name. 

`/* Stylesheet */  
div.label {  
  @apply w-100pc bg-blue rounded;  
}`

`<!-- DOM -->  
<div className="label lg:w-80pc">...</div>`\`\`\`

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

## Overridden tailwindcss classes

The following default tailwindcss classes have been overridden. You should not trust the official documentation for these sections and instead use the values below.

### Border radius

We've overridden the defaults and added both pixel and percentage values. Pixel values are provided from 1px to 50px. Percentage values are provided from 1% to 100%.

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

There are 9 default sizes and 7 mobile sizes. We also provide view window sizes from values 1 to 20.

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

## Extended tailwindcss classes

The following sections has additions beyond the tailwindcss default values. All of the defaults are included for these sections. The additions below are also available.

### Background size

Percentage classes from 1% to 100% have been added to the [three default background size classes](https://tailwindcss.com/docs/background-size/#app).

`.bg-1pc { background-size: 1%; }  
.bg-2pc { background-size: 2%; }  
/* ... */  
.bg-99pc { background-size: 99%; }  
.bg-100pc { background-size: 100%; }`

