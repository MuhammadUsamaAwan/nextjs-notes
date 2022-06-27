# NEXT JS

## Files & Folders

- **pages:** default routing
  - **api:** backend api's
    - **hello.js:** localhost/3000/api/hello
  - **\_app.js:** entry point
  - **index.js:** default route, localhost:3000
- **public:** all static assets
- **styles:**
  - **global.css**
  - \*\*module.css

<br>

## Pages & Routing

In Next.js, a page is a React Component in the pages directory. Each page is associated with a route based on its file name.<br>
Next.js supports pages with dynamic routes. For example, if you create a file called pages/posts/[id].js, then it will be accessible at posts/1, posts/2, etc.

<br>

## Pre-rendering

**Static Generation (Recommended):** The HTML is generated at build time and will be reused on each request.<br>
**Server-side Rendering:** The HTML is generated on each request.

<br>

## Data Fetching

### getServerSideProps

Use this if the data changes often.

```js
function Page({ data }) {
  // Render data...
}

// This gets called on every request
export async function getServerSideProps() {
  // Fetch data from external API
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  // Pass data to the page via props
  return { props: { data } }
}
```

### getStaticProps

Use this if the data doesn't changes

```js
export async function getStaticProps({ params }) {
  return {
    props: {}, // will be passed to the page component as props
  }
}
```

### Incremental Static Regeneration

To use ISR, add the revalidate prop to getStaticProps

```js
export async function getStaticProps() {
  const res = await fetch('https://.../posts')
  const posts = await res.json()

  return {
    props: {
      posts,
    },
    // Next.js will attempt to re-generate the page:
    // - When a request comes in
    // - At most once every 10 seconds
    revalidate: 10, // In seconds
  }
}
```

### getStaticPaths

```js
export const getStaticPaths = async () => {
  const res = await fetch(`${server}/api/articles`)

  const data = await res.json()

  const ids = articles.map(article => article.id)
  const paths = ids.map(id => ({ params: { id: id.toString() } }))

  return {
    paths,
    fallback: false,
  }
}
```

<br>

### Layouts

```js
// pages/_app.js

import Layout from '../components/layout'

export default function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  )
}
```

```js
// components/Layout.js

import Nav from './Nav'
import Meta from './Meta'
import Header from './Header'

const Layout = ({ children }) => {
  return (
    <>
      <Meta />
      <Nav />
      <div className={styles.container}>
        <main className={styles.main}>
          <Header />
          {children}
        </main>
      </div>
    </>
  )
}

export default Layout
```

```js
// components/Meta.js

import Head from 'next/head'

const Meta = ({ title, keywords, description }) => {
  return (
    <Head>
      <meta name='viewport' content='width=device-width, initial-scale=1' />
      <meta name='keywords' content={keywords} />
      <meta name='description' content={description} />
      <meta charSet='utf-8' />
      <link rel='icon' href='/favicon.ico' />
      <title>{title}</title>
    </Head>
  )
}

Meta.defaultProps = {
  title: 'WebDev Newz',
  keywords: 'web development, programming',
  description: 'Get the latest news in web dev',
}

export default Meta
```

<br>

## Image Component and Image Optimization

```js
import Image from 'next/image'
import profilePic from '../public/me.png'

export default function Home() {
  return (
    <>
      <Image
        src={profilePic}
        alt='Picture of the author'
        // width={500} automatically provided
        // height={500} automatically provided
        // blurDataURL="data:..." automatically provided
        // placeholder="blur" // Optional blur-up while loading
      />

      <Image
        src='/me.png'
        alt='Picture of the author'
        width={500} // Because Next.js does not have access to remote files during the build process, you'll need to provide the width, height
        height={500}
      />
    </>
  )
}
```

<br>

## Linking Between Pages & Dynamic Routes

```js
import Link from 'next/link'
import { useRouter } from 'next/router'

export default function Home() {
    const { pid } = router.query
    router.push('/about')
    return (
    <>
        <Link href="/">
    </>
    )
}

```
