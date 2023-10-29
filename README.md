# Next.js 13 (App Directory)
reference: [Doc](https://nextjs.org/docs)

## Init
reference: [Doc](https://nextjs.org/docs/getting-started/installation)
```shell
npx create-next-app@latest
```

Result:
```shell
What is your project named? my-app
Would you like to use TypeScript? Yes
Would you like to use ESLint? Yes
Would you like to use Tailwind CSS? Yes
Would you like to use `src/` directory? Yes
Would you like to use App Router? (recommended) Yes
Would you like to customize the default import alias (@/*)? Yes
What import alias would you like configured? @/*
```

## Routing
reference: [Doc](https://nextjs.org/docs/app/building-your-application/routing)

 - ### Page [Doc](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#pages)
 - ### Layout [Doc](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#layouts)
 - ### Template [Doc](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#templates)
 - ### Group Route [Doc](https://nextjs.org/docs/app/building-your-application/routing/route-groups#examples)
 - ### Dynamic Route [Doc](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#example)
   - #### Multi Segments [Doc](https://nextjs.org/docs/app/building-your-application/routing/dynamic-routes#catch-all-segments)
 - ### Not Found [Doc](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)
 - ### Meta Data [Doc](https://nextjs.org/docs/app/api-reference/file-conventions/metadata)

## Typescript tsconfig alias

## Font

### Local [Doc](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#local-fonts)
```tsx
// src/app/layout.tsx


import localFont from 'next/font/local'
 
// Font files can be colocated inside of `app`
const myFont = localFont({
   src: './my-font.woff2',
   display: 'swap',
   variable: '--my-font',
})
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={myFont.variable}>
      <body>{children}</body>
    </html>
  )
}
```
### With Tailwind [Doc](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#with-tailwind-css)
```ts
// tailwind.config.ts


/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './src/app/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
     fontFamily: {
        sans: ['var(--my-font)'],
        serif: ['var(--my-font)'],
     },
  },
  plugins: [],
}
```

## Server vs. Client Components


## Next Themes
reference: [Doc](https://www.npmjs.com/package/next-themes)

```shell
npm install next-themes
```

with tailwind: [Doc](https://www.npmjs.com/package/next-themes#with-tailwind)
```ts
// tailwind.config.ts


module.exports = {
  darkMode: 'class'
}
```

use: [Doc](https://www.npmjs.com/package/next-themes#use)



