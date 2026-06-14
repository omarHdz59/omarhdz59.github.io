---
layout: single
title: "Buscador de Publicaciones"
permalink: /search/
author_profile: false
---

<div class="custom-search-container">
  <div class="custom-search-wrapper">
    <span class="custom-search-icon">🔍</span>
    <input 
      type="text" 
      id="global-search" 
      placeholder="Escribe para buscar por máquina, vulnerabilidad, CVE, comando, texto..." 
      onkeyup="liveSearch()"
      autocomplete="off"
    >
    <button id="clear-search-btn" onclick="resetSearch()" title="Limpiar búsqueda">✕</button>
  </div>
  <p class="custom-search-stats" id="search-results-stats">Cargando publicaciones...</p>
</div>

<hr class="cyber-divider">

<div class="custom-posts-grid" id="main-posts-container">
  {% for post in site.posts %}
    
    {% comment %} 
      Filtros estrictos de Jekyll para limpiar el texto:
      - strip_html: quita etiquetas web
      - strip_newlines: elimina saltos de línea que rompen el atributo HTML
      - escape: convierte comillas para que no rompan el string de JS
    {% endcomment %}
    {% assign clean_excerpt = post.excerpt | strip_html | strip_newlines | escape | downcase %}
    {% assign clean_title = post.title | strip_html | strip_newlines | escape | downcase %}
    {% assign clean_tags = post.tags | join: ' ' | strip_newlines | escape | downcase %}

    <article class="custom-post-card" data-search-content="{{ clean_title }} {{ clean_excerpt }} {{ clean_tags }} {{ post.date | date: '%d/%m/%Y' }}">
      <a href="{{ post.url | relative_url }}" class="custom-post-link">
        
        {% if post.header.teaser %}
          <div class="custom-post-teaser">
            <img src="{{ post.header.teaser | relative_url }}" alt="{{ post.title }}" loading="lazy">
          </div>
        {% endif %}
        
        <div class="custom-post-meta-content">
          <h3 class="custom-post-card-title">{{ post.title }}</h3>
          
          {% if post.excerpt %}
            <p class="custom-post-card-excerpt">{{ post.excerpt | strip_html | truncate: 140 }}</p>
          {% endif %}
          
          <div class="custom-post-card-footer">
            <span class="custom-post-card-date">📅 {{ post.date | date: "%d/%m/%Y" }}</span>
            <span class="custom-read-more-text">Leer Write-up →</span>
          </div>
        </div>
        
      </a>
    </article>
  {% endfor %}
</div>

<div id="no-results-msg" class="custom-no-results-alert" style="display: none;">
  🚫 No se encontraron publicaciones que coincidan con tu búsqueda.
</div>

<style>
  /* Contenedor Externo */
  .custom-search-container {
    max-width: 650px;
    margin: 40px auto 30px auto;
    padding: 0 10px;
    box-sizing: border-box;
  }

  /* La Barra de Búsqueda */
  .custom-search-wrapper {
    display: flex !important;
    align-items: center !important;
    background: #0f172a !important; 
    border: 2px solid #334155 !important;
    border-radius: 50px !important; 
    padding: 0 20px !important;
    height: 54px !important; 
    box-sizing: border-box !important;
    transition: all 0.3s ease;
  }

  .custom-search-wrapper:focus-within {
    border-color: #ef4444 !important;
    box-shadow: 0 0 15px rgba(239, 68, 68, 0.25) !important;
    background: #1e293b !important;
  }

  .custom-search-icon {
    color: #64748b;
    font-size: 1.1rem;
    margin-right: 12px;
    display: flex;
    align-items: center;
    flex-shrink: 0;
  }

  /* Reset del Input */
  #global-search {
    flex-grow: 1;
    height: 100% !important; 
    background: transparent !important; 
    border: none !important; 
    outline: none !important; 
    color: #f8fafc !important; 
    font-size: 1.05rem !important;
    padding: 0 !important; 
    margin: 0 !important;
    box-shadow: none !important; 
    appearance: none !important; 
    width: 100%;
  }

  #global-search::placeholder {
    color: #64748b !important;
  }

  /* Botón '✕' */
  .custom-search-wrapper #clear-search-btn {
    background: transparent !important;
    border: none !important;
    outline: none !important;
    box-shadow: none !important;
    color: #64748b !important;
    font-size: 1.2rem;
    cursor: pointer;
    padding: 0 0 0 10px !important;
    margin: 0 !important;
    visibility: hidden;
    flex-shrink: 0;
    display: flex;
    align-items: center;
    height: 100%;
  }

  .custom-search-wrapper #clear-search-btn:hover {
    color: #f43f5e !important;
  }

  .custom-search-stats {
    font-size: 0.85rem;
    color: #94a3b8;
    margin-top: 10px;
    font-weight: 500;
  }

  .cyber-divider {
    border: 0;
    height: 1px;
    background: linear-gradient(to right, transparent, #334155, transparent);
    margin-bottom: 45px;
  }

  /* SOLUCIÓN AL ENCOMADO: Grid Aislado con Márgenes Forzados */
  .custom-posts-grid {
    display: grid !important;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)) !important;
    gap: 35px !important; /* Espaciado amplio entre tarjetas (arriba, abajo, lados) */
    width: 100% !important;
    box-sizing: border-box !important;
    padding: 10px 0 !important;
  }
  
  .custom-post-card {
    background: #0f172a !important;
    border: 1px solid #1e293b !important;
    border-radius: 12px !important;
    overflow: hidden !important;
    display: flex !important;
    flex-direction: column !important;
    height: 100% !important; /* Fuerza a que mantengan proporciones homogéneas */
    margin: 0 !important; /* Resetea herencias que encimen elementos */
    box-sizing: border-box !important;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .custom-post-card:hover {
    transform: translateY(-5px);
    border-color: #ef4444 !important;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5) !important;
  }
  
  .custom-post-link {
    text-decoration: none !important;
    color: inherit !important;
    display: flex !important;
    flex-direction: column !important;
    height: 100% !important;
  }
  
  .custom-post-teaser img {
    width: 100% !important;
    height: 160px !important;
    object-fit: cover !important;
    border-bottom: 1px solid #1e293b !important;
    display: block !important;
  }
  
  .custom-post-meta-content {
    padding: 20px !important;
    display: flex !important;
    flex-direction: column !important;
    flex-grow: 1 !important;
  }
  
  .custom-post-card-title {
    font-size: 1.25rem !important;
    color: #f1f5f9 !important;
    margin: 0 0 12px 0 !important;
    line-height: 1.3 !important;
    font-weight: 700 !important;
  }
  
  .custom-post-card-excerpt {
    font-size: 0.88rem !important;
    color: #94a3b8 !important;
    line-height: 1.5 !important;
    margin: 0 0 20px 0 !important;
    flex-grow: 1 !important;
  }
  
  .custom-post-card-footer {
    display: flex !important;
    justify-content: space-between !important;
    align-items: center !important;
    border-top: 1px solid #1e293b !important;
    padding-top: 12px !important;
    font-size: 0.8rem !important;
    margin-top: auto !important; /* Empuja el footer siempre abajo */
  }
  
  .custom-post-card-date {
    color: #64748b !important;
  }
  
  .custom-read-more-text {
    color: #ef4444 !important;
    font-weight: 600 !important;
  }
  
  .custom-post-card:hover .custom-read-more-text {
    color: #b91c1c !important;
  }

  .custom-no-results-alert {
    text-align: center;
    padding: 40px;
    background: #0f172a;
    border: 1px dashed #334155;
    border-radius: 12px;
    color: #94a3b8;
    font-size: 1.05rem;
    margin-top: 20px;
  }
</style>

<script>
  function liveSearch() {
    const searchInput = document.getElementById('global-search');
    // Convierte a minúsculas y quita espacios vacíos extras
    const query = searchInput.value.toLowerCase().trim();
    const clearBtn = document.getElementById('clear-search-btn');
    const cards = document.querySelectorAll('.custom-post-card');
    const stats = document.getElementById('search-results-stats');
    const noResults = document.getElementById('no-results-msg');
    
    let matchCount = 0;

    // Mostrar u ocultar la equis de limpieza
    clearBtn.style.visibility = query.length > 0 ? 'visible' : 'hidden';

    cards.forEach(card => {
      const content = card.getAttribute('data-search-content');
      
      // Valida que el atributo contenga información legible
      if (content && content.indexOf(query) !== -1) {
        card.style.setProperty('display', 'flex', 'important');
        matchCount++;
      } else {
        card.style.setProperty('display', 'none', 'important');
      }
    });

    // Cambiar estadísticas en tiempo real
    if (query.length === 0) {
      stats.textContent = `Mostrando todas las publicaciones (${cards.length})`;
      noResults.style.display = 'none';
    } else {
      stats.textContent = `Resultados encontrados: ${matchCount}`;
      noResults.style.display = matchCount === 0 ? 'block' : 'none';
    }
  }

  function resetSearch() {
    const searchInput = document.getElementById('global-search');
    searchInput.value = '';
    liveSearch();
    searchInput.focus();
  }

  // Inicialización de contador
  window.addEventListener('DOMContentLoaded', () => {
    const totalCards = document.querySelectorAll('.custom-post-card').length;
    document.getElementById('search-results-stats').textContent = `Mostrando todas las publicaciones (${totalCards})`;
  });
</script>
