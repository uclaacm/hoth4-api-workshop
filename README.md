# hoth4-api-workshop
API workshop at Hack on the Hill

Slides: [link here](https://docs.google.com/presentation/d/1YEgcMFwmaE4JY3IDK3QxzQHgyrVeyOmiA6DKoMgxDP0/edit?usp=sharing)

The final product: [https://uclaacm.github.io/hoth4-api-workshop/](https://uclaacm.github.io/hoth4-api-workshop/)

##Demo Part I

In this part, we are going to use an API provided by this workshop. This API offers the endpoint https://api.hoth4.timothygu.me/memes, which will return text that looks like this:
```
[
  {
    "image": "https://scontent-lax3-1.xx.fbcdn.net/v/t1.0-9/27656971_1644569528954328_458318846514208241_n.jpg?oh=eb48e7f9c9317aa99092ab59e372ccbb&oe=5B1CD4F0",
    "fb": "https://www.facebook.com/groups/163576114113950/permalink/393948747743351/",
    "position": {
      "lat": 34.07227,
      "lng": -118.452014
    }
  },
  {
    "image": "https://scontent-lax3-1.xx.fbcdn.net/v/t1.0-9/27544612_10215514514362124_7612462266016096164_n.jpg?oh=052b4c69a1b613fc1996bfbe471c5c14&oe=5B1A5445",
    "fb": "https://www.facebook.com/groups/163576114113950/permalink/393954784409414/",
    "position": {
      "lat": 34.07286,
      "lng": -118.449968
    }
  }
  ... more memes here ...
 ]
```
This format is called JSON (JavaScript Object Notation). 

Notice that each meme object has these three properties:

* image (image url)
* fb (Facebook url)
* position (an object containing the longitude and latitude coordinates)

We're going to use these properties later on in the demo.

The following function goes to the endpoint https://api.hoth4.timothygu.me/memes, parses the text it gets as JSON, and then returns the array of memes. 

```html
<!DOCTYPE html>
<script>
// async means that this function is taken out of the main program flow
// this allows you to use await
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

Put this code into your index.html file. Open index.html with your browser and right-click -> "Inspect element" so you can view the console. You should see the array of meme objects that we printed when we called the function. 

##Demo Part II

Now we're going to use another API from Google Maps in order to put all the memes we just got onto a map. 

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
    // When you click on the marker, you'll go to the Facebook link of the meme
    marker.addListener('click', () => {
      window.open(meme.fb);
    });
  }
}
</script>
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY_HERE&amp;callback=initMap"></script>
```
