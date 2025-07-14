---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

<!-- 🔒 隐藏预览内容：每个标签取 chapter_number == 0 的那篇文章 -->
<div id="tag-previews" style="display: none;">
  {% for tag in site.tags %}
    {% assign preview_post = tag[1] | where: "chapter_number", 0 | first %}
    {% if preview_post %}
      <div id="preview-{{ tag[0] | slugify }}">
        {{ preview_post.content | markdownify | strip_html | truncate: 200 }}
      </div>
    {% endif %}
  {% endfor %}
</div>

<!-- 📚 按标签分类展示文章，每个标签下的文章按 chapter_number 倒序排列 -->
{% assign ordered_tags = site.tags | sort %}
{% for tag in ordered_tags %}
  <h2>{{ tag[0] }}</h2>
  <ul>
    {% assign posts_sorted = tag[1] | sort: 'chapter_number' | reverse %}
    {% for post in posts_sorted %}
      <li>
        <a href="{{ post.url }}" class="preview-link" data-tag="{{ tag[0] | slugify }}">
          {{ post.title }}
        </a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}

<!-- 💅 悬浮预览框的样式 -->
<style>
.preview-box {
  position: absolute;
  max-width: 320px;
  padding: 12px;
  background: #fffefc;
  border: 1px solid #ccc;
  box-shadow: 2px 2px 8px rgba(0,0,0,0.15);
  font-size: 14px;
  line-height: 1.4;
  display: none;
  z-index: 9999;
  border-radius: 4px;
  pointer-events: none;
  color: #333;
}
.post-list { display: none; }
</style>

<!-- 🧠 预览交互逻辑 -->
<div id="preview-box" class="preview-box"></div>
<script>
document.addEventListener("DOMContentLoaded", function () {
  const previewBox = document.getElementById("preview-box");

  document.querySelectorAll(".preview-link").forEach(link => {
    link.addEventListener("mouseover", function (e) {
      const tagSlug = this.dataset.tag;
      const previewContent = document.getElementById("preview-" + tagSlug);
      if (previewContent) {
        previewBox.innerHTML = previewContent.innerHTML;
        previewBox.style.display = "block";
      }
    });

    link.addEventListener("mousemove", function (e) {
      previewBox.style.top = (e.pageY + 12) + "px";
      previewBox.style.left = (e.pageX + 12) + "px";
    });

    link.addEventListener("mouseout", function () {
      previewBox.style.display = "none";
    });
  });
});
</script>
