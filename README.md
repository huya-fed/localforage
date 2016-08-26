# localForage

---

localStorage 能够让你实现基本的数据存储，但它的速度慢，而且不能处理二进制数据。IndexedDB 和 WebSQL 是异步的，速度快，支持大数据集，但他们的API 使用起来有点复杂。不仅如此，IndexedDB 和 WebSQL 没有被所有的主流的浏览器厂商支持，这种情况最近也不太可能改变。

https://localforage.github.io/localForage/

http://www.cnblogs.com/dolphinX/p/3415761.html

---

## 使用说明

Mozilla 开发了一个叫 localForage 的库 ，使得离线数据存储在任何浏览器都是一项容易的任务。

localForage 是一个使用非常简单的 JavaScript 库的，提供了 get，set，remove，clear 和 length 等等 API，还具有以下特点：

- 支持回调的异步 API；
- 支持 IndexedDB，WebSQL 和 localStorage 三种存储模式（自动为你加载最佳的驱动程序）；
- 支持 BLOB 和任意类型的数据，让您可以存储图片，文件等等。
- 支持 ES6 Promises；



##API使用


### setItem

	// Set a value with localStorage:
	localStorage.setItem('key', JSON.stringify('value'));
	doSomethingElse();
	
	// The same code with localForage:
	localforage.setItem('key', 'value').then(doSomethingElse);
	
	// localForage also support callbacks:
	localforage.setItem('key', 'value', doSomethingElse);


### getItem


	localforage.getItem('somekey').then(function(value) {
	    // This code runs once the value has been loaded
	    // from the offline store.
	    console.log(value);
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});
	
	// Callback version:
	localforage.getItem('somekey', function(err, value) {
	    // Run this code once the value has been
	    // loaded from the offline store.
	    console.log(value);
	});


### 存储多种类型

	localforage.setItem('somekey', 'some value').then(function (value) {
	    // Do other things once the value has been saved.
	    console.log(value);
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});
	
	// Unlike localStorage, you can store non-strings.
	localforage.setItem('my array', [1, 2, 'three']).then(function(value) {
	    // This will output `1`.
	    console.log(value[0]);
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});


	// You can even store binary data from an AJAX request.
	req = new XMLHttpRequest();
	req.open('GET', '/photo.jpg', true);
	req.responseType = 'arraybuffer';
	
	req.addEventListener('readystatechange', function() {
	    if (req.readyState === 4) { // readyState DONE
	        localforage.setItem('photo', req.response).then(function(image) {
	            // This will be a valid blob URI for an <img> tag.
	            var blob = new Blob([image]);
	            var imageURI = window.URL.createObjectURL(blob);
	        }).catch(function(err) {
	          // This code runs if there were any errors
	          console.log(err);
	        });
	    }
	});

### removeItem

	localforage.removeItem('somekey').then(function() {
	    // Run this code once the key has been removed.
	    console.log('Key is cleared!');
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});


### clear

	localforage.clear().then(function() {
	    // Run this code once the database has been entirely deleted.
	    console.log('Database is now empty.');
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});


### length，有多少个数据库


	localforage.length().then(function(numberOfKeys) {
	    // Outputs the length of the database.
	    console.log(numberOfKeys);
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});


### key


	localforage.key(2).then(function(keyName) {
	    // Name of the key.
	    console.log(keyName);
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});

### keys

	localforage.keys().then(function(keys) {
	    // An array of all the key names.
	    console.log(keys);
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});


### iterate 迭代

	// The same code, but using ES6 Promises.
	localforage.iterate(function(value, key, iterationNumber) {
	    // Resulting key/value pair -- this callback
	    // will be executed for every item in the
	    // database.
	    console.log([key, value]);
	}).then(function() {
	    console.log('Iteration has completed');
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});


	// The same code, but using ES6 Promises.
	localforage.iterate(function(value, key, iterationNumber) {
	    if (iterationNumber < 3) {
	        console.log([key, value]);
	    } else {
	        return [key, value];
	    }
	}).then(function(result) {
	    console.log('Iteration has completed, last iterated pair:');
	    console.log(result);
	}).catch(function(err) {
	    // This code runs if there were any errors
	    console.log(err);
	});

### SETTINGS API

设置用那个类型存储

- localforage.INDEXEDDB
- localforage.WEBSQL
- localforage.LOCALSTORAGE

		// Force localStorage to be the backend driver.
		localforage.setDriver(localforage.LOCALSTORAGE);
		
		// Supply a list of drivers, in order of preference.
		localforage.setDriver([localforage.WEBSQL, localforage.INDEXEDDB]);


### CONFIG

	// This will rename the database from "localforage"
	// to "Hipster PDA App".
	localforage.config({
	    name: 'Hipster PDA App'
	});
	
	// This will force localStorage as the storage
	// driver even if another is available. You can
	// use this instead of `setDriver()`.
	localforage.config({
	    driver: localforage.LOCALSTORAGE,
	    name: 'I-heart-localStorage'
	});
	
	// This will use a different driver order.
	localforage.config({
	    driver: [localforage.WEBSQL,
	             localforage.INDEXEDDB,
	             localforage.LOCALSTORAGE],
	    name: 'WebSQL-Rox'
	});


### DRIVER

	localforage.driver();
	// "asyncStorage"


### supports
	localforage.supports(localforage.INDEXEDDB);
	// true


### CREATEINSTANCE

	var store = localforage.createInstance({
	  name: "nameHere"
	});
	
	var otherStore = localforage.createInstance({
	  name: "otherName"
	});
	
	// Setting the key on one of these doesn't affect the other.
	store.setItem("key", "value");
	otherStore.setItem("key", "value2");