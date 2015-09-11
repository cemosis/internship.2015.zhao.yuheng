#A localization solution: jekyll-multiple-plugin

## Install and set-up
To install multiple languages plugin:

```bash
gem install jekyll-multiple-language-plugin
```

In the file `_plugins/ext.rb` add: 

```ruby
require 'jekyll/multiple/language/plugin
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

After test, this approach works perfectly for every page.

Note: Due to a serious bug that this gem can't read post under `_posts` at all, and the bug fix in [chapter 2](ch2/bugfix.md). The installation procedure is no more needed, the source code is already included in `_plugin` folder.