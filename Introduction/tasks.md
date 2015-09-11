#Tasks During Internship

## Website development

### csmi.math.unistra.fr, csmi.cemosis.fr
- Building website with Jekyll from gourd up
- English and German localization
- Make it responsive on mobile devices

### www.cemosis.fr
- Migrate old Google Sites and rebuild it with Jekyll
- rewrite all contents into news website as they are written directly on Google Sites pages
- Find a simple, unified solution to write content for various projects
- Make it responsive on mobile devices

### www.feelpp.org and doc.feelpp.org
- Migrate the website from Github pages, and host them on dedicated server

## Centerlized `news` repository for three websites above
- Create a github repository as a git subtree called `news` to collect all news files written in markdown included these three websites
- Synchronise automatically all news files between three websites

## Github Webhook development
- Setup a web service on the dedicated server to automatically generate or deploy all websites owned by Cemosis.
- The web service must be also capable of synchronising news repository between three websites mentioned above.