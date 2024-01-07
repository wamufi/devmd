---
boostnote:
  createdAt: '2022-05-13T09:44:43.685Z'
  updatedAt: '2022-05-13T09:45:09.556Z'
  type: MARKDOWN_NOTE
  folder: f9b4c2c416b7c9c01097
  title: Google Maps Marker fit
  tags: []
  linesHighlighted: []
  isStarred: false
  key: 7433f6d5-ed5f-4a20-8b9a-375b29ed068f
  storage: d112f8ec1e85e056a09d
---

Google Maps Marker fit
---

```swift
var bounds = GMSCoordinateBounds()
for location in locationArray
{
    let latitude = location.valueForKey("latitude")
    let longitude = location.valueForKey("longitude")

    let marker = GMSMarker()
    marker.position = CLLocationCoordinate2D(latitude:latitude, longitude:longitude)
    marker.map = self.mapView
    bounds = bounds.includingCoordinate(marker.position)
}

mapView.setMinZoom(1, maxZoom: 15)//prevent to over zoom on fit and animate if bounds be too small

let update = GMSCameraUpdate.fit(bounds, withPadding: 50)
mapView.animate(update)

mapView.setMinZoom(1, maxZoom: 20) // allow the user zoom in more than level 15 again

```

https://stackoverflow.com/questions/53358418/swift-how-to-fit-gmsmapview-google-map-to-show-all-markers-and-prevent-over