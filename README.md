# datapartnership.org

This repo contains the code and deployment details for the datapartnership.org site.

## How is the site built?

The site is a "statically generated site". This means, instead of running a web server with a database and a traditional CMS, we use a tool called a static site generator to transform the text into ready made html pages that we can show to the user (at a blazing fast speed). There are no databases, no complicated servers to manage, backup, run and keep secure.

In "dev-speak", a traditional web development method might use something like "LAMP" stack (Linux, Apache, MySQL, PHP -- think Drupal) or "MEAN" stack (Mongo, Express, Angular, Node). This site uses ["JAM" Stack]( https://jamstack.org/): Javascript, API, Markdown.

## Toolset
- Static site generator: [Hugo](https://gohugo.io/)
- Deployment: [Netlify](https://www.netlify.com/)
- Editing:
  - [Forestry](https://forestry.io/) (can be updated to be what you want).
  - GitHub Web UI.
  - Your local text editor -- this assumes you know how to use the command line and git.

## Editing the site

### Edit online on Forestry.io
- Create an account with forestry.io
- Ask Holly(hkrambeck@worldbank.org) to add you to the team.
- Login and edit (the UI is mostly self explanatory)

### Edit in the browser with GitHub's web user interface

You can use the GitHub pages to just go to any Markdown file in content, edit it using the web user interface. Once you commit your change, it will take a few seconds for the site to rebuild.

![edit file](./docs/img/edit-file.png)

### Edit Locally

Assumptions:
- You can use the command line (basic level).
- You can use git.

A faster and more flexible way to edit and tinker with the site is to work locally. This is especially recommended if you want to experiment with heavy edits. You will need to:

- install hugo https://gohugo.io/getting-started/installing/
- install git https://git-scm.com/
- optional: install GitHub Desktop or whatever UI tool you are comfortable with to push your changes. https://git-scm.com/downloads/guis

To install Hugo, you can read the instructions on the link, but in reality, it will take you only a minute or so if you do:

OSX: `brew install hugo`  
Ubuntu: `sudo apt-get install hugo`

For using git and github, this is a little bit out of the scope of this readme, but there are plenty of tutorials online.
If you are using GitHub Desktop, this is a good place to start https://help.github.com/en/desktop/contributing-to-projects


## Development

For development:

- Install Hugo
- Install Git (it would be surprising if you don't have this already)
-


## Deployment
