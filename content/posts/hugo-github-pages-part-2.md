---
title: "Building a GitHub Pages Site with Hugo, GitHub and CircleCI [Part 2]"
date: 2020-07-12T14:23:31+03:00
draft: false
toc: false
images:
aliases:
  - /posts/2020/07/hugo-github-ve-circleci-ile-github-pages-sayfasi-olusturmak-2/
tags:
  - GitHub
  - CircleCI
  - Automation
  - Pipeline
  - Hugo Framework
  - Go Language
  - Blog
---

In [the first part](/posts/2020/07/building-a-github-pages-site-with-hugo-github-and-circleci-part-1/) we created the **blog** repo. Now let's connect it to CircleCI. You could use other CI services instead of CircleCI here; the goal is to build and understand a pipeline.

> **Update (2026):** this site has since moved from CircleCI to GitHub Actions, and the source now lives in the same repository as the published site. The steps below describe the original setup.

Log in with GitHub at **[https://circleci.com/vcs-authorize/](https://circleci.com/vcs-authorize/)**. On the next screen CircleCI will try to connect to GitHub and ask for permissions; grant them.

- After choosing your project with **Add project**, you will see a screen like the one below. It offers ready-made templates for some project types; pick the hello-world one.

![CircleCI](/images/circleCI-first.PNG)

- I had already done this for the **blog** repo, so I picked a different repo for the screenshot. After this step you need to create and approve a pull request. You can see an example PR at [https://github.com/turgayh/JavaScript-Algorithm/pull/1](https://github.com/turgayh/JavaScript-Algorithm/pull/1).
- That's it. Now open `config.yml` in the `.circleci` folder at the root of the repo and clear its contents.

```yaml
version: 2
jobs:
  build:
    docker:
      - image: felicianotech/docker-hugo
    working_directory: ~/project
```

- This gives us a working environment based on a Docker image that contains the Hugo framework, with a project directory.

```yaml
   steps:
      - checkout
      - run:
          name: "Run Hugo"
          command: |
            git submodule add https://github.com/turgayh/hugo-theme-hello-friend-ng.git themes/hello-friend-custom
            hugo -D
            ls ./public
```

- Add the theme to the **blog** repo as a submodule. Note the name **hello-friend-custom**: you can change it. I forked the theme from the main project and use my fork as the submodule, so I renamed it; the theme name in `config.toml` changes accordingly.
- Build the **blog** repo with Hugo and check that the output folder `public` was created.

```yaml
      - run:
          name: "Clone Website"
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              mkdir www
              cd www
              git clone https://github.com/turgayh/turgayh.github.io.git
              cd turgayh.github.io
              rm -r *
              cp -r ../../public/* ./
              ls -la
            fi
```

- Clone the GitHub Pages repo into the `www` directory and delete its contents, then copy in the files from `public` generated in the previous step as the new content.

```yaml
      - run:
          name: "Update Website"
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              git config --global user.email "turgayh@itu.edu.tr"
              git config --global user.name "turgayh"
              cd ./www/turgayh.github.io
              git add .
              git commit -m "Circle CI Update Website | Build Number:${CIRCLE_BUILD_NUM}"
              git push -q https://${DEPLOY_KEY}@github.com/turgayh/turgayh.github.io.git master
            fi
```

- The GitHub Pages content is now updated and we need to commit it, again from CircleCI. The **${DEPLOY_KEY}** parameter in the snippet above must be generated specifically for the target GitHub project and passed to CircleCI as a parameter.
- Create the **${DEPLOY_KEY}** value on GitHub via Settings > Personal Access Tokens > Generate Token.
- Add the generated token to CircleCI under Environment Variables.

![CircleCI environment variables](/images/token.PNG)

- The **blog** repo is ready. Push it and check whether it builds on CircleCI.

![Successful build](/images/success-build.PNG)

### config.yml

```yaml
version: 2
jobs:
  build:
    docker:
      - image: felicianotech/docker-hugo
    working_directory: ~/project
    steps:
      - checkout
      - run:
          name: "Run Hugo"
          command: |
            git submodule add https://github.com/turgayh/hugo-theme-hello-friend-ng.git themes/hello-friend-custom
            hugo -D
            ls ./public

      - run:
          name: "Clone Website"
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              mkdir www
              cd www
              git clone https://github.com/turgayh/turgayh.github.io.git
              cd turgayh.github.io
              rm -r *
              cp -r ../../public/* ./
              ls -la
            fi

      - run:
          name: "Update Website"
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              git config --global user.email "turgayh@itu.edu.tr"
              git config --global user.name "turgayh"
              cd ./www/turgayh.github.io
              git add .
              git commit -m "Circle CI Update Website | Build Number:${CIRCLE_BUILD_NUM}"
              git push -q https://${DEPLOY_KEY}@github.com/turgayh/turgayh.github.io.git master
            fi
```
