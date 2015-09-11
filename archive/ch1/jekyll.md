##Jekyll, the static site generator

In simplest terms, Jekyll is a website generator. It takes a template directory containing raw text files in various formats, such as Markdown (or Textile), or HTML, and runs those files through converters, then generates a static HTML website that can be hosted on a web server.

Jekyll is also the engine behind GitHub Pages which is probably Jekyll’s easiest to use hosting solution. With it you can simply upload your Jekyll blog to a free GitHub repository and have it automatically compiled and deployed each time you commit. This blog site is hosted on GitHub Pages, for free.

##Why would we use Jekyll?

One of the main reasons to use Jekyll is its simplicity. The CSMI website presents all contents related to the CSMI master of university of Strasbourg. A static site will just do the job.

Pros:

Jekyll doesn’t require any database, unlike Wordpress or other content management systems (CMS). Pages and Posts are converted to static HTML prior to being uploaded to your site.

Jekyll is fast because, being stripped down and without a database, you’re just serving up static pages
Your Jekyll website doesn’t include any functionality or features that you aren’t using.

Jekyll is secure because vulnerabilities that affect platforms like WordPress don’t exist as Jekyll has no CMS, database or PHP.

Cons:

As a ‘bloging platform for hackers,’ Jekyll is aimed at people who don’t mind installing the required dependencies, working from the command line, or doing a little coding.

In most Jekyll configurations everything is done on your local computer. There are many dependencies to install and you can’t blog from just any device such as your phone.
Jekyll doesn’t work very well for multi-author sites and has no functionality for visitors to login to your site.

## Set up Jekyll website

Make sure that Ruby and RubyGems are installed, after that type in command line:

`gem install jekyll`

After installing Jekyll, I created the base Jekyll project.

`jekyll new csmi.cemosis.fr`

For testing Jekyll website, move into the project's folder and type:

`jekyll serve`

Then the website will run at `http://localhost: 4000`

## The file structure and URL routing

Here is the basic directory struct of a Jekyll website:

```
.
├── _config.yml
├── _drafts
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _data
|   └── members.yml
├── _site
├── .jekyll-metadata
└── index.html
```

When user visits firstly the website, the index.html will be visited. All `html` and `markdown` files are going to be transformed when we run `jekyll build` command. Jekyll uses a YAML Front Matter section in these files in order to organise the website routing.

```yaml
---
layout: default
---
```

When jekyll encounters `layout: default`, it will go to `_layout` folder and find a file called `default.html`.

And this is what i put into the `default.html` file.

```html
<!DOCTYPE html>
<html lang="fr">

  {% include head.html %}

  <body>

    {% include header.html %}


    {{ content }}

    

    {% include common.html %}

    {% include footer.html %}

    {% include script.html %}


  </body>

</html>
```
Its barely a standard html file, but with some Liquid template language. 
The `{% include xxx.html %}` code snippet means the default layout will include the `head.html` in `_include` folder. And the `{{ content }}` is a very important part of page rendering. It is the variation part of you whole basic html skeleton.
For example, if the user visits `csmi.cemosis.fr/team/`. It will in fact visits the `default.html` + team.md file's code inserted into the `{{ content }}` part.

```html
---
layout: default
permalink: /team/
---

<center style="padding-top:80px"><h2>&Eacute;quipe pédagogique</h2></center>

<div class="container" style="max-width:500px">

{% for member in site.data.team.members %}

	<table class="table table-bordered table-hover table-striped" style="margin-top:50px">
		<tbody>
		<tr>
			<td style="text-align:center">{{ member.position }}</td>
		</tr>
		<tr>
			<td>
				<ul>

					<li>{{ member.name }}</li>
					<li>Bureau: {{ member.office }}</li>
					<li>E-mail: <a href="mailto:{{ member.email }}">{{ member.email }}</a></li>
					<li>Téléphone: {{ member.telephone }}</li>
				</ul>
			</td>
		</tr>
		</tbody>
	</table>

{% endfor %}

</div>

```

There is two way to set up website routing. The first is to create a markdown file in the root directory and use YAML front matter to set permalink. Just like i made the team page, i need to specify that it uses default layout and its URL is /team/.

The other way is to create a folder named team and to recreate an index.html file. This way serve better when creating more complex pages. For example, i want to created a news pages with severals layers of html layout. I must create a news folder and an index.html file in that folder.
```html
---
layout: news
permalink: /news/
author: all
---

{% for post in site.posts %}
	{% include news_item.html %}
{% endfor %}
``` 
I specified this page has a permanent link `/news/` and i choice a layout other than the basic default layout that i created.

