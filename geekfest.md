---
layout: default
title: GeekFest
---

<header class="main-header no-cover geekfest">
<nav class="main-nav overlay clearfix">
    <a class="back-button icon-arrow-left" href="/">Home</a>
    {% include menu.html %}
    
</nav>
<div class="vertical">
    <div class="main-header-content inner">
        <h1 class="page-title">GeekFest</h1>
        <h2 class="page-description">
            Screencasts for developers
        </h2>
    </div>
</div>
<a class="scroll-down icon-arrow-left" href="#content" data-offset="-45"><span class="hidden">Scroll Down</span></a>
</header>

<main id="content" class="content" role="main">
{% for post in site.posts %}
{% if post.tags contains "GeekFest" %}

<article class="post">
    <header class="post-header">
        <h2 class="post-title"><a href="{{ post.url }}">{{ post.title }}</a></h2>
    </header>
        {{ post.content }}
    
</article>
{% endif %}
{% endfor %}
</main>