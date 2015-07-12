## Supported data type

In addition to the built-in variables available from Jekyll, we can specify our own custom data that can be accessed via the Liquid templating system.

Jekyll supports loading data form YAML, JSON, and CSV files located in the `_data` directory. This is the only place we can put data files in.

With this feature, we can avoiding many heavy hard-coded part.

For CSMI website, we have 3 types of data: the educational team of master CSMI, the testimony of students, and the courses of master CSMI.

So below the `_data` folder i created a `course` folder, a `team` folder and a `testimony` file. As we doesn't have many testimonies for now, i simply store them in one single YAML file.

In the YAML file, the data will be stored like this way:

`testimony.yml:`

```yaml
- author: Cécile Daversin
  testimony: >
              Mon intérêt pour les mathématiques m'a conduit naturellement vers les mathématiques fondamentales. 
              En découvrant que mes connaissances pouvaient servir à répondre à des besoins concrets, j'ai choisi 
              de m'orienter vers les mathématiques appliquées. 
              Mon master en simulation numérique m'a permis d'avoir mon premier contrat en tant qu'ingénieur en calcul 
              scientifique au Laboratoire National des Champs Magnétiques Intenses, avant de commencer ma thèse.
              Mes travaux sont quotidiennement utilisés au sein du laboratoire, ce qui me procure le sentiment d'être utile 
              et complètement intégrée dans le processus complet de fabrication des aimants, réunissant une large gamme de métiers.
               C'est ce lien étroit et direct entre le développement de méthodes numériques innovantes et les applications concrètes
                qui m'a convaincu de poursuivre dans cette voie.
  job: Doctorante, Université de Strasbourg
  id: testimony-0
  year: 

```

`courseS1.yml:`

```yaml

- name: Algorithmique I
  ects: 3
  content: Algorithmes algébriques, algorithmes de tris. Estimations asymptotiques.
  knowledege: La sensibilité aux coûts et performances d'algorithmes variés. Savoir déterminer une complexité maximale et une complexité en moyenne.
  info: "Session de rattrapage : cf. MECC 2014-15"
  duration: "Cours Intégrés : 26h"
  id: s1-1

````

`team/members.yml`

```yaml

- name: Christophe Prudhomme
  position: Responsable du Master
  office: UFR Math Info Bureau 210
  email: prudhomme@unistra.fr
  telephone: 03 68 85 00 89
  
- name: Marion Seidel
  position: Responsable administrative
  office: 
  email: mseidel@unistra.fr
  telephone: 03 68 85 02 88

```

## Accessing data with Liquid

In the case that we need to show all the educational team members of CSMI.
I used the following code to show them in tables.

```html

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
```