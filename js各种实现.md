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

// 实现trim
String.prototype._trim = function () {
    return this.replace(/^\s+|\s+$/g, '')
}

// 实现柯里化 sum(3)(4)(5),,,(n)()
function sum(a) {
    return function (b) {
        if (b) {
            return sum(a + b)
        }
        return a
    }
}

// 实现reduce
Array.prototype._reduce = function (fn, a) {
    var acc = a
    for (var i of this) {
        acc = fn(acc, i)
    }
    return acc
}

// 实现斐波那契数列
const fib = n => n > 1 ? fib(n - 2) + fib(n - 1) : n

function fib2(n) {
    if (n < 1) { return 0; } else if (n == 1 || n == 2) { return 1; }
    let one = 0, two = 1, temporary = 1
    for (let i = 3; i <= n; i++) {
        temporary = one + two
        one  = two
        two = temporary
    }
    return temporary;
}

// 实现promise.all
function promise_all(arr) {
    return new Promise((resolve, reject) => {
        if (Array.isArray(arr)) {
            let count, res = []
            for (let i = 0; i < arr.length; i++) {
                Promise.resolve(p).then((r) => {
                    res[i] = r
                    count++
                    if (count === arr.length) {
                        resolve(res)
                    }
                }).catch((e) => {
                    reject(e)
                })
            }
        } else {
            reject(TypeError(''))
        }
    })
}


// 实现一个EventBus
class EventBus {
    constructor() {
        this.events = Object.create(null)
    }
    $on(name, fn) {
        if (!this.events[name]) {
            this.events[name] = []
        }
        this.events[name].push(fn)
    }
    $off(name, fn) {
        if (fn) {
            if (this.$events[name]) {
                this.$events[name] = this.$events[name].filter(fun => fun !== fn)
            }
        } else {
            delete this.$events[name]
        }
    }
    $emit(name, ...args) {
        if (name && this.$events[name]) {
            for (let fn of this.$events[name]) {
                fn.apply(this, args)
            }
        }
    }
    $once(name, fn) {
        let fun = (...args) => {
            fn.apply(this, args)
            this.$off(name, fn)
        }
        this.$on(name, fun)
    }
}

// 实现一个throttle
function throttle(fn, wait) {
    let timer, start = Date.now()
    return function () {
        let now = Date.now(), context = this, args = arguments
        if (now - start < wait) {
            if (!timer) {
                timer = setTimeout(() => {
                    fn.apply(context, args)
                    timer = null
                    start = Date.now()
                }, wait)
            }
        } else {
            clearTimeout(timer)
            fn.apply(context, args)
            timer = null
            start = now
        }
    }
}

// 实现一个debounce
function debounce(cb, delay){
    var timer;
    return function(){
        var args = Array.prototype.slice.call(arguments,1)
        if(timer){
            clearTimeout(timer)
        }
        timer = setTimeout(function(){
            cb.apply(this,args)
        },delay)
    }
}


// Promise.race
function race(arr){
    return new Promise(function(resolve, reject){
        if(Array.isArray(arr)){
            for(let item of arr){
                Promise.resolve(item).then(resolve).catch(reject)
            }
        }else{
            reject(TypeError(''))
        }
    })
}

// Promise.allSettled
function allSettled(arr){
    return new Promise((resolve, reject) =>{
        if(Array.isArray(arr)){
            var res = [], c = 0
            for(let i = 0; i < arr.length; i++){
                arr[i].then(r =>{
                    res[i] = {status: 'fulfiled',value:r}
                    dealResult()
                }).catch(e =>{
                    res[i] = {status: 'rejected',value:e}
                    dealResult()
                })
            }
            function dealResult(){
                if(++c === arr.length){
                    resolve(res)
                }
            }
        }else{
            reject(TypeError(''))
        }
    })
}

// 实现一个方法随机生成一个合法的css颜色值 如 '#c1c1c1' 或者 rgba()
function cMaker(t){
    if(t){
        var a = () => parseInt(Math.ceil(Math.random()*16)).toString(16)
        return `#${a()}${a()}${a()}${a()}${a()}${a()}`
    }else{
        var a = () => parseInt(Math.random() * 255)
        return `rgba(${a()},${a()},${a()},1)`
    }
}



// 实现一个 requestAnimationFrame
// function deal(){
//     requestAnimationFrame(deal)
// }
// requestAnimationFrame(deal)

var start = 0
function requestAnimationFrame(handler){
    var gap = 1000 / 60
    var now = Date.now()
    var delay = Math.max(0, gap - (now - start))

    var id = setTimeout(function(){
        handler(now + delay)
    }, delay)
    start = now + delay
    return id
}


```
