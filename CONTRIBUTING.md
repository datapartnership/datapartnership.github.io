# Contributing

**Thanks for contributing to the Development Data Partnership!** The [Development Data Partnership](https://datapartnership.org/) website is a [Hugo](https://gohugo.io/)-generated static page that is continuously deployed on [GitHub Pages](https://pages.github.com) via [GitHub Actions](https://github.com/features/actions). If not familiar with Hugo, please check out a [Quick Start](https://gohugo.io/getting-started/quick-start/).

In summary, the continuous deployment (CD) works as follows:

- On each new pull request, a preview will be available. The deploy preview will be posted by the `netlify bot` on the comments.

- On each commit to `main`, the website is published to [datapartnership.org](https://datapartnership.org). The domain is registed AWS Route 53 with the World Bank.

## How to post/edit on the Partnership blog

All content is are located under [content/](https://github.com/datapartnership/datapartnership.org/tree/master/content). Follow the instructions below on how to create and submit your story for publication.

**Checklist**

- Finalize the text on a Word document (include link on the issue). If necessary, coordinate Data Partner approval(s).
- [Create an issue](https://github.com/datapartnership/datapartnership.github.io/issues/new) with the title of the story (e.g, #94) and link it to [Operations](https://github.com/orgs/datapartnership/projects/9).
- Create a story folder under `updates/`, e.g. [content/updates/africa-digital-transformation](content/updates/africa-digital-transformation). Create a blog post `index.md` file in Markdown using the [Story Template](#story-template).
- Create a branch and commit all changes, including additional materials. Please use folder you just created to upload any attachments and images.
- [Open a pull request](https://github.com/datapartnership/datapartnership.github.io/pulls) and assign a [maintainer](https://github.com/orgs/datapartnership/teams/maintainers) as reviewer.

## How to upload a image/picture on your blog post

To add an image to your story, upload a `.png` or `.jpeg` image to the story folder. Remember to use a relative path in Markdown.

For example,

```{markdown}
![Mapbox Spain](Mapbox-Spain.png)
```

## Story Template

When writing about your project, consider the template and the [TOML](https://toml.io/en/) preamble below to create and include metadata about your story. All files are Markdown files (If you want to know more about the syntax, please visit [GitHub Markdown Cheatsheet page](https://guides.github.com/features/mastering-markdown/)).

- title: Select title
- authors: Select authors
- date: Select time reference when your post is published, e.g. `2020-03-19T20:12:14Z`
- post_type: Select type, i.e. *"Case Study", "Article", "Events" or "Tutorial"*.
- tags: Select one or more tags.
- links: Select one or more external (Read more) links.
- partner: Select Data Partner(s) associated with this project, e.g. `["Meta, Mapbox"]`.
- dev_partner: Select Development Partner(s) associated with this project, e.g. `["World Bank"]`.

For example, use the preamble below.

```{markdown}
+++
title = "Addressing COVID-19 through Public-Private Data Partnerships: Where Do We Put New Testing Facilities?"
authors = ["Bruno Nuno"]
date = 2020-03-19T20:12:14Z
partner = ["Meta", "Mapbox"]
dev_partner = ["World Bank"]
+++
```

### Case Study Template

```{markdown}
+++
title =
authors =
post_type =
partner =
dev_partner =
tags =
links =
date =
+++

Provide a brief introduction to the project and mention the Team/Global Practice/Sector and Development Partner.

## Challenge

Describe in detail the challenge the project addressed.

## Solution

Describe what solution the problem applied to address the challenge and what data dataset and data partners contributed to solving the problem.

## Impact
Tell us more about the impact of the project and the results obtained by implementing the solution. Please mention if the project plans to continue/expand or scale in other countries/sectors.

```
