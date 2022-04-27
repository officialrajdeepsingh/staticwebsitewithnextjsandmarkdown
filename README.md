This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `pages/index.js`. The page auto-updates as you edit the file.

[API routes](https://nextjs.org/docs/api-routes/introduction) can be accessed on [http://localhost:3000/api/hello](http://localhost:3000/api/hello). This endpoint can be edited in `pages/api/hello.js`.

The `pages/api` directory is mapped to `/api/*`. Files in this directory are treated as [API routes](https://nextjs.org/docs/api-routes/introduction) instead of React pages.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js/) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/deployment) for more details.

export async function getStaticPaths() {
  const files = fs.readdirSync(path.join('posts'))

  let tempStorage = []
  const temppaths = files.map((filename) => {

    const markdownWithMeta = fs.readFileSync(
      path.join('posts', filename),
      'utf-8'
    )

    const { data: frontmatter } = matter(markdownWithMeta)

    if (frontmatter.draft === false) {
      frontmatter.categories.map(
        category => {
          let slug = slugify(category)
          tempStorage.push(slug)

        }
      )

      return {
        params: {
          slug: tempStorage,
        },
      }
    } else {
      return null
    }


  })


  
  
  // const  paths= tempStorage.filter((item, 
  //       index) => tempStorage.indexOf(item) === index);

const paths=["npm"]


  return {
    paths,
    fallback: false,
  }

}

export async function getStaticProps({ params: { slug } }) {

  // Get files from the posts dir
  const files = fs.readdirSync(path.join('posts'))

  let tempStorage = []

  console.log(slug, ' slug ')
  
  // Get slug and frontmatter from posts

  const tempPosts = files.map((filename) => {

    // Get frontmatter
    const markdownWithMeta = fs.readFileSync(
      path.join('posts', filename),
      'utf-8'
    )

    const { data: frontmatter } = matter(markdownWithMeta)

    if (frontmatter.draft === false) {
        frontmatter.categories.map(
            category => {
                 let categroySlug = slugify(category)
                if(slug ===categroySlug ){
                  tempStorage.push(frontmatter)
                }
            }
        )
    } else {
      return null
    }

    return {
      slug,
      tempPosts,
    }
  })

  console.log(tempPosts, ' tempPosts ')

  //  remove null in tempPosts 

  const posts = tempPosts.filter(
    post => {
      return post && post
    }
  )

  return {
    props: {
      frontmatter,
      slug
    },
  }


}