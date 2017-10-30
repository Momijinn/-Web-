Sample-to-move-markers-with-GoogleMapAPI
====
GoogleMapで２点間をつなぎ、その間をマーカーが動くwebアプリ

## Description
GoogleMapsAPIを使用し、行きと帰りのマーカーを設定すると行きからマーカーが出現し、アニメーションでゴールまでいく面白いWebアプリのサンプル

## Demo
http://htmlpreview.github.io/?https://github.com/Momijinn/-Web-/blob/master/index.html

## Requirement
* [Google Developer API](https://console.developers.google.com/apis/library?hl=ja&project=smart-surf-146714&pli=1)

## Usage
javascirptを読む
```javascirpt
function initMap() {
  var directionsService = new google.maps.DirectionsService;
  var directionsDisplay = new google.maps.DirectionsRenderer;
  var origin_address;
  var destination_address;

  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 8,
    center: {lat: 36.578057, lng: 136.64866}
  });
  directionsDisplay.setMap(map);

  document.getElementById('go_btn').addEventListener('click', function() {
      origin_address = document.getElementById('origin_address').value;
      estination_address = document.getElementById('destination_address').value;

      calculateAndDisplayRoute(map, directionsService, directionsDisplay, origin_address, estination_address);
  });
}

//set Directions
function calculateAndDisplayRoute(map, directionsService, directionsDisplay, origin_address, destination_address) {
    var marker;

    directionsService.route({
        origin: origin_address,
        destination: destination_address,
        travelMode: google.maps.DirectionsTravelMode.WALKING
    },function(response, status) {
        if (status === google.maps.DirectionsStatus.OK) {
            directionsDisplay.setDirections(response);

            marker = setMarker(map, new google.maps.LatLng(response.routes[0].bounds.f.b, response.routes[0].bounds.b.f));

            movemarker(marker, 0, response.routes[0].overview_path.length, response.routes[0].overview_path);
        }else {
            window.alert('Directions request failed due to ' + status);
        }
    });

    document.getElementById('clr_btn').addEventListener('click', function() {
        marker.setMap(null);
    });
}

//set Marker
function setMarker(map, locate){
    return new google.maps.Marker({
        position:locate,
        map:map,
        title:'my'
    });
}

//moveing marker 再帰処理
function movemarker(marker, step, fin_steps, locates){
    marker.setPosition(new google.maps.LatLng(locates[step].lat(), locates[step].lng()));

    if(step <= fin_steps){
        setTimeout(function(){
            movemarker(marker, step + 1, fin_steps, locates);
            },25);
        }
}
```

## Install
YOUR_API_KEYのところに取得したGoogleDeveloperAPIを入れる
```html
<!-- <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&signed_in=true&callback=initMap"async defer></script>-->

<!-- example -->
<script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCNN59_V7kxt_6Vha-BNFGOfvqQjb7yW9g&signed_in=true&callback=initMap"
        async defer></script>
```

## Licence
This software is released under the MIT License, see LICENSE.

## Author
[Twitter](https://twitter.com/momijinn_aka)

[Blog](http://www.autumn-color.com/)