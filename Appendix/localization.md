#Localization With Jekyll: jekyll-multiple-plugin

## Installation and set-up
To install multiple languages plugin, copy the source code file `localization.rb` written in Ruby from the author's [repository](https://github.com/screeninteraction/jekyll-multiple-languages-plugin) into a folder called `_plugins` just under the project's root directory.

There is bug in original source code that causes Jekyll can't read at all markdown files in `_posts`.

I browsed the source code, and found this following code snippet.

```ruby
alias :read_posts_org :read_posts
def read_posts(dir)
  if dir == ''
    read_posts("_i18n/#{self.config['lang']}/")
  else
    read_posts_org(dir)
  end
end
```
The read_posts method is a jekyll's built-in function that reads all the files in `<base>/_posts` and create a new Post object with each one. By default it will use `_posts` as parameter, but somehow it doesn't work correctly here.
So what i did is that i passed `_posts` as parameter so that it can read correctly the post.
```ruby
alias :read_posts_org :read_posts
def read_posts(dir)
  if dir == '_posts'
    read_posts("_i18n/#{self.config['lang']}/")
  else
    read_posts_org(dir)
  end
end
```

Set configurations in _config.yml: 

```yaml
languages: ["fr", "en", "de"]

exclud_from_localizations: ["js", "images", "stylesheet"]
```
This means the default language is French, all the webpages will also be translated into English and German. When the gem helps jekyll generate html pages, it will not recopy files in the js folder, images folder or stylesheet folder. If I don't set excluded files, it will copy all asset files into the translated site folders: `_fr` and `_de`. Which will take more storage  spaces.

Next we need to create a folder called `_18n` (named by the gem's author) and create three translation asset files: `de.yml`, `en.yml`, and `fr.yml` in it.

These files contain text blocks and their associated variable names which can be accessed via Liquid template language.

`fr.yml`
```yaml
global:

navbar:
    home: Page d'accueil
    team: "&Eacute;quipe"
    promotions: Promotions
    news: Actualit&eacute;s
    course: Enseignements
```
`en.yml`
```yaml
global:

navbar:
    home: Home
    team: Team
    promotions: Classes
    news: News
    course: Courses
```

So with these two files, simply use `{{ t navbar.home }}` or `{{ translated navbar.home }}` in index.html file for representing the navigation button which targets to home page.

# Link between languages

The multiple languages gem provides us some useful variables for generating links between different languages.

For example, the website main page by default is in French. An english speaker came in and clicked on the UK's flag button. Then all URLs should always begin with a `/en/` before the page's URL relative to website's hostname.(csmi.cemosis.fr).

`csmi.cemosis.fr/team` -> `csmi.cemosis.fr/en/team`

To do so, i firstly set up a contional statement in all basic layouts(index and default).

```
  {% assign link-lang = "" %}
  {% if site.lang == "en" %}
    {% assign link-lang = "/en" %}
  {% endif %}
  {% if site.lang == "de" %}
    {% assign link-lang = "/de" %}
  {% endif %}
  {% include head.html %}
```

If the rendered page is in French, nothing special we keep it as original URL.
If the rendered page is in English, then `link-lang` will become `/en`. Then I add `/en` at the front of any link.

Then there is a second part: put buttons in navbar to switch between all three languages.
After several hours of testing, I finally found out a functional approach.


```html
          {% if site.lang == "fr" %}
            {% capture link1 %}/en{{ page.url }}{% endcapture %}
            {% capture link2 %}/de{{ page.url }}{% endcapture %}
            <li><a href="{{ link1 }}"><img src="/images/United-Kingdom.png"></a></li>
            <li><a href="{{ link2 }}"><img src="/images/Germany.png"></a></li>
          {% endif %}

          {% if site.lang == "en" %}
            {% capture link1 %}{{ page.url }}{% endcapture %}
            {% capture link2 %}{{ site.baseurl | replace:'en','de' }}{{ page.url }}{% endcapture %}
            <li><a href="{{ link1 }}"><img src="/images/France.png"></a></li>
            <li><a href="{{ link2 }}"><img src="/images/Germany.png"></a></li>
          {% endif %}

          {% if site.lang == "de" %}
            {% capture link1 %}{{ page.url }}{% endcapture %}
            {% capture link2 %}{{ site.baseurl | replace:'de','en' }}{{ page.url }}{% endcapture %}
            <li><a href="{{ link1 }}"><img src="/images/France.png"></a></li>
            <li><a href="{{ link2 }}"><img src="/images/United-Kingdom.png"></a></li>
          {% endif %}
```

The `page.url` is the URL relative to root URL. For example, if the active web page is `csmi.cemosis.fr/team`, then `page.url` equals to `/team/`. The first conditional statement just adds a `en` or a `de` before the relative URL. The slash symbol is super important because without it jekyll will treat it as a relative URL to the current page which means the `csmi.cemosis.fr/en/team` will become `csmi.cemosis.fr/team/en/team`. This total mess stuck me so hard when I was trying to find a way out.

For languages other than French we need not only `page.url` but also `site.baseurl`. To come back to French, it's quite simple: we get the relative URL and that's it, it does not have language prefix. But for switching between English and German, we need a URL that contains the language prefix. This is the reason that we need `site.baseurl`. The `site.baseurl` contains a language postfix. For example, the `site.baseurl` of `csmi.cemosis.fr/en/team/` is `csmi.cemosis.fr/en/`. What we need to do is just replace `en` by `de`. And we don't forget to bring back the relative URL so that we can stay in the same page while changing the display language.