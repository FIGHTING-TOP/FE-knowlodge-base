# js的常见方法实现

```javascript
// 实现一个instanceof
function _instanceof(obj, fn) {
    var proto = obj.__proto__
    while (proto) {
        if (ptoto === fn.prototype) {
            return true
        } else {
            proto = proto.__proto__
        }
        return false
    }
}

// 实现一个new
function _new(fn) {
    var args = Array.prototype.slice.call(arguments, 1)
    var obj = new Object()
    var res = fn.apply(obj, args)
    if (res instanceof Object) {
        return res
    }
    return obj
}


// 实现一个flat var res = [...flat([1,2,[1,2,[1,2]]], Infinity)]
function* _flat(arr, depth){
    if(depth === undefined)depth = 1
    for(let item of arr){
        if(Array.isArray(item) && depth > 0){
            yield * flat(item, depth - 1)
        }else{
            yield item
        }
    }
}



// 实现一个sleep
//Promise
const sleep = time => {
  return new Promise(resolve => setTimeout(resolve,time))
}
sleep(1000).then(()=>{
  console.log(1)
})

//Generator
function* sleepGenerator(time) {
  yield new Promise(function(resolve,reject){
    setTimeout(resolve,time);
  })
}
sleepGenerator(1000).next().value.then(()=>{console.log(1)})

//async
function sleep(time) {
  return new Promise(resolve => setTimeout(resolve,time))
}
async function output() {
  let out = await sleep(1000);
  console.log(1);
  return out;
}
output();

//ES5
function sleep(callback,time) {
  if(typeof callback === 'function')
    setTimeout(callback,time)
}

function output(){
  console.log(1);
}
sleep(output,1000);




```
