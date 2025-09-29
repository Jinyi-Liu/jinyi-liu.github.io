<h1 id="research"></h1>

<h2 style="margin: 60px 0px -15px;">Publications <temp style="font-size:15px;">


<div class="publications">
<ol class="bibliography">

{% assign idx = 0 %}
{% for link in site.data.publications.main %}

<li>
<div class="pub-row">
  <!-- <div class="col-sm-3 abbr" style="position: relative;padding-right: 15px;padding-left: 15px;"><abbr class="badge">{{ link.conference_short }}</abbr></div> -->
  <div class="col-sm-9" style="position: relative;padding-right: 15px;padding-left: 20px;">
      {% assign idx = idx | plus: 1 %}
      <div class="title"><span class="badge label-num">{{ idx }}</span> <a href="{{ link.pdf }}">{{ link.title }}</a></div>
      <div class="author">{{ link.authors }}</div>
      <div class="periodical"><em>{{ link.conference }}</em>
      </div>
    <div class="links">
      <!-- {% if link.pdf %} 
      <a href="{{ link.pdf }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">PDF</a>
      {% endif %} -->
      {% if link.code %} 
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Code</a>
      {% endif %}
      <!-- {% if link.page %} 
      <a href="{{ link.page }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project Page</a>
      {% endif %}
      {% if link.data %} 
      <a href="{{ link.data }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Dataset</a>
      {% endif %}
      {% if link.bibtex %} 
      <a href="{{ link.bibtex }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">BibTex</a>
      {% endif %} -->
      {% if link.notes %} 
      <strong> <i style="color:#e74d3c; font-weight:600">{{ link.notes }}</i></strong>
      {% endif %}
      {% if link.others %} 
      {{ link.others }}
      {% endif %}
    </div>
  </div>
</div>
</li>

<!-- <br> -->

{% endfor %}

</ol>
</div>


{% if site.data.publications.wp %}
<h2 style="margin: 40px 0px -15px;">Working papers</h2>
<div class="publications">
<ol class="bibliography">
{% for link in site.data.publications.wp %}
<li>
<div class="pub-row">
  <!-- <div class="col-sm-3 abbr" style="position: relative;padding-right: 15px;padding-left: 15px;"><abbr class="badge">{{ link.conference_short }}</abbr></div> -->
  <div class="col-sm-9" style="position: relative;padding-right: 15px;padding-left: 20px;">
      {% assign idx = idx | plus: 1 %}
      <div class="title"><span class="badge label-num">{{ idx }}</span> {{ link.title }}</div>
      {% if link.authors %}<div class="author">{{ link.authors }}</div>{% endif %}
      {% if link.conference %}<div class="periodical"><em>{{ link.conference }}</em></div>{% endif %}
      {% if link.notes %}<div class="periodical"><em>{{ link.notes }}</em></div>{% endif %}
    <div class="links">
      <!-- {% if link.pdf %}
      <a href="{{ link.pdf }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">PDF</a>
      {% endif %} -->
      {% if link.code %}
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Code</a>
      {% endif %}
      <!-- {% if link.page %}
      <a href="{{ link.page }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project Page</a>
      {% endif %}
      {% if link.data %}
      <a href="{{ link.data }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Dataset</a>
      {% endif %}
      {% if link.bibtex %}
      <a href="{{ link.bibtex }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">BibTex</a>
      {% endif %} -->
      {% if link.others %}
      {{ link.others }}
      {% endif %}
    </div>
  </div>
</div>
</li>
<br>
{% endfor %}
</ol>
</div>
{% endif %}


</ol>
</div>


{% if site.data.publications.wip %}
<h2 style="margin: 40px 0px -15px;">Work in progress</h2>
<div class="publications">
<ol class="bibliography">
{% for link in site.data.publications.wip %}
<li>
<div class="pub-row">
  <!-- <div class="col-sm-3 abbr" style="position: relative;padding-right: 15px;padding-left: 15px;"><abbr class="badge">{{ link.conference_short }}</abbr></div> -->
  <div class="col-sm-9" style="position: relative;padding-right: 15px;padding-left: 20px;">
      {% assign idx = idx | plus: 1 %}
      <div class="title"><span class="badge label-num">{{ idx }}</span> {{ link.title }}</div>
      {% if link.authors %}<div class="author">{{ link.authors }}</div>{% endif %}
      {% if link.conference %}<div class="periodical"><em>{{ link.conference }}</em></div>{% endif %}
      {% if link.notes %}<div class="periodical"><em>{{ link.notes }}</em></div>{% endif %}
    <div class="links">
      <!-- {% if link.pdf %}
      <a href="{{ link.pdf }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">PDF</a>
      {% endif %} -->
      {% if link.code %}
      <a href="{{ link.code }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Code</a>
      {% endif %}
      <!-- {% if link.page %}
      <a href="{{ link.page }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Project Page</a>
      {% endif %}
      {% if link.data %}
      <a href="{{ link.data }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">Dataset</a>
      {% endif %}
      {% if link.bibtex %}
      <a href="{{ link.bibtex }}" class="btn btn-sm z-depth-0" role="button" target="_blank" style="font-size:12px;">BibTex</a>
      {% endif %} -->
      {% if link.others %}
      {{ link.others }}
      {% endif %}
    </div>
  </div>
</div>
</li>
<br>
{% endfor %}
</ol>
</div>
{% endif %}


