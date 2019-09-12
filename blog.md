---
nav-title: Blog
breadcrumb: Blog
---

<h1> Our blog articles </h1>

{% for post in site.posts %}
    <section class="post-teaser"> 
        {% if post.image %}
            <img class="image" src="{{ site.baseurl }}/images/posts/{{ post.image }}"/>
        {% else %}
            <img class="image" src="{{ site.baseurl }}/images/blog.jpg"/>
        {% endif %}  
        <p class="title"> <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a> </p>
        <p class="date"> Date: {{ post.date | date: '%d.%m.%Y' }} </p>
        <p class="author"> Author: {{ post.author }} </p>
        {% if post.excerpt %}
            <p class="excerpt"> {{ post.excerpt }} </p>
        {% else %}
            <p class="excerpt"> {{ post.content | strip_html | truncatewords: 50 }} </p>
        {% endif %}
    </section>
{% endfor %}
