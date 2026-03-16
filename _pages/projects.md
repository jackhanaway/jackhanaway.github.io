title: "Projects & Apps"
permalink: /projects/
A collection of analytical tools and research projects built across professional and collegiate baseball environments. Each project was designed to solve a real operational problem — whether that's giving coaches faster access to athlete information, profiling physical development against peer benchmarks, or quantifying the effect of pitch shape on hitter performance.

{% assign projects = site.projects %}
{% for project in projects %}
[{{ project.title }}]({{ project.url }})
{{ project.excerpt }}
Tools: {{ project.tools }}
[View Project →]({{ project.url }}){: .btn .btn--primary}

{% endfor %}
