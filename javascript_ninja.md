# Ninia Tips
---

## 1. sorting array

```javascript
values.sort(function(value1,value2){ return value2 - value1; });
```

## 2. page startup
```javascript
function startup(){
/* do something wonderful */
}
window.onload = startup;
```

## 3. about thread
It is important to note that the browser event loop is single-threaded. Every
event that is placed into the event queue is handled in the order that it is
placed onto the queue. 

## 4. function name
```javascript
function func2() {console.log("I am another function");}
console.log(typeof func2);
console.log(func2.name);
```

## 5. recursion

This is the worst approach.
```javascript
var ninja = {
  chirp: function(n) {
    return n > 1 ? ninja.chirp(n - 1) + "-chirp" : "chirp";
  }
};
```

This is the better approach.
```javascript
var ninja = {
  chirp: function(n) {
    return n > 1 ? this.chirp(n - 1) + "-chirp" : "chirp";
  }
};
```
When using the inline function, the named inline function is only visible
within the scope of its own body.
```javascript
var ninja = {
  chirp: function signal(n) {
    return n > 1 ? signal(n - 1) + "-chirp" : "chirp";
  }
};
```

```javascript
var ninja = {
  chirp: function(n) { //#1
    return n > 1 ? arguments.callee(n - 1) + "-chirp" : "chirp";
  }
};
```

## 6. the !! operator 
```javascript
return !!"hello"; // same as return true;
return !!0; // same as return false;
```

## 7. funcition memorization

```javascript
function isPrime(value) {
  // complete this ...
  return false;
}
```
## 8. memorizing dom elements
```javascript
function getElements(name) {
  if (!getElements.cache) getElements.cache = {};
  return getElements.cache[name] = getElements.cache[name] || document.getElementsByTagName(name);
}
```
This technique results to 5x performance increase.

## 9. a bit about `arguments` in function (not an array object by default)

```javascript
function multiMax(multi) {
  return multi * Math.max.apply(Math,
  Array.prototype.slice.call(arguments, 1)); #1
}
assert(multiMax(3, 1, 2, 3) == 9, "3*3=9 (First arg, by largest.)");
```

## 10. all functions have `length` property
```javascript
function makeNinja(name){}
function makeSamurai(name, rank){}
assert(makeNinja.length == 1, "Only expecting a single argument");
assert(makeSamurai.length == 2, "Two arguments expected");
```

## 11. a better way to overload functions with different number of parameters
```javascript
function addMethod(object, name, fn) {
  var old = object[name]; #1
  object[name] = function(){ #2
    if (fn.length == arguments.length) {
      return fn.apply(this, arguments);
    }
    else if (typeof old == 'function') {
      return old.apply(this, arguments);
    }
  };
}
```
and then, we can do
```javascript
var ninja = {};
addMethod(ninja,'whatever',function(){ /* do something */ });
addMethod(ninja,'whatever',function(a){ /* do something else */ });
addMethod(ninja,'whatever',function(a,b){ /* yet something else */ });
```
