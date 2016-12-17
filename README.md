# [maumagnaguagno.github.io](https://maumagnaguagno.github.io)
**My internets**

Based on [jekyll-minimal-theme](https://github.com/henrythemes/jekyll-minimal-theme)

```
 __________________
|__________________| <-- Nav bar (Home, ...)
|       |          |
|       |          | <-- Split view
|       |          |     Separations may be invisible
|       |          |
|_______|__________|
```

Apply split view using divs.
Avoid reaching 100% to create better separations

```HTML
<div class="split" markdown="1">
  <div style="width:45%">
    Left text is here.
  </div>
  <div style="width:45%" markdown="1">
    And the right content is here.
  </div>
</div>
```