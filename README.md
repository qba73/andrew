# andrew

I wanted an http server that allows me to add a simple annotation into an index.html that is replaced
with the contents of any html files that are below the current index.html in the file system hierarchy.

It's grown a little to include a small sitemap generator.  

## invocation
andrew -h to see the help

andrew accepts up to three arguments, in this order:
```bash
andrew [contentRoot] [address] [baseUrl]
```
contentRoot is the directory you're serving from, that contains your top level index.html. andrew follows
apache's lead on expecting index.html in any directory as a default page.

address is the address you want to bind the server to. Specify as an address:port combination.

baseUrl is the hostname you're serving from. This is a part of sitemaps and rss feeds. It contains the protocol
e.g. `https://playtechnique.io`


## rendering the .AndrewIndexBody
Given this file system structure:
```text
index.html
articles/
        index.html
        article-1.html
        article-2.html
        article-2.css
        article-1.js
fanfics/
        index.html
        story-1/
                potter-and-draco.html
        story-2/
                what-if-elves-rode-mice-pt1.html
                what-if-elves-rode-mice-pt2.html
```

if articles/index.html contains `{{ .AndrewIndexBody }}` anywhere, that will be replaced with:

```html
    <a class="andrewindexbodylink" id="andrewindexbodylink0" href="article-1.html">article 1</a>
    <a class="andrewindexbodylink" id="andrewindexbodylink1" href="article-2.html">article 2</a>
```

if fanfics/index.html contains `{{ .AndrewIndexBody }}`, that'll be replaced with:

```html
    <a class="andrewindexbodylink" id="andrewindexbodylink0" href="story-1/potter-and-draco.html">Potter and Draco</a>
    <a class="andrewindexbodylink" id="andrewindexbodylink0" href="story-2/what-if-elves-rode-mice-pt1.html">what-if-elves-rode-mice-pt1.html</a>
    <a class="andrewindexbodylink" id="andrewindexbodylink0" href="story-2/what-if-elves-rode-mice-pt1.html">what-if-elves-rode-mice-pt2.html</a>
```

## page titles
If a page contains a `<title>` element, Andrew picks it up and uses that as the name of a link.
If the page does not contain a `<title>` element, then Andrew will use the file name of that file as the link name.

## ordering of pages
In this release, Andrew serves you page links ascii-betically.

## sitemap.xml
When the endpoint `baseUrl/sitemap.xml` is visited, Andrew will automatically generate a sitemap containing paths to all html pages.

## server
`go install github.com/playtechnique/andrew/cmd`
