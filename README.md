# Place Picker - Demo Project in React

<p>
This dummy project demonstrates topics on “Side Effect” and “Use effect”

Side Effects are tasks that do not impact the current component render cycle

Navigator:
It is an object that is provided by the browser. This object has a geolocation object which has a method getCurrentPosition that gives the location of the user, who is browsing the web site. It accepts a callback function which will be run by the browser when the location has been fetched. This method takes time by the browser and it can last a couple of milliseconds or seconds.

navigator.geolocation.getCurrentPosition( () => { } );

navigator.geolocation.getCurrentPosition((position) => {
const sortPlaces = sortPlacesByDistance( //sortPlacesByDistance is from loc.js
AVAILABLE_PLACES,
position.coords.latitude,
position.coords.longitude
);
});

This code is not directly related to the App component. The only code related to the App component is JSX code. This code gives a side effect because it is not related to tasks of the rendering component.  
We have to use sortPlaces everywhere in this App. But it takes time to get places, therefore when we use it before fetching we will get an error as it is undefined or we will get an infinite loop because we have uses update State function in that code. This is a side effect and in order to avoid such a situation we have to do something called the use effect.

useEffect():
useEffect does not not return a value unlike useState or useRef. useEffect accepts two parameters; one is a anonymous callback function and this function has the code that gives side effect, and the other is the array of dependency of that function

useEffect(() => {
// browser gives the location of the user who is browsing the website
navigator.geolocation.getCurrentPosition((position) => {
const sortPlaces = sortPlacesByDistance(
AVAILABLE_PLACES,
position.coords.latitude,
position.coords.longitude
);
setAvailablePlaces(sortPlaces);
});
}, []);

This useEffect function runs only when the App component is rendered and returns its JSX code.
As the dependency array is empty; [], therefore useEfffect() runs only once after the App component is finished/rendered.
If there is no dependency array in useEffect(), then the useEffect() might run after whenever the App component is rendered again due to the state update function. Therefore, we will get an infinite loop. But in case of an empty dependency array, we will not get an infinite loop.

We have to avoid useEffect if we do not need it, because it is the extra rendering cycle of the component.

We use a localStorage that is given by a browser.
localStorage’s setItem allows us to store some data in the browser storage and that data will be available if we leave the website and come back later or we reload the website.

    localStorage.setItem('selectedPlaces', JSON.stringify([]))

Stringify accepts an array

function handleSelectPlace(id) {
setPickedPlaces((prevPickedPlaces) => {
if (prevPickedPlaces.some((place) => place.id === id)) {
return prevPickedPlaces;
}
const place = AVAILABLE_PLACES.find((place) => place.id === id);
return [place, ...prevPickedPlaces];
});
const storedIds = JSON.parse(localStorage.getItem("selectedPlaces")) || [];
if (storedIds.indexOf(id) === -1) {
localStorage.setItem(
"selectedPlaces",
JSON.stringify([id, ...storedIds])
);
}
}

</p>
