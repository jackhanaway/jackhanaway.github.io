title: "Posts"
permalink: /posts/
Research write-ups and analytical deep dives. These posts accompany the projects in my portfolio and are written to communicate findings to a baseball audience — not just a technical one.

<ul class="taxonomy__index">
  {% assign posts = site.posts %}
  {% for post in posts %}
    <li>
      <a href="{{ post.url }}">
        <strong>{{ post.title }}</strong> &mdash; {{ post.date | date: "%B %-d, %Y" }}
      </a>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>
