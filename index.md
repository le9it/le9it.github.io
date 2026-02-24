---
layout: page
title: Posts
permalink: /
---

<div class="archive-tags">
  <a href="#all" class="tag-button" onclick="showAll()">Show All</a>
  {% assign tags = site.tags | sort %}
  {% for tag in tags %}
    <a href="#{{ tag[0] | slugify }}" class="tag-button">{{ tag[0] }}</a>
  {% endfor %}
</div>

<div id="archive-content">
  <div id="all-posts-section">
    <ul class="post-list">
      {% for post in site.posts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %d, %Y" }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
      </li>
      {% endfor %}
    </ul>
  </div>

  {% for tag in tags %}
  <div class="tag-section" id="{{ tag[0] | slugify }}" style="display: none;">
    <h2 class="post-list-heading">{{ tag[0] | capitalize }}</h2>
    <ul class="post-list">
      {% for post in tag[1] %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %d, %Y" }}</span>
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
      </li>
      {% endfor %}
    </ul>
  </div>
  {% endfor %}
</div>

<style>
  /* Sorting Buttons styling */
  .archive-tags { margin-bottom: 30px; border-bottom: 1px solid var(--border-color); padding-bottom: 20px; }
  
  .tag-button {
    display: inline-block;
    padding: 6px 15px;
    margin: 4px;
    background: rgba(255, 255, 255, 0.05) !important;
    border: 1px solid var(--border-color) !important;
    color: var(--link-color) !important;
    border-radius: 4px;
    text-decoration: none;
    font-size: 0.9em;
    cursor: pointer;
  }
  .tag-button:hover { background: var(--link-color) !important; color: white !important; }

  /* Official Minima Post List Styling */
  .post-list { margin-left: 0; list-style: none; }
  .post-list > li { margin-bottom: 30px; }
  .post-meta { font-size: 14px; color: #828282; }
  .post-link { display: block; font-size: 24px; font-weight: 400; }
  
  .post-list-heading {
    font-size: 28px;
    margin-bottom: 20px;
    padding-top: 10px;
  }
</style>

<script>
  function applyFilter() {
    const hash = window.location.hash.toLowerCase();
    const allPosts = document.getElementById('all-posts-section');
    const sections = document.querySelectorAll('.tag-section');
    
    if (!hash || hash === '#all') {
      allPosts.style.display = 'block';
      sections.forEach(s => s.style.display = 'none');
      return;
    }

    allPosts.style.display = 'none';
    const targetId = hash.replace('#', '');
    sections.forEach(s => {
      s.id === targetId ? s.style.display = 'block' : s.style.display = 'none';
    });
  }

  function showAll() { window.location.hash = 'all'; }
  window.addEventListener('hashchange', applyFilter);
  window.addEventListener('load', applyFilter);
</script>
