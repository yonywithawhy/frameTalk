frameTalk
=========
frameTalk.js provides a simple way to use window.postMessage for iFrame communication.
After loading, frameTalk will add event listener to the window it is loaded.
You have to load frameTalk.js script in all windows or iFrames you want to use it.
frameTalk.js works without any dependencies, but if jQuery is found, you can use handshake method asynchronously 
to run code after the communication between iFrames is ensured.
frameTalk.js needs window.JSON to run, and it will log the issue on the console if JSON is not found.

<h2>Ultra fast usage reference: </h2>
 
```javascript
// ensure connection from "#_Iframe" iframe element and ask for some data from parent
$(document).ready(
    frameTalk.handshake(window.top, "_Iframe").then(
        function(connectionOK) {            
			if (connectionOK === true) {
				// run the window.top.fetchDepartmentData function with some params
				frameTalk.sendMessage(window.top, "fetchDepartmentData", ["sales", "John Doe"]);
			} else {
				// handshake failed
			}            
        },
        function(error) { 
			// handshake failed, possibly no frameTalk to listen to handshake there
		}
    )
);

// ask data with a promise from parent to this iFrame. Id of this iFrame must be declared
frameTalk.sendPromise(window.top, "_Iframe", "spyreqs.rest.getWebLists", []).then(say,say);
```

frameTalk object is the only public object that frameTalk.js exposes. It has following public methods and properties:

<h3>frameTalk.debuging</h3>
**description:** Sets or gets the debuging setting. Default is false. Set to true to console.log anything that frameTalk does in the specific window or iFrame.

```javascript
frameTalk.debuging = true;
```

<h3>frameTalk.getId</h3>
**description:** Returns the random 4 digit id number created from frameTalk auto init.

```javascript
var myUniqueId = frameTalk.getId();
```

<h3>frameTalk.failTimeLimit</h3>
**description:** Default value is 5000 ms. It is the time limit until it stops trying for a handshake. 
If loading iFrames takes more than 5 seconds, you can change this limit.

```javascript
frameTalk.failTimeLimit = 15000;
```

<h3>frameTalk.init</h3>
**description:** This method adds the proper event listener to the window, depending on browser standards. It is called once the js is loaded. If called again, it makes sure it will not add another listener. Once it is called, it puts a property to the window: 

**returns:** The method call returns true / false 

<h3>frameTalk.sendMessage (where, theFunction, theParams)</h3>
**description:** This method uses window.postMessage to send the message. 

```javascript
frameTalk.sendMessage( window.top, "doRunFn", [1,2,3,'four'] );
frameTalk.sendMessage( iframeDOMobject, "doRunFn", 154 );
```

**parameters:** 
<ul>
	<li>where: (type: DOM object) : the iFrame or window to talk to</li>
	<li>theFunction: (type: string) : the listener's function's name you want to run </li>
	<li>theParams: (type: array or string/number for single values) : the params of the listener's function's. </li>
</ul>

**returns:** Nothing. Will log on the console any error

Note: there is a fourth parameter, the promiseInd which is the handshake promise Index and it is not to be used. That is way any parameters for the function to be called should be in an array object.

<h3>frameTalk.sendPromise (where, fromId, theFunction, theParams)</h3>
**description:** This method uses window.postMessage to ask the promise. 

```javascript
frameTalk.sendPromise(window.top, "_Iframe", "spyreqs.rest.getWebLists", []).then(say,say);
```

**parameters:** 
<ul>
	<li>where: (type: DOM object) : the iFrame or window to talk to</li>
	<li>fromId: (type: string) : the id of the iFrame that promise was asked from</li>
	<li>theFunction: (type: string) : the listener's function's name you want to run </li>
	<li>theParams: (type: array or string/number for single values) : the params of the listener's function's. </li>
</ul>

**returns:** a promise (jQuery promise)

<h3>frameTalk.handshake (toWindow, fromId)</h3>
**description:** This method tries to ensure communication between this window and the destination window.

```javascript
  // use with promise: (jQuery is needed)
  var dest = window.document.getElementById('child1');
  // would also work: var dest = window.document.getElementById('child1').contentWindow;
  // or : var dest = window.top;
	frameTalk.handshake(dest).then(
		function(result) { alert("success:" + result); },
		function(error) { alert('handshake failed. ' +  error ); }
	); 
```

```javascript
  // use without promise. Will log the result on console
  var dest = window.document.getElementById('child1');
	frameTalk.handshake(dest);
```

**parameters:** 
<ul>
	<li>toWindow: (type: DOM object) : the iFrame or window to handshake with</li>
	<li>fromId: (type: string) : the id of the iFrame that handshake was started from</li>
</ul>

**returns:** A promise or nothing, depending on syntax




