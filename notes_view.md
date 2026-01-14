---
layout: simple
title: Note Viewer
permalink: /notes/view/
---

<div style="display:flex; gap:10px; flex-wrap:wrap; align-items:center; margin-bottom: 12px;">
  <a href="{{ '/notes/' | relative_url }}" style="text-decoration:none; border:1px solid #333; padding:6px 10px;">← Back to list</a>
  <span id="fileLabel" style="font-size:0.95em;"></span>
  <a id="openOriginal" href="#" target="_blank" rel="noopener"
     style="text-decoration:none; border:1px solid #333; padding:6px 10px;">
    Open original
  </a>
</div>

<div id="msg" style="margin: 10px 0;"></div>

<iframe id="viewer"
        title="HTML note viewer"
        style="width: 100%; height: 85vh; border: 1px solid #ddd; background: #fff;"
        loading="lazy"
        referrerpolicy="no-referrer">
</iframe>

<script>
  (function () {
    var params = new URLSearchParams(window.location.search);
    var file = (params.get("file") || "").trim();

    var fileLabel = document.getElementById("fileLabel");
    var msg = document.getElementById("msg");
    var iframe = document.getElementById("viewer");
    var openOriginal = document.getElementById("openOriginal");

    function escapeHtml(s) {
      return String(s)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
    }

    function normalizeFile(v) {
      v = (v || "").trim();
      v = v.replace(/^\/+/, "");
      v = v.replace(/^html\//, "");
      return v;
    }

    if (!file) {
      msg.innerHTML =
        "<p><strong>No file selected.</strong> Go back to the list and click a note.</p>";
      iframe.style.display = "none";
      openOriginal.style.display = "none";
      return;
    }

    file = normalizeFile(file);
    // Source URL: jsDelivr serves .html with a renderable content-type.
    var src = "https://cdn.jsdelivr.net/gh/Jinyi-Liu/paper_reading_note@main/html/" + encodeURI(file);

    document.title = file + " | Notes";
    fileLabel.textContent = file;
    openOriginal.href = src;

    msg.innerHTML = "<p>Loading…</p>";

    fetch(src, { cache: "force-cache" })
      .then(function (r) {
        if (!r.ok) throw new Error("Failed to fetch: HTTP " + r.status);
        return r.text();
      })
      .then(function (html) {
        // Ensure relative links inside the note resolve correctly.
        // We inject a <base> tag pointing to the html/ folder.
        var baseHref = "https://cdn.jsdelivr.net/gh/Jinyi-Liu/paper_reading_note@main/html/";

        // If the note already has a <head>, inject base right after it; otherwise prepend.
        var baseTag = '<base href="' + baseHref + '">';
        var patched = html;
        if (/<head[^>]*>/i.test(patched)) {
          patched = patched.replace(/<head[^>]*>/i, function (m) { return m + baseTag; });
        } else {
          patched = baseTag + patched;
        }

        iframe.srcdoc = patched;
        msg.innerHTML = "";
      })
      .catch(function (err) {
        msg.innerHTML =
          "<p><strong>Could not render this note.</strong></p>" +
          "<p>Try “Open original”.</p>" +
          "<p>Error: <code>" + escapeHtml(err && err.message ? err.message : String(err)) + "</code></p>";
      });
  })();
</script>
