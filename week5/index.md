<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
# HIT238 The mobile paradigm



<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
## Build Tools


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Many tasks to automate
* Minimise javascript
* Minimise CSS
* Linting
* Testing
* Import/Concatanate Javascript
* Prepare images


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Automate all the things


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Many build toolkits
* Gulp
* Grunt
* Webpack


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Gulp
* Easy
* Clean
* Well Supported
* Uses node


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### A caveat on Gulp
* Gulp 3 does not work with Node 10
* Gulp 4 is not fully released
* We will use Gulp 4 with the @latest tag
	* This is not best practice with most packages
* Most gulp documentation is for Gulp 3
	* This will be outdated in the next few months


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Node / NPM
* A javascript runtime environment
* Based on Chrome's V8 JS engine
* Runs on the server (or your computer)
* Includes Node Package Manager (NPM)


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Initialize a node project
```
npm init
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Install Gulp globally
```
npm install -g gulp-cli
npm install -g gulp@next
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
To run a globally installed gulp you can use the gulp command

```
gulp
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Install Gulp for your project
```
npm install 
npm install --save-dev gulp@next
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
If you install gulp for your project you will need to add the following to package.json
```
"scripts": {
	"build": "gulp"
}
```

To run use the npm run command

```
npm run build
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Gulpfile
* Uses gulpfile.js
* Written in javascript
* Uses streams to pass files between plugins
* Import plugins with require
* Install plugins with NPM
* Calls default task by default


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
```
var gulp = require('gulp');
var concat = require('gulp-concat');
var sourcemaps = require('gulp-sourcemaps');

gulp.task('js', function(){
  return gulp.src('src/js/*.js')
    .pipe(sourcemaps.init())
    .pipe(concat('app.min.js'))
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('build/js'))
});

gulp.task('default', gulp.series('js'));
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Activity
* Use the gulpfile from the previous slide
* Install gulp and plugins using npm`
* Create the directory src/js
* Add some javascript from a previous week
* Use gulp to build the javascript



<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
## WebRTC
Web Real Time Communication


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
* API to capture media
* Can stream media
* Only available over https (or localhost)


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### getUserMedia
* Gets a media connection

```
navigator.mediaDevices.getUserMedia(options)
  .then(successFunction)
	.catch(errorFunction);
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Options
Options specifies what media you want

```
{
	video: true
}
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Success Function
Handle the media stream
```
function successFunction(mediaStream) {

	// Video element to show stream
	var videoElement = document.querySelector('video');

	// Attach media stream to the video element
	videoElement.srcObject = mediaStream;

}
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Your turn
* Open the pen [https://codepen.io/elvey/pen/RBOjBX](https://codepen.io/elvey/pen/RBOjBX)
* Write code to show video in the video player


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Activity: Work through the codelab
[WebRTC Codelab](https://codelabs.developers.google.com/codelabs/webrtc-web/#0)



<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
## IndexedDB


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
* Storage API for large amounts of data
* NoSQL Database
* Can storage complex data
* Can store files (as blogs)
* Is low level and complicated
	* Lots of libraries available to make it more user friendly


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Using IndexedDB
This one is pretty complex so take it slowly.

Ask questions if you get lost


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Check if it is supported
```
if ('indexedDB' in window) {
	// We have indexedDB
}
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Connect to a database
```
var connReq = indexedDB.open(
  dbName,
  dbVersion
);
```

[IDBFactory.open()](https://developer.mozilla.org/en-US/docs/Web/API/IDBFactory/open) returns an [IDBOpenRequest](https://developer.mozilla.org/en-US/docs/Web/API/IDBOpenDBRequest) immediately but asyncronouly connects using events


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
```
var conn = indexedDB.open('test-db', 3);

conn.addEventListener('success', function(evt) {
  var db = evt.result.target;
  console.log('connected event', evt);
});

conn.addEventListener('error', function(evt) {
  console.log(
    'error connecting',
    evt.target.error
  );
});

conn.addEventListener('upgradeneeded',function(evt) {
  console.log('upgrade needed', evt)
});
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Error events
* The error should not be too common
* Usually means
	* Your version is too low
	* User denied permission to the DB


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Creating or updating your database
* The onupgradeneeded events triggers an [IDBVersionChangeEvent](https://developer.mozilla.org/en-US/docs/Web/API/IDBVersionChangeEvent)
* This happens when you increase the version number (or first create the database)
* Create or delete any required object stores with [IDBDatabase.createObjectStore()](https://developer.mozilla.org/en-US/docs/Web/API/IDBDatabase/createObjectStore)
* If the event finishes without erorrs the onsuccess event is triggered


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Object stores
* IndexedDB uses object stores
	* A bit like tables
* Stores a variety of data
* If you use a keypass the object key is stored in the object
	* Note that if you use a keypass you can only store objects
* [MDN](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Using_IndexedDB#Structuring_the_database) has a great breakdown on the structure


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
```
conn.addEventListener('upgradeneeded',function(evt) {
  console.log('upgrade needed', evt)
  var db = evt.target.result;

  // Create an objectStore for this database
  var objectStore = db.createObjectStore(
    "users",
    { keyPath: "id" }
  );
  objectStore.createIndex(r
    'name',
    'name',
    {unique: false}
  );

  // Listen for the completed transaction
  objectStore
    .transaction
    .addEventListener('complete', function(evt) {
   console.log('Store created');
  });
});

```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Transactions
* All IndexedDB work is done in transactions
* Transactions have a limited life
	* If you don't use them they expire
	* They last as long as there are pending requests
	* You can extend a transaction by adding more requests
		* even in a completed request event callback
	* An inactive transactions triggers a TRANSACTION\_INACTIVE\_ERR

Note:
Transactions have a very specific lifetime. Their life is tied to the event loop. If the code returns to the javascript event loop without any pending transactions they will become inactive. You stop this by giving them work to do. 


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Create a transaction
* Call transaction on the database
* Pass the stores you want to access
* Specify the type of access you require

```
var transaction = db.transaction(
  ['users'],
  'readwrite'
);
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Add some data
```
var objectStore = transaction.objectStore('users');
var request = objectStore.add({
  id: 123,
  name: 'Elvey'
});
request.addEventListener('success', function(evt) {
  console.log(
    'Successfully added data',
    evt.target.result
  );
});
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Update data
* Add only creates a new record
* Throws an error if the key already exists
* To update use put

```
var transactiong = objectStore.put({id: 123, name: 'Elvey', role: 'teacher'});
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Delete a record
```
var transaction = objectStore.delete('123');
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Getting data
```
var transaction = objectStore.get('123');
transaction.addEventListener('success', function(evt) {
  var data = evt.target.result;
  console.log('Loaded', data);
});
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
But what if you don't know the ID?


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### Cursors
```
  var store = db.transaction(['users']).objectStore('users');
  var cursor = store.openCursor();
  cursor.addEventListener('success', function(evt) {
    var thisCursor = evt.target.result;
    if(thisCursor) {
      console.log('key', thisCursor.key);
      console.log('value', thisCursor.value);
      thisCursor.continue();
    } else {
      console.log('No more items');
    }
  });
}
```


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
#### getAll keys
* IndexedDB 2.0 includes a getAll function to get a list of all keys from the store
* However the support [isn't quite as strong yet](https://caniuse.com/#feat=indexeddb2)


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
### Your turn
* Open the pen [https://codepen.io/elvey/pen/GBbKJW](https://codepen.io/elvey/pen/GBbKJW)
* Connect to a database
* Create a new store to store users ID and their name in upgradedneeded
* Log an error to the console if there is a problem connecting
* If connection is successful
	* Add data from inputs when saveBtn is clicked
	* Load data from the store into the table
	* Update the list when you save data


<!-- .slide: data-background-image="../images/bg-smartphone.jpg" -->
* Is it slow to update the list when you save?
	* Why? How can you speed it up?
* Can you add a delete button to the table?