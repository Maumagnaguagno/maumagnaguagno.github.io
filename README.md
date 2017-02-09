# [maumagnaguagno.github.io](https://maumagnaguagno.github.io)
**My internets**

Based on [jekyll-minimal-theme](https://github.com/henrythemes/jekyll-minimal-theme)

```
 ___________________
|___________________| <-- Nav bar
|   | 1 Collumn |   |
|   |     |     |   | <-- Split view
|   |  L  |  R  |   |     Separations are invisible
|   |     |     |   |
|___|___________|___|
```

Apply split view using divs.
Avoid reaching 100% to create better separations

```HTML
<div class="split">
  <div markdown="1">
    Left text is here.
  </div>
  <div markdown="1">
    And the right content is here.
  </div>
</div>
```