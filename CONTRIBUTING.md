# Contributing

🎉 Thanks for taking the time to contribute to the Development Data Partnership! 🎉

First off, visit our [blog](https://datapartnership.org/updates/) to explore and learn more about news and projects. Our blog is a [Hugo](https://gohugo.io/) static site that is deployed on [GitHub Pages](https://pages.github.com) with [GitHub Actions](https://github.com/features/actions). If not familiar with Hugo, check out a [Quick Start](https://gohugo.io/getting-started/quick-start/).

- On each new pull request, a preview will be available. The deploy preview will be posted by the `netlify bot` on the comments.

- On each commit to `main`, the website is published to https://datapartnership.org.

## How to post/edit on the Partnership blog

All blog posts, published through this repository on [GitHub Pages](https://pages.github.com), are located under [updates](https://github.com/datapartnership/datapartnership.org/tree/master/content/updates). Follow the instructions below on how to create and have your story published.

Alternatively, you can become an editor on [Forestry.io](https://forestry.io/). For more information, please reach out [datapartnership@worldbank.org](mailto:datapartnership@worldbank.org).

**Checklist**

- Finalize the text, including getting approval (if necessary) from Data Partners, on a Word document (include link on the issue).
- [Create a issue](https://github.com/datapartnership/datapartnership.github.io/issues/new) with the title of the story, e.g,  [Digitalization and Resilience to COVID-19 Shocks: Evidence from Sub-Saharan Africa](https://github.com/datapartnership/datapartnership.github.io/issues/94).
- Create a blog post file in Markdown under `updates`. See [Story Template](#story-template).
- Create a branch and commit all changes, including additional materials. Please use `static` to upload any attachments and images.
- [Open a pull request](https://github.com/datapartnership/datapartnership.github.io/pulls) and assign a [maintainer](https://github.com/orgs/datapartnership/teams/maintainers) as reviewer.

## How to upload a image/picture on your blog post

To add an image to your story, upload a `.png` or `.jpeg` image to the [static](https://github.com/datapartnership/datapartnership.github.io/tree/master/static) and import it on the Markdown file ((don't forget the leading slash).

For example,

```{markdown}
![Mapbox Spain](/Mapbox-Spain.png)
```

## Story Template

When writing about your project, consider the template and the [TOML](https://toml.io/en/) preamble below to create and include metadata about your story. All files are Markdown files (If you want to know more about the syntax, please visit [GitHub Markdown Cheatsheet page](https://guides.github.com/features/mastering-markdown/)).

- title: Select title
- authors: Select authors
- date: Select time reference when your post is published, e.g. `2020-03-19T20:12:14Z`
- type: Select type, i.e. *"Case Study", "Article", "Events" or "Tutorial"*.
- partner: Select Data Partner(s) associated with this project, e.g. `["Facebook, Mapbox"]`.
- dev_partner: Select Development Partner(s) associated with this project, e.g. `["World Bank"]`.

For example, use the preamble below.

```{markdown}
+++
title = "Addressing COVID-19 through Public-Private Data Partnerships: Where Do We Put New Testing Facilities?"
authors = ["Bruno Nuno"]
date = 2020-03-19T20:12:14Z
partner = ["Facebook", "Mapbox"]
dev_partner = ["World Bank"]
+++
```

### Case Study Template

- Challenge: Provide a brief introduction about the project and theme of study as well as describe the main problem related to it

- Solution: Describe into more details about the project and what solutions it proposes to solve a specific issue. Additionally, do not forget to describe a little bit about the dataset or data resource used on this initiative that was provided by a Data Partners

- Impact: The final and conclusive part of your text describes the achievement and results that were obtained by implementing the referred solution