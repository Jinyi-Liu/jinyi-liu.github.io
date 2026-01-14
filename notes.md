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

<h3>Browse notes</h3>

<p style="margin-bottom: 8px;">
This page loads the file list directly from GitHub, so you can click a note to view it.
</p>

<div style="display:flex; gap:8px; flex-wrap:wrap; align-items:center; margin-bottom: 10px;">
  <input id="filter"
         type="text"
         placeholder="Filter notes (type to search)…"
         style="flex: 1 1 320px; max-width: 520px; padding: 8px; border: 1px solid #ccc; border-radius: 0;" />
  <span id="status" style="font-size: 0.95em;"></span>
</div>

<div id="notesList"
     style="border: 1px solid #eee; padding: 10px; max-height: 38vh; overflow: auto;">
  Loading…
</div>

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
    var viewBase = "{{ '/notes/view/' | relative_url }}";
    var input = document.getElementById("noteFile");
    var btn = document.getElementById("loadBtn");
    var iframe = document.getElementById("viewer");
    var open = document.getElementById("openNewTab");
    var listEl = document.getElementById("notesList");
    var statusEl = document.getElementById("status");
    var filterEl = document.getElementById("filter");

    function setStatus(msg) {
      if (statusEl) statusEl.textContent = msg || "";
    }

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

    function escapeHtml(s) {
      return String(s)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
    }

    function renderList(items) {
      if (!listEl) return;
      if (!items.length) {
        listEl.innerHTML = "<em>No notes found.</em>";
        return;
      }

      listEl.innerHTML = items.map(function (it) {
        // show relative path under html/
        var display = it.path.replace(/^html\//, "");
        var viewHref = viewBase + "?file=" + encodeURIComponent(display);
        return (
          '<div style="margin: 4px 0; display:flex; gap:10px; align-items:center; flex-wrap:wrap;">' +
            '<a href="' + escapeHtml(viewHref) + '">' + escapeHtml(display) + "</a>" +
            '<a href="' + escapeHtml(it.url) + '" target="_blank" rel="noopener" style="font-size:0.9em; text-decoration:none; border:1px solid #333; padding:2px 6px;">Open</a>' +
          "</div>"
        );
      }).join("");
    }

    function applyFilter(allItems) {
      var q = (filterEl && filterEl.value ? filterEl.value : "").trim().toLowerCase();
      if (!q) return allItems;
      return allItems.filter(function (it) {
        return it.path.toLowerCase().indexOf(q) !== -1;
      });
    }

    // Load list of html files from GitHub using the Trees API (recursive)
    // This avoids needing a hardcoded manifest and supports subfolders.
    var allItems = [];
    function loadIndex() {
      setStatus("Loading list…");
      if (listEl) listEl.textContent = "Loading…";

      fetch("https://api.github.com/repos/Jinyi-Liu/paper_reading_note/git/trees/main?recursive=1", {
        headers: { "Accept": "application/vnd.github+json" }
      })
      .then(function (r) {
        if (!r.ok) throw new Error("GitHub API error: " + r.status);
        return r.json();
      })
      .then(function (data) {
        var tree = (data && data.tree) ? data.tree : [];
        allItems = tree
          .filter(function (n) { return n && n.type === "blob" && typeof n.path === "string"; })
          .filter(function (n) { return n.path.startsWith("html/") && n.path.toLowerCase().endsWith(".html"); })
          .map(function (n) {
            return {
              path: n.path,
              url: "https://cdn.jsdelivr.net/gh/Jinyi-Liu/paper_reading_note@main/" + encodeURI(n.path)
            };
          })
          .sort(function (a, b) { return a.path.localeCompare(b.path); });

        setStatus(allItems.length ? ("Found " + allItems.length + " notes") : "No notes found");
        renderList(applyFilter(allItems));
      })
      .catch(function (err) {
        setStatus("Failed to load list");
        if (listEl) {
          listEl.innerHTML =
            "<p><strong>Could not load notes list.</strong></p>" +
            "<p>This is usually caused by GitHub API rate limits or network blocking.</p>" +
            '<p>You can still browse the folder here: <a href="https://github.com/Jinyi-Liu/paper_reading_note/tree/main/html">paper_reading_note/html</a></p>' +
            "<p>Error: <code>" + escapeHtml(err && err.message ? err.message : String(err)) + "</code></p>";
        }
      });
    }

    if (filterEl) {
      filterEl.addEventListener("input", function () {
        renderList(applyFilter(allItems));
      });
    }

    loadIndex();
  })();
</script>
