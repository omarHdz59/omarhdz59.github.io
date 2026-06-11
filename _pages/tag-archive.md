---
layout: single
title: "Posts by Tag"
permalink: /tags/
author_profile: false
---

<div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; width: 100%; margin: 20px 0 40px 0; padding: 0;">
  <button onclick="filterTag('all')" class="btn btn--primary" style="margin: 0; min-width: 120px; text-align: center;">
    Mostrar Todo
  </button>
  {% for tag in site.tags %}
    <button onclick="filterTag('{{ tag[0] | slugify }}')" class="btn btn--info" style="margin: 0; min-width: 120px; text-align: center;">
      {{ tag[0] }} ({{ tag[1].size }})
    </button>
  {% endfor %}
</div>

<hr style="border: 1px solid #415a77; margin-bottom: 30px;">

<style>
  /* Fuerza a que los links de etiquetas dentro de estas listas no salten de línea */
  .tag-section .page__taxonomy-item {
    display: inline-block !important;
    margin-right: 10px !important;
    margin-bottom: 5px !important;
  }
  .tag-section .page__taxonomy {
    display: block !important;
    text-align: left !important;
  }
</style>

<div id="tags-container" style="width: 100%;">
  {% for tag in site.tags %}
    <div class="tag-section" id="section-{{ tag[0] | slugify }}" style="margin-bottom: 40px;">
      <h2 style="border-bottom: 2px solid #415a77; padding-bottom: 5px; color: #00b4d8;"># {{ tag[0] }}</h2>
      
      <div class="entries-list">
        {% for post in tag[1] %}
          {% include archive-single.html %}
        {% endfor %}
      </div>
    </div>
  {% endfor %}
</div>

<script>
  function filterTag(tagSlug) {
    const sections = document.querySelectorAll('.tag-section');
    sections.forEach(section => {
      if (tagSlug === 'all') {
        section.style.display = 'block';
      } else {
        if (section.id === 'section-' + tagSlug) {
          section.style.display = 'block';
        } else {
          section.style.display = 'none';
        }
      }
    });
  }

  window.addEventListener('DOMContentLoaded', () => {
    const hash = window.location.hash.replace('#', '');
    if (hash) {
      filterTag(hash);
    }
  });
</script>
