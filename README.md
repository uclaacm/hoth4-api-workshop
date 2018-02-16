# hoth4-api-workshop
API workshop at Hack on the Hill

Slides: [link here](https://docs.google.com/presentation/d/1YEgcMFwmaE4JY3IDK3QxzQHgyrVeyOmiA6DKoMgxDP0/edit?usp=sharing)

The final product: [https://uclaacm.github.io/hoth4-api-workshop/](https://uclaacm.github.io/hoth4-api-workshop/)

```html
<!DOCTYPE html>
<script>
async function getMemes() {
  const response = await fetch('https://api.hoth4.timothygu.me/memes');
  const memes = await response.json();
  console.log(memes);
  return memes;
}
getMemes();
</script>
```

Demo Part II
```html
<!DOCTYPE html>
<script>
async function getMemes() {
  const response = await fetch('https://api.hoth4.timothygu.me/memes');
  const memes = await response.json();
  console.log(memes);
  return memes;
}
</script>

<div id="map" style="width: 100%; height: 100vh"></div>
<script>
async function initMap() {
  // Create a Map centered on UCLA.
  const map = new google.maps.Map(document.getElementById('map'), {
    center: { lat: 34.071, lng: -118.446 },
    zoom: 15
  });
  // Get dat hot memes.
  const memes = await getMemes();
  // Create a marker on the map for every meme.
  for (const meme of memes) {
    const marker = new google.maps.Marker({
      map: map,
      position: meme.position,
      icon: {
        url: meme.image,
        scaledSize: {
          height: 75,
          width: 75
        }
      }
    });
    marker.addListener('click', () => {
      window.open(meme.fb);
    });
  }
}
</script>
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY_HERE&amp;callback=initMap"></script>
```
