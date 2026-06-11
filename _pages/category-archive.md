---
layout: single
title: "Posts by Category"
permalink: /categories/
author_profile: false
---

<div style="display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; width: 100%; margin: 20px 0 40px 0; padding: 0;">
  <button onclick="filterCategory('all')" class="btn btn--primary" style="margin: 0; min-width: 120px; text-align: center;">
    Mostrar Todo
  </button>
  {% for category in site.categories %}
    <button onclick="filterCategory('{{ category[0] | slugify }}')" class="btn btn--info" style="margin: 0; min-width: 120px; text-align: center;">
      {{ category[0] }} ({{ category[1].size }})
    </button>
  {% endfor %}
</div>

<hr style="border: 1px solid #415a77; margin-bottom: 30px;">

<style>
  .category-section .page__taxonomy-item {
    display: inline-block !important;
    margin-right: 10px !important;
    margin-bottom: 5px !important;
  }
  .category-section .page__taxonomy {
    display: block !important;
    text-align: left !important;
  }
</style>

<div id="categories-container" style="width: 100%;">
  {% for category in site.categories %}
    <div class="category-section" id="section-{{ category[0] | slugify }}" style="margin-bottom: 40px;">
      <h2 style="border-bottom: 2px solid #415a77; padding-bottom: 5px; color: #00b4d8; text-transform: uppercase;">
        📁 {{ category[0] }}
      </h2>
      
      <div class="entries-list">
        {% for post in category[1] %}
          {% include archive-single.html %}
        {% endfor %}
      </div>
    </div>
  {% endfor %}
</div>

<script>
  function filterCategory(categorySlug) {
    const sections = document.querySelectorAll('.category-section');
    sections.forEach(section => {
      if (categorySlug === 'all') {
        section.style.display = 'block';
      } else {
        if (section.id === 'section-' + categorySlug) {
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
      filterCategory(hash);
    }
  });
</script>
