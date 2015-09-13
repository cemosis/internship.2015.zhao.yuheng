#"Database" in Jekyll
As a static website generator, Jekyll doesn't support using a real database. But Jekyll supports loading data from YAML, JSON, and CSV files located in the `_data` directory under the root of website source code.
With help of these data files, we can create our own some kind of fake database to avoid many repetitive works in our code and seperate real contents from website's code.
I picked YAML as the data file's format. Here is an example:
File name:`members.yml`
```yaml
- name: Christophe Prudhomme
  github: prudhomm
  bio: Professor in Scientifc Computing
  place: Strasbourg

- name: Vincent Chabannes
  github: vincentchabannes
  bio: Research Engineer
  place: Grenoble

- name: Vincent Huber
  github: vhuber
  bio: Research Engineer
  place: Strasbourg
  
- name: Matthieu Boileau
  github: boileaum
  bio: Research Engineer
  place: Strasbourg

- name: Alexandre Ancel
  github: aancel
  bio: Research Engineer
  place: Strasbourg
```
A list of professors is built, the following code snippet can access this list:
```Liquid
{% for member in site.data.members %}
	//each member contains the informations from name to place
	//do something wuth member
{% endfor %}
```
An real implentation of professor and his courses is included in [Appendix/Jekyll Tips And Tricks](../Appendix/jekyll.md) chapter.