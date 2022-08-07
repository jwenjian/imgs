# imgs

CloudFlare workers to proxy images

```js
const GH_REPO = "jwenjian/imgs"
const GH_IMG_FOLDER = "/img/"
const GH_BRANCH = 'master'

addEventListener("fetch", event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  let url = request.url;
  let filename = url.substring(url.indexOf(GH_IMG_FOLDER), url.length)
  let img_type = filename.substring(filename.indexOf(".") + 1, filename.length)
  let gh_resp = await fetch(`https://raw.githubusercontent.com/${GH_REPO}/${GH_BRANCH}${filename}`)
  return new Response(gh_resp.body, {headers: {'Content-Type': `image/${img_type}`, 'Access-Control-Allow-Origin': '*', 'Cache-Control': 'public, max-age=604800, immutable'}})
}
```

Access url: `<worker url>/img/*.jpeg`
