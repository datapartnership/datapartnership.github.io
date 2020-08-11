+++
date = 2020-07-13T00:00:00Z
title = "Introducing the Data Partnership Knowledge Base"
type = "Article"
+++

After rolling out a [Pilot Phase](/updates/why-datacollaboratives/) in 2018, the Development Data Partnership has scaled up fast, and since our colleagues have brought to life more than 100 projects supported by partnerships with more than 20 companies.

All we inspire to do is to see data being put in good and responsable use. From the very [beginning](updates/data-day/), we have watched diverse areas of study and projects being carried out and strongly benefiting from third-party data, specially where data scarcity poses an extra challenge for success. Today, we are accompanying the evolution of projects addressing [Sustainable Development Goals](https://www.un.org/sustainabledevelopment/sustainable-development-goals/) with teams putting energy into food security, [estimating access to employment opportunities](http://localhost:1313/updates/employment-opportunities-in-bangladesh/), [mobility indicators](https://blogs.worldbank.org/sustainablecities/poor-people-respond-differently-stay-home-orders-heres-what-data-says), [locust response](https://www.climacell.org/blog/climacell-completes-24-hour-hackathon-to-help-address-the-locust-spread-in-africa-and-beyond/), healthcare acessibility, road safety, among others.

On the next phase, we are expanding support and resources across all participating organizations ranging from data documentation, snippets and examples to provisioning data pipelines and computing environments. Collaboration plays a lead role of the initiative. It is important to see that all that information is up-to-date, easily acessible and collaborative.

We are introducing the Development Data Partnership Knowledge Base!

On the [Data Partnership](https://datapartnership.org) website, you will now find the [Documentation](https://datapartnership.github.io/welcome/) link that will take you directly to our Knowledge Base where you can explore further.

<img src="/updates/data-partnership-knowledge-base/data-partnership-knowledge-base-site.png" style="width:100%;"/><br>

Partially inspired by the [Team Data Science Process](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) and [How to build a data analytics dream team](https://mitsloan.mit.edu/ideas-made-to-matter/how-to-build-a-data-analytics-dream-team), we are paving the way to accomodate all different components and roles that make a data-driven project successful and creating a centralized reference for data scientists and researchers.

The Development Data Partnership Knowledge Base follows the **literate programming** paradigm.

*Literate programming is a programming paradigm introduced by Donald Knuth in which a computer program is given an explanation of its logic in a natural language, such as English, interspersed with snippets of macros and traditional source code, from which compilable source code can be generated.*

As part of the Development Data Partnership, members have access to:

- Tools and snippets to get your data analysis up and running.
- A Python package that includes both partner-specific and general facilitators.
- A command-line toolbox that includes both partner-specific and general facilitators.

<img src="/updates/data-partnership-knowledge-base/data-partnership-knowledge-base-partnership.png" style="width:100%;"/><br>

Each participating international organization, e.g. the Inter-American Development Bank, has its own documentation set with the Data Partners it has signed agreements with.

The reason for that is because we have to be able to provide different contributors access to specific subsets of the documentation while preserving the possibility to share code base among all participating organizations.

<img src="/updates/data-partnership-knowledge-base/data-partnership-knowledge-base-partner.png" style="width:100%;"/><br>

Each Data Partner, for example Google, also has its own home. You can find snippets as well as fully working examples for common tasks, such as downloading from Facebook GeoInsights using Python, filtering Cuebiq geospatial data, running spatial joins using Dask and Geopandas or using Mapbox's Matrix API.

And possibly even make your own contributions!

## Data Partner Documentation

As we have said, each Data Partner has a dedicated page on the documentation.

In fact, that page has a double life and is a Jupyter notebook! You can either choose to work with it  by cloning from the repository or by viewing a rendered page on your browser.

It is important to say that we are not reinviting the wheel; rather, the documentation offers a centralized place for references and examples as well as details about the datasets, such as how to access (**Getting Access**), about the data itself (**Summary**) and **Examples**.

### Getting Access

After submitting your proposal to [Data Partnership](https://datapartnership.org/) and having it approved, this section details how you should expect to get access to data for each Data Partner.

Even before your project take shape, the section helps estimate what kind what resources will be required from your team.

### Summary

The rationale for a data summary is to give a sneak peek into the datasets, even before submitting a project proposal, so you can have a rough estimate and better decide whether or not the data partner opportunity will help you in your project.

For example, when exploring the mobile data opportunities, you will be able to see the number of records by geographic region and day by day, so you can evalulate the quality of the sampling of each Data Partner. Also, you will be able to compare those metrics from different partners, possibly deciding for one or a combination of datasets.

### Examples and Snippets

Taking an inspiration from *Don't repeat yourself (DRY)* principle, examples, snippets and facilitators are at the heart of what the documentation was brought to life.

A good example of a facilitator that was created by Data Partership is for handling [Facebook](https://dataforgood.fb.com).

Unfortunately, no API is available. Fortunately, the Data Partnership supports an easy alternative for programmatically downloading data from the Facebook GeoInsights Portal via the Facebook class on the *datapartnership* Python package.

After installing the package, you will be able to easily create a login session and download the files directly and programmatically from your notebook.

See more about the Facebook class on the [repository](https://github.com/datapartnership/ddp-docs-facebook).

## Tips and Tricks

As the name suggests, this section is where you can find tips and guides on how to set up common tools in projects, such as AWS, Python and environment variables.

## Collaboration

The Data Partnership is not only about licensing and data, but it is about sharing code, knowledge and building a community. That is why we take advantage of everything GitHub has to offer. You are more than welcome to open issues and pull requests and share your own contributions.

Certainly your team's efforts will be highly appreciated and recognized. Read more on [contributing](https://datapartnership.github.io/welcome/).

_Authors:_

* [_Gabriel Stefanini Vicente_](https://g4brielvs.me/), _Data Scientist, Development Data Partnership_

*The Development Data Partnership is a partnership between international organizations and companies, created to facilitate the use of third-party data in research and international development.*