---
layout: simple
title: Notes
permalink: /notes/
---

<h2>Paper reading notes (HTML)</h2>

<p>
My paper reading notes are maintained in <a href="https://github.com/Jinyi-Liu/paper_reading_note">Jinyi-Liu/paper_reading_note</a>.
Some notes are exported as <code>.html</code> so they can be viewed correctly in the browser.
</p>

<p>
Base URL:
<code id="baseUrl">https://cdn.jsdelivr.net/gh/Jinyi-Liu/paper_reading_note@main/html/</code>
</p>

<hr>

<h3>Open a note</h3>

<p style="margin-bottom: 8px;">
Enter an HTML filename from the <code>html/</code> folder (for example: <code>my_note.html</code>), then click “Load”.
</p>

<div style="display:flex; gap:8px; flex-wrap:wrap; align-items:center;">
  <input id="noteFile"
         type="text"
         placeholder="e.g. xxx.html"
         style="flex: 1 1 320px; max-width: 520px; padding: 8px; border: 1px solid #ccc; border-radius: 0;" />
  <button id="loadBtn"
          type="button"
          style="padding: 8px 12px; border: 1px solid #333; background: #fff; cursor: pointer;">
    Load
  </button>
  <a id="openNewTab"
     href="#"
     target="_blank"
     rel="noopener"
     style="padding: 8px 12px; border: 1px solid #333; text-decoration: none;">
    Open in new tab
  </a>
</div>

<p style="margin-top: 10px; font-size: 0.95em;">
If you’re not sure about the filename, browse the folder here:
<a href="https://github.com/Jinyi-Liu/paper_reading_note/tree/main/html">paper_reading_note/html</a>.
</p>

<div style="margin-top: 14px;">
  <iframe id="viewer"
          title="HTML note viewer"
          style="width: 100%; height: 80vh; border: 1px solid #ddd; background: #fff;"
          loading="lazy"
          referrerpolicy="no-referrer">
  </iframe>
</div>

<script>
  (function () {
    var base = "https://cdn.jsdelivr.net/gh/Jinyi-Liu/paper_reading_note@main/html/";
    var input = document.getElementById("noteFile");
    var btn = document.getElementById("loadBtn");
    var iframe = document.getElementById("viewer");
    var open = document.getElementById("openNewTab");

    function normalizeFilename(v) {
      v = (v || "").trim();
      // allow users to paste full URLs; if so, use as-is
      if (/^https?:\/\//i.test(v)) return v;
      // strip leading slashes / folder prefix if pasted
      v = v.replace(/^\/+/, "");
      v = v.replace(/^html\//, "");
      return base + encodeURI(v);
    }

    function loadFromInput() {
      var src = normalizeFilename(input.value);
      iframe.src = src;
      open.href = src;
    }

    btn.addEventListener("click", loadFromInput);
    input.addEventListener("keydown", function (e) {
      if (e.key === "Enter") loadFromInput();
    });

    // Support /notes/?file=xxx.html
    var params = new URLSearchParams(window.location.search);
    var file = params.get("file");
    if (file) {
      input.value = file;
      loadFromInput();
    }
  })();
</script>
