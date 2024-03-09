🌐 **可以前往[ReadingHere](https://www.readinghere.com/)进行css的学习**
## CSS 代码片段增强 Obsidian 视觉效果 
 ![[Pasted image 20221220172543.png]]
### 🔳鼠标移动到图片上图片自动放大
```css
.markdown-preview-view img 
{
  display: block;
  margin-top: 20pt;
  margin-bottom: 20pt;
  margin-left: auto;
  margin-right: auto;
  width: 50%; /* experiment with values */
  transition: transform 0.25s ease;
}

.markdown-preview-view img:hover 
{
  -webkit-transform: scale(1.8); /* experiment with values */
  transform: scale(2);
}
/* 作者: https://forum.obsidian.md/u/den/summary */
/* 来源: https://forum.obsidian.md/t/meta-post-common-css-hacks/1978/29 */

```
### 🔳修改左栏的文件-添加emoji
```css
div[data-path="Attachments"] .nav-folder-title-content::before
{
  content: "\1f600";
}

div[data-path="Resources"] .nav-folder-title-content::before
{
  content: "\1f33a";
}

div[data-path="Notes"] .nav-folder-title-content::before
{
  content: "\1f4cb";
}

div[data-path="Daily"] .nav-folder-title-content::before
{
  content: "\1f5d3";
}
//文件名自定义,就上面双引号里的
```
 🌐上面代码中的字符编码与图片对应关系可参见[Unicode for emoji](http://www.unicode.org/emoji/charts/full-emoji-list.html)

