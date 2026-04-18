---
#
# By default, content added below the "---" mark will appear in the home page
# between the top bar and the list of recent posts.
# To change the home page layout, edit the _layouts/home.html file.
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
#
layout: default
---

<script>
(function () {
  var PALETTES = [
    { key: 'blue',   color: '#60a5fa', label: 'Blue' },
    { key: 'green',  color: '#4ade80', label: 'Green' },
    { key: 'purple', color: '#c084fc', label: 'Purple' }
  ];

  function getSaved() {
    try { return localStorage.getItem('resume-color-theme') || 'blue'; } catch (e) { return 'blue'; }
  }

  function applyTheme(key) {
    document.documentElement.classList.remove('theme-purple', 'theme-green');
    if (key === 'purple') { document.documentElement.classList.add('theme-purple'); }
    else if (key === 'green') { document.documentElement.classList.add('theme-green'); }
    try { localStorage.setItem('resume-color-theme', key); } catch (e) {}
    document.querySelectorAll('.rcp-swatch').forEach(function (s) {
      s.classList.toggle('rcp-active', s.getAttribute('data-theme') === key);
    });
  }

  function init() {
    var picker = document.createElement('div');
    picker.id = 'resume-color-picker';
    picker.style.cssText = 'position:fixed;bottom:24px;right:24px;z-index:9999;display:flex;align-items:center;gap:7px;padding:8px 14px;border-radius:999px;backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);background:rgba(30,50,100,0.35);border:1px solid rgba(255,255,255,0.18);box-shadow:0 4px 20px rgba(0,0,0,0.2);';

    PALETTES.forEach(function (p) {
      var btn = document.createElement('button');
      btn.className = 'rcp-swatch';
      btn.setAttribute('data-theme', p.key);
      btn.setAttribute('title', p.label);
      btn.style.cssText = 'width:14px;height:14px;border-radius:50%;border:2px solid rgba(255,255,255,0.4);cursor:pointer;padding:0;background:' + p.color + ';transition:transform 0.2s,opacity 0.2s,border-color 0.2s;opacity:0.5;';
      btn.addEventListener('mouseenter', function () { if (!btn.classList.contains('rcp-active')) { btn.style.opacity = '0.85'; } });
      btn.addEventListener('mouseleave', function () { if (!btn.classList.contains('rcp-active')) { btn.style.opacity = '0.5'; } });
      btn.addEventListener('click', function () { applyTheme(p.key); });
      picker.appendChild(btn);
    });

    document.body.appendChild(picker);

    var style = document.createElement('style');
    style.textContent = '.rcp-active{opacity:1 !important;transform:scale(1.35);border-color:rgba(255,255,255,0.92) !important;}';
    document.head.appendChild(style);

    applyTheme(getSaved());
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', init);
  } else {
    init();
  }

  try {
    var saved = getSaved();
    if (saved === 'purple') { document.documentElement.classList.add('theme-purple'); }
    else if (saved === 'green') { document.documentElement.classList.add('theme-green'); }
  } catch (e) {}
})();
</script>
