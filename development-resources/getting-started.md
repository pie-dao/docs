---
description: >-
  The following pages attempt to give you all you need to get started building
  on top of Pie Network products. If you have any questions, please visit the
  PieDAO Discord or the Forums.
---

# Getting Started

{% hint style="info" %}
This section of the docs is evolving rapidly. Please consider visiting us on [Discord](https://discord.gg/QHBTnpt) or the [Forums](https://forum.piedao.org) if you want to get involved in PieDAO development.
{% endhint %}

## NPM Packages

All PieDAO NPM packages are publishing on [npmjs.com](https://npmjs.com) under the `@pie-dao` prefix. [You can view the most current ones here](https://www.npmjs.com/search?q=%40pie-dao).

### @pie-dao/tailwind

This package provides the [tailwindcss](https://tailwindcss.com) plugin used by the PieDAO [frontend-template repo](https://github.com/pie-dao/frontend-template). Including it in your tailwindcss configuration will give you all of the base styles and classes discussed later on in the [styling section](https://docs.piedao.org/development-resources/styling).

#### Installation

`yarn add @pie-dao/tailwind`

`/* tailwind.config.js */  
const piedao = require('@pie-dao/tailwind');  
  
module.exports = {  
  theme: {},  
  variants: {},  
  plugins: [piedao],  
}`

\`\`

