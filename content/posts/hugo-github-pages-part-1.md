---
title: "Building a GitHub Pages Site with Hugo, GitHub and CircleCI [Part 1]"
date: 2020-07-12T09:34:27+03:00
draft: false
toc: false
images:
aliases:
  - /posts/2020/07/hugo-github-ve-circleci-ile-github-pages-sayfasi-olusturmak-1/
tags:
  - GitHub
  - CircleCI
  - Automation
  - Pipeline
  - Hugo Framework
  - Go Language
  - Blog
---

![Hugo Framework](/images/hugo-framework.png)

**Hugo Framework:** think of it as a tool for building static websites. It is an ideal environment for blog posts: if you like writing in Markdown and managing everything through code, it is a great fit.

> **Update (2026):** this site has since moved from CircleCI to GitHub Actions, and the source now lives in the same repository as the published site. The steps below describe the original setup.

- This blog runs on the approach described here. I automated as much of the process as I could.

1. Create two GitHub repositories. One of them must be the GitHub Pages repo, i.e. the repo that hosts the content served at `[username].github.io`.
   [This page explains how to create it.](https://pages.github.com)

2. The repo should contain only a `README.md`; delete any other files if there are some.

3. I created two repos: **blog** and **turgayh.github.io**.

4. Create the Hugo project locally. [Hugo Quick Start](https://gohugo.io/getting-started/quick-start/)

```shell
hugo new site blog
```

This creates the content of the **blog** repo.

```shell
cd blog
git init
git submodule add https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
```

- I added the theme to the project as a git submodule. You should now see the theme in a folder named `hello-friend-ng` under `themes`.
- **config.toml** holds your site's settings. You need to set the theme in this file; to start with, you can copy the contents of `config.toml` from `themes/hello-friend-ng/exampleSite`.

```shell
hugo new posts/my-first-post.md
```

- This creates a first example post in Markdown under `content/posts`.

```shell
hugo server -D
```

- If everything went well, you will see the output below at [http://localhost:1313](http://localhost:1313). Now we can commit the **blog** repo.

![Result with the example theme](/images/hugo-example.png "Result with the example theme")

In [Part 2](/posts/2020/07/building-a-github-pages-site-with-hugo-github-and-circleci-part-2/) we connect the repo to CircleCI so the site is built and deployed automatically.
