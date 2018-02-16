# hoth4-api-workshop
API workshop at Hack on the Hill

Slides: [link here](https://docs.google.com/presentation/d/1YEgcMFwmaE4JY3IDK3QxzQHgyrVeyOmiA6DKoMgxDP0/edit?usp=sharing)

The final product: [https://uclaacm.github.io/hoth4-api-workshop/](https://uclaacm.github.io/hoth4-api-workshop/)

Demo Part I

In this part, we are going to use an API provided by this workshop. This API offers the url https://api.hoth4.timothygu.me/memes, which will give you back an array of memes. Each meme object has these properties:

image (image url)
fb (Facebook url)
position (an object containing the longitude and latitude coordinates)

We're going to use these properties later on in the demo. 

```html
<!DOCTYPE html>
<script>
// async allows the browser to do other things while running this function
async function getMemes() {
  // await makes sure that response is filled before continuing
  const response = await fetch('https://api.hoth4.timothygu.me/memes');
  const memes = await response.json();
  console.log(memes);
  return memes;
}
getMemes();
</script>
```

Demo Part II

Companies usually require you to have an account to get a key for their API (so they can keep track of who's using it and make sure it's not abused). 
To get the Google Maps API key, go to: https://developers.google.com/maps/documentation/javascript/ 
Click "Get a key", make a new project, and you should get a key. The key looks like a long string of letters and numbers. 

Copy and paste the following code into your index.html file, replacing the previous code. 
Notice that the top part is the same as before, except we don't call getMemes() until later.

At the bottom, replace ```YOUR_API_KEY_HERE``` with the API key you got previously. If you're having troubles with this, use our API Key: ```AIzaSyD0AVr2hv7dSnCQtSIoyleEZ4xinq_oSFM```

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
