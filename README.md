# Bookmarklets
Handy bookmarklet for various tasks.

## Make Google results URLs selectable

Shows results URL as selectable text on the top on the page.
```js
javascript:window.d||(d=document.querySelector('#extabar'),p=document.createElement('pre'),d.appendChild(p),p.innerText+='\n',document.querySelectorAll('h3').forEach(e=>{if(e.parentNode.href){p.innerText+='\n'+e.parentNode.href}}));
```

<details>
<summary>Unminified code</summary>

```js
let d = document.querySelector('#extabar')
let p = document.createElement('pre')
d.appendChild(p)
p.innerText += '\n'
document.querySelectorAll('h3').forEach(e => {
    if(e.parentNode.href){
        p.innerText += '\n' + e.parentNode.href
    }
})
```
</details>

$~$

## Force mono audio output
Creates a mono copy of the first audio source found on the page. Useful when a video on Youtube has sound on one side only.

```js
javascript:c=new AudioContext();s=c.createMediaElementSource(document.querySelector('video'));x=c.createChannelSplitter();m=c.createChannelMerger();s.connect(x);x.connect(m,0,0);x.connect(m,0,1);m.connect(c.destination)
```

<details>
<summary>Unminified code</summary>

```js
let context = new AudioContext();
let source = context.createMediaElementSource(document.querySelector('video'));
let splitter = context.createChannelSplitter();
let merger = context.createChannelMerger();
source.connect(splitter);
splitter.connect(merger, 0, 0);
splitter.connect(merger, 0, 1);
merger.connect(context.destination)
```
</details>

$~$

## Generate playlist from Apache dirlisting
Finds all links pointing to video files in the current page and format them as a .pls file, usable in VLC.

```js
javascript:l=[...document.querySelectorAll(['mkv','avi','mp4','mp3','ogg','webm','m4v'].map((e=>`[href$='.${e}']`)).join(','))],f=e=>e.split('/').filter((e=>e.length)).pop(),d=document.createElement('a'),d.setAttribute('href','data:text/plain;charset=utf-8,'+encodeURIComponent('[playlist]\n\n'+l.map(((e,t)=>`File${++t}=${e.href}\nTitle${t}=${f(e.href)}\nLength${t}=-1`)).join('\n')+`\n\nNumberOfEntries=${l.length}\nVersion=2`)),d.setAttribute('download',f(document.location.href)+'.pls'),d.style.display='none',document.body.appendChild(d),d.click(),document.body.removeChild(d);
```

<details>
<summary>Unminified code</summary>

```js
// Find all links pointing to video files
let cssSelector = ['mkv', 'avi', 'mp4', 'mp3', 'ogg', 'webm', 'm4v'].map(s => `[href$='.${s}']`).join(',');
let links = [...document.querySelectorAll(cssSelector)];
let urlToFilename = (s) => s.split('/').filter(a => a.length).pop();

// Format links as a .pls file
let playlistContent = '[playlist]\n\n' + links.map((e, i) => `File${++i}=${e.href}\nTitle${i}=${urlToFilename(e.href)}\nLength${i}=-1`).join('\n') + `\n\nNumberOfEntries=${links.length}\nVersion=2`;

// Download the generated file
let downloadLink = document.createElement('a');
downloadLink.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(playlistContent));
downloadLink.setAttribute('download', urlToFilename(document.location.href) + '.pls');
downloadLink.style.display = 'none';
document.body.appendChild(downloadLink);
downloadLink.click();
document.body.removeChild(downloadLink);
```
</details>
