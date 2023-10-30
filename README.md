# Next.js 13 (App Directory)

reference: [Doc](https://nextjs.org/docs)

## table of contents
* [Init](#init)
* [Routing](#routing)
* [Typescript tsconfig alias](#typescript-tsconfig-alias)
* [Font](#font)
   + [Local](#local)
   + [With Tailwind](#with-tailwind)
* [Server vs. Client Components](#server-vs-client-components)
* [Next Themes](#next-themes)
* [Next Internationalization](#next-internationalization)
   + [In Client Components:](#in-client-components)
   + [Navigation](#navigation)
* [Storybook](#storybook)

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
 - ### Private Folder [Doc](https://nextjs.org/docs/app/building-your-application/routing/colocation#private-folders)
 - ### Not Found [Doc](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)
 - ### Meta Data [Doc](https://nextjs.org/docs/app/api-reference/file-conventions/metadata)

## Typescript tsconfig alias

```json
{
   "compilerOptions": {
      "paths": {
         "@/components/*": ["./src/app/_components/*"],
         "@/layouts/*": ["./src/app/_layouts/*"]
      }
   }
}
```

## Font

### Local

reference: [Doc](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#local-fonts)

In `src/app/layout.tsx`
```tsx
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
### With Tailwind

reference: [Doc](https://nextjs.org/docs/app/building-your-application/optimizing/fonts#with-tailwind-css)

In `tailwind.config.ts`
```ts

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './src/app/**/*.{js,ts,jsx,tsx}',
  ],
  theme: {
     fontFamily: {
        sans: ['var(--my-font)'],
     },
  },
  plugins: [],
}
```

## Server vs. Client Components
YouTube Video with example:

[![Watch the video](https://img.youtube.com/vi/3Dw6D_WuzSE/maxresdefault.jpg)](https://youtu.be/3Dw6D_WuzSE)


## Next Themes

reference: [Doc](https://www.npmjs.com/package/next-themes)

```shell
npm install next-themes
```

with tailwind: [Doc](https://www.npmjs.com/package/next-themes#with-tailwind)

In `tailwind.config.ts`
```ts
module.exports = {
  darkMode: 'class'
}
```

In `src/app/_layouts/ProvidersLayout/ProvidersLayout.tsx`
```tsx
'use client'

import { ThemeProvider } from 'next-themes'
import { FC, PropsWithChildren } from 'react'

const ProviderLayout: FC<PropsWithChildren> = ({ children }) => {
  return (
    <ThemeProvider enableSystem={false} attribute="class">
      {children}
    </ThemeProvider>
  )
}

export default ProviderLayout
```

In `src/app/_components/ThemeChanger/ThemeChanger.tsx`
```tsx
'use client'

import { useTheme } from 'next-themes'
import { FC, useEffect, useState } from 'react'

const ThemeChanger: FC = () => {
  const { theme, setTheme } = useTheme()

   {/** Just Run In Client */}
  const [isInClient, setIsInClient] = useState(false)
  useEffect(() => {
    setIsInClient(true)
  }, [])

  const toggle = () => {
    setTheme(theme === 'light' ? 'dark' : 'light')
  }
  
  if (!isInClient) return null

  return (
    <div>
       <button onClick={toggle}>
          {theme}
       </button>
    </div>
  )
}

export default ThemeChanger
```

In `src/app/layout.tsx`
```tsx
import ProviderLayout from '@layouts/ProvidersLayout/ProvidersLayout' // <== Add This
import ThemeChanger from '@components/ThemeChanger/ThemeChanger' // <== Add This

// ...

const RootLayout: FC = () => {
  
  // ...
  
  return (
     <html className={myFont.variable}>
        <body>
           <ProviderLayout> {/** <== Add This */}
              <ThemeChanger /> {/** <== Add This */}
              {children}
           </ProviderLayout> {/** <== Add This */}
        </body>
     </html>
  )
}

export default RootLayout
```

## Next Internationalization

reference: [Doc](https://next-intl-docs.vercel.app/docs/getting-started/app-router-server-components)

```shell
npm install next-intl@3.0.0-beta.19
```

In `src/app/_messages/en/general.ts`
```ts
const message = {
  title: 'Hello world!',
  hello: 'Hello {name}!',
  cardinal:
    'You have {count, plural, =0 {no followers yet} =1 {one follower} other {# followers}}.',
  richText: 'This is <important><very>very</very> important</important>',
}

export default message
```

In `src/app/_messages/fa/general.ts`
```ts
const message = {
   title: 'سلام دنیا!',
   hello: 'سلام {name}!',
   cardinal:
           'شما {count, plural, =0 {هیج فالوری ندارید} =1 {فقط یک فالور دارید} other {# فالور دارید}}.',
   richText: 'این <important><very>خیلی</very> مهمه</important>',
}

export default message
```

In `src/app/_messages/en.ts`
```ts
export { default as general } from './en/general'
```

In `src/app/_messages/fa.ts`
```ts
export { default as general } from './fa/general'
```

In `i18n.ts`
```ts
import { getRequestConfig } from 'next-intl/server'

export default getRequestConfig(async ({ locale }) => {
  return {
    messages: await import(`./src/app/_messages/${locale}.ts`),
  }
})
```

In `next.config.js`
```ts
/** @type {import('next').NextConfig} */
const nextConfig = {}

const withNextIntl = require('next-intl/plugin')('./i18n.ts')

module.exports = withNextIntl(nextConfig)
```

In `src/middleware.ts`
```ts
import createMiddleware from 'next-intl/middleware'

export default createMiddleware({
  locales: ['fa', 'en'],
  defaultLocale: 'fa',
})

export const config = {
  matcher: ['/((?!api|_next|_vercel|.*\\..*).*)'],
}
```

In `src/app/_components/LanguageChanger/LanguageChanger.tsx`
```tsx
'use client'

import { useLocale } from 'next-intl'
import { usePathname, useRouter } from 'next-intl/client'
import { FC } from 'react'

const LanguageChanger: FC = () => {
   const router = useRouter()
   const pathname = usePathname()
   const locale = useLocale()

   const toggle = () => {
      router.replace(pathname, { locale: locale === 'en' ? 'fa' : 'en' })
   }

   return (
           <div>
              <button onClick={toggle}>{locale === 'en' ? 'English' : 'فارسی'}</button>
           </div>
   )
}

export default LanguageChanger
```

Move `layout.tsx` and `page.tsx` from `src/app` to `(routes)/[locale]/`

In `src/app/(routes)/[locale]/layout.tsx`
```tsx
// ...
import ThemeChanger from '@components/LanguageChanger/LanguageChanger' // <== Add This
import { FC, PropsWithChildren } from 'react' // <== Edit This
import { notFound } from 'next/navigation' // <== Add This

const locales = ['fa', 'en'] // <== Add This

// ...

interface IRootLayoutProps extends PropsWithChildren<{ params: { locale: string } }> {} // <== Add This

const RootLayout: FC<IRootLayoutProps> = ({ children, params: { locale } }) => { // <== Edit This
  // Add This
  if (!locales.includes(locale)) notFound()
  const isRtlLang = locale === 'fa'

  return (
    <html lang={locale} className={myFont.variable}> {/** <== Edit This */}
      <body dir={isRtlLang ? 'rtl' : 'ltr'}> {/** <== Edit This */}
         <ProviderLayout>
            <LanguageChanger /> {/** <== Add This */}
            <ThemeChanger />
            {children}
         </ProviderLayout>
      </body>
    </html>
  )
}

export default RootLayout
```

In `src/app/(routes)/[locale]/lang/page.tsx`
```tsx
import { useTranslations } from 'next-intl'
import { FC } from 'react'

const LangPage: FC = () => {
  const t = useTranslations()
  const tGeneral = useTranslations('general')

  return (
    <>
      <p>{t('general.title')}</p>
      <p>{tGeneral('title')}</p>
      <p>{tGeneral('hello', { name: 'Masoud' })}</p>
      <p>{tGeneral('cardinal', { count: 0 })}</p>
      <p>{tGeneral('cardinal', { count: 1 })}</p>
      <p>{tGeneral('cardinal', { count: 3580 })}</p>
      <p>
        {tGeneral.rich('richText', {
          important: (chunks) => <b>{chunks}</b>,
          very: (chunks) => <i>{chunks}</i>,
        })}
      </p>
    </>
  )
}

export default LangPage
```

### In Client Components:

reference: [Doc](https://next-intl-docs.vercel.app/docs/environments/server-client-components)

### Navigation

reference: [Doc](https://next-intl-docs.vercel.app/docs/routing/navigation)

example: `src/app/_components/LanguageChanger/LanguageChanger.tsx`

## Storybook

---> TODO <---
