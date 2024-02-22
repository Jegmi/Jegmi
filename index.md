# 🏠 Jannes Jegminat - personal blog

What I missed most when I worked in industry was not being able to share my work openly. It is the beginning of 2024 and I am moving back to research. As a PostDoc at Mt Sinai in NY, I will work at the intersection of clinicians, ML practioners and millions of patient records. Finally, I can share again what I learn. 

You will find notes about:
  1. statistics and machine learning applied to clinical data
  2. procedures and use-cases at Mt Sinia hospital
  3. the commercialisation of my applied research via a spin-off

If something strikes you as interesting, as wrong or both, feel free to reach out!

{% for post in site.posts %}
{{ post.excerpt }}
<a href=".{{ post.url }}">➡️ {{post.title }}</a>
{% endfor %}
