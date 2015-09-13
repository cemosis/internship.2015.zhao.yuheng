#Develop Website With Jekyll
In simplest terms, Jekyll is a website generator. It takes a template directory containing raw text files in various formats, such as Markdown (or Textile), or HTML, and runs those files through converters, then generates a static HTML website that can be hosted on a web server.

Jekyll is also the engine behind GitHub Pages which is probably Jekyll’s easiest to use hosting solution. With it I can simply upload my Jekyll blog to a free GitHub repository.

##Why would we use Jekyll?

One of the main reasons to use Jekyll is its simplicity. The CSMI website presents all contents related to the CSMI master of university of Strasbourg. A static site will just do the job. Same with cemosis.fr, the website for Cemosis itself.

Pros:

Jekyll doesn’t require any database, unlike Wordpress or other content management systems (CMS). Pages and Posts are converted to static HTML prior to being uploaded to your site.

Jekyll is fast because, being stripped down and without a database, you’re just serving up static pages.
Jekyll website doesn’t include any functionality or features that you aren’t using.

Jekyll is secure because vulnerabilities that affect platforms like WordPress don’t exist as Jekyll has no CMS, database or PHP.

## The file structure of a Jekyll website

Here is the basic directory struct of a Jekyll website:

```
.
├── _config.yml
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2013-05-22-presentation-cemosis.md
|   └── 2013-09-04-nouvel-arrivant.md
├── _data
|   └── members.yml
├── _site
├── .gitignore
├── _config.yml
└── index.html
```
Before the static website generation, Jekyll will parse the content from markdown under `_posts` directory and will read data from `YAML` data files under `_data` directory when there is any data file called by the Liquid Template language.

## The Liquid Template language
Jekyll uses the Liquid templating language to process templates. Here template means that we define some structured output(usually as HTML code) to generate automatically some complex content.
