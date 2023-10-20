# Creating an AWS CI/CD Pipeline for serverless Next.JS Applications

Author: Larry Thng

GitHub Repo URL: https://github.com/larrythng96/blog-starter-app

## Outline

Traditionally, Next.js applications have been deployed using Vercel, an out-of-the-box CI/CD pipeline designed for serverless Next.js projects. However, due to several limitations including cost and vendor lock-in, this project explores deploying Next.js applications on AWS (which provides better control of system resourcs and configuration), especially since there are not much online resources on how to do this.

## CI/CD Pipeline Architecture

_Please refer to Project 2 Presentation.pptx found in root directory for further information (there is an architecture diagram there which makes it easier to illustrate than words)_

### Outline: Local Machine > GitHub > AWS CodePipeline > AWS CodeBuild > AWS ECR > AWS ECS + Fargate

1. Local Machine: Make changes to source code on local machine.

Source code includes:

a. Dockerfile, a set of instructions on how to take code and build it in a docker container.

b. dockerignore, the files to ignore when building Dockerfile

c. buildspec.yml, a set of instructions for CodePipeline on how to handle the image inside AWS.

When done coding, push code to GitHub.

2. GitHub: Serves as the code repository for the Next.js application.

Any code commit triggers a Webhook linked to CodePipeline

3. AWS CodePipeline: acts as the orchestrator for the entire deployment process.

It stitches various AWS services into a unified workflow.

4. AWS CodeBuild: acts as the Docker Image builder

Upon activation by CodePipeline, it fetches the code, builds a Docker image, and pushes this to AWS S3 as a temporary cache which is then retrieved by ECR.

5. Elastic Container Registry(ECR): Repository for all Docker images.

6. Elastic Container Service (ECS): Container management service. It provides orchestration, scheduling, and deployment of Containers.

Fargate: A serverless server for containers, eliminating the need to manage underlying infrastructure. When deploying with ECS, you select Fargate as your launch type. “Serverless” means that the server’s resources are managed by the provider, in this ECS and Fargate's server configuration and resources are managed by AWS.

Upon receiving notification that there is a new image in ECR, ECS will trigger building of the image and deploy it on the Fargate server.

Upon successful deployment, you will get a server IP address, with your app deployed there!

<br>
<hr>

## Source Code Information

### A statically generated blog example using Next.js, Markdown, and TypeScript

This is the existing [blog-starter](https://github.com/vercel/next.js/tree/canary/examples/blog-starter) plus TypeScript.

This example showcases Next.js's [Static Generation](https://nextjs.org/docs/basic-features/pages) feature using Markdown files as the data source.

The blog posts are stored in `/_posts` as Markdown files with front matter support. Adding a new Markdown file in there will create a new blog post.

To create the blog posts we use [`remark`](https://github.com/remarkjs/remark) and [`remark-html`](https://github.com/remarkjs/remark-html) to convert the Markdown files into an HTML string, and then send it down as a prop to the page. The metadata of every post is handled by [`gray-matter`](https://github.com/jonschlinkert/gray-matter) and also sent in props to the page.

### Demo

[https://next-blog-starter.vercel.app/](https://next-blog-starter.vercel.app/)

### Deploy your own

Deploy the example using [Vercel](https://vercel.com?utm_source=github&utm_medium=readme&utm_campaign=next-example) or preview live with [StackBlitz](https://stackblitz.com/github/vercel/next.js/tree/canary/examples/blog-starter)

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/vercel/next.js/tree/canary/examples/blog-starter&project-name=blog-starter&repository-name=blog-starter)

#### Related examples

- [WordPress](/examples/cms-wordpress)
- [DatoCMS](/examples/cms-datocms)
- [Sanity](/examples/cms-sanity)
- [TakeShape](/examples/cms-takeshape)
- [Prismic](/examples/cms-prismic)
- [Contentful](/examples/cms-contentful)
- [Strapi](/examples/cms-strapi)
- [Agility CMS](/examples/cms-agilitycms)
- [Cosmic](/examples/cms-cosmic)
- [ButterCMS](/examples/cms-buttercms)
- [Storyblok](/examples/cms-storyblok)
- [GraphCMS](/examples/cms-graphcms)
- [Kontent](/examples/cms-kontent)
- [Umbraco Heartcore](/examples/cms-umbraco-heartcore)
- [Builder.io](/examples/cms-builder-io)
- [TinaCMS](/examples/cms-tina/)
- [Enterspeed](/examples/cms-enterspeed)

### How to use

Execute [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app) with [npm](https://docs.npmjs.com/cli/init), [Yarn](https://yarnpkg.com/lang/en/docs/cli/create/), or [pnpm](https://pnpm.io) to bootstrap the example:

```bash
npx create-next-app --example blog-starter blog-starter-app
```

```bash
yarn create next-app --example blog-starter blog-starter-app
```

```bash
pnpm create next-app --example blog-starter blog-starter-app
```

Your blog should be up and running on [http://localhost:3000](http://localhost:3000)! If it doesn't work, post on [GitHub discussions](https://github.com/vercel/next.js/discussions).

Deploy it to the cloud with [Vercel](https://vercel.com/new?utm_source=github&utm_medium=readme&utm_campaign=next-example) ([Documentation](https://nextjs.org/docs/deployment)).

## Notes

`blog-starter` uses [Tailwind CSS](https://tailwindcss.com) [(v3.0)](https://tailwindcss.com/blog/tailwindcss-v3).
