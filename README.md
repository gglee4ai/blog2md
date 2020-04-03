# Blogger to Markdown

 Convert Blogger & WordPress backup blog posts to hugo compatible markdown documents 


    Usage: node index.js b|w <BLOGGER BACKUP XML> <OUTPUT DIR>

For Blogger imports, blog posts and comments (as seperate file `<postname>-comments.md`) will be created in "`out`" directory

```
    node index.js b your-blogger-backup-export.xml out
```

For WordPress imports, blog posts and comments (as seperate file `<postname>-comments.md`) will be created in "`out`" directory

```
    node index.js w your-wordpress-backup-export.xml out
```

If you want the comments to be merged in your post file itself. you can use flag `m` at the end. Defaults to `s` for seperate comments file

```
    node index.js w your-wordpress-backup-export.xml out m
```


## Installation (usual node project)

* Download or Clone this project
* `cd` to directory
* Run `npm install` to install dependencies
* Run `node index.js <arg...>`

## Notes to self

Script to convert posts from Blogger to Markdown.

- [x] Read XML
- [x] Parse Entries (Posts and comments) (with xpath?)
- [x] Parse Title, Link, Created, Updated, Content, Link
- [ ] List Post & Respective comment counts
- [x] Content to MD - pandoc?
- [ ] Parse Images, Files, Videos linked to the posts
- [x] Create output dir
- [ ] List items that are not downloaded( or can't) along with their .md file for user to proceed


## Reasons

* Wrote this to consilidate and convert my blogs under one roof.
* Plain simple workflow with `hugo` 
* Ideas was to download associated assets (images/files) linked to post. Gave up, because it was time consuming and anyhow I need to validate the markdown with assets of converted. And I don't see benefit.
* Initial assumption was to parse with `xpath` but I found `xml2json.js` was easier
* Also thought `pandoc` is a overkill and `turndown.js` was successful, though I had to wrap empty `text` to `md` instead of `html`.
* I want to retain comments. Believe it or not, There were some **good** comments.
* Was sick and spent around ~12 hrs over 5 days in coding and testing with my blog contents over ~150 posts. And also, I find parsing _oddly satisfying_ when it result in success. `¯\_(ツ)_/¯`



## 나의 추가 사항

### subscript, superscript html 보존

* 아래 라인을 추가하면 해결됨

<pre><code>
var tds = new TurndownService({ codeBlockStyle: 'fenced', fence: '```' })
tds.keep(['sup', 'sub']) # 이 라인을 추가하면 해결됨
</code></pre>


### WordPress의 엔터키를 유지

* 이 링크 참조 https://github.com/domchristie/turndown/issues/264
* /node_modules/turndown/lib/turndown.cjs.js 에서 아래와 같이 변경하면 해결됨

<pre><code>
while (node !== element) {
  if (node.nodeType === 3 || node.nodeType === 4) { // Node.TEXT_NODE or Node.CDATA_SECTION_NODE
    //Original code
    //var text = node.data.replace(/[ \r\n\t]+/g, ' ');

    //Changed code
    var text = node.data.replace(/[ \r\t]+/g, ' ');
</code></pre>
