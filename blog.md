---
nav-title: Blog
author: Christian Rödel
---

<h1> Recent blog articles </h1>

{% for post in site.posts %}
  <section class="post"> 
    <h3 class="title"> <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> </h3>
      <section class="date"> {{ post.date | date: '%d.%m.%Y' }} </section>
      <section class="author"> {{ post.author }} </section>
      <img class="image" src="{{ site.baseurl }}/assets/images/{{ post.image }}"/>
      <section class="excerpt"> {{ post.content | strip_html | truncatewords: 50 }} </section>
  </section>
{% endfor %}
