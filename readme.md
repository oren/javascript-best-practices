#JavaScript Best Practices

Douglas's good parts of ES6: proper tail calls, (no need for while loop), spread operator, module, let, destructuring, weakmaps.  
He stopped using the following: new, Object.create, this, null, falsiness, for loops.

http://youtu.be/PSGEjv3Tqo0

## TCO - Tail Call Optimization
less memory consumption when we return a function call (due to reuse of the stack frame)

  function() {
    var x = 1;
    return foo(x);
  }

## spread operator
example(...args) instead of example.apply(null, args)

in the past we had to use apply to split array into it's items:

  function example(a, b, c) {
    console.log(a, b, c); // 1, 2, 3
  }

  var args = [1, 2, 3];
  example.apply(null, args);

the above is not needed anymore.

  function example(a, b, c) {
    console.log(a, b, c); // 1, 2, 3
  }

  var args = [1, 2, 3];
  example(...args);

## module

http://jsmodules.io/

There are several reasons why they couldn't just use commonjs:

1. it *has* to be asynchornous.  
node's require is specifically designed to be synchronous

2. they are statically analyzable.  
you can tell via static analysis what a piece of js depends on and exports.  
you can't know that in node, since you can do `require(foo + '/bar')`.  
you can know what a module depends on by just reading the code and not needing to run the code.  
And why do we care? The browser can tell what dependencies it needs to download and load.

## let

like var but limited scope to a block, statement or expression

  if (name) {
    let lastName = 'doe';
    name = name + ' ' + lastName;
  }

## destructuring

### syntax

  [a, b] = [1, 2]
  {a, b} = {a:1, b:2}

  var [x, y, z] = [1, 2, 3]; // the same as var x = 1, y = 2, z = 3;
  function example() { return [1, 2, 3]; }   var [a, b, c] = example();   console.log(a, b, c); // 1, 2, 3

## weakmaps

Maps that you can't iterate over, which means implementations can (and are expected to) remove keys that aren't referenced anywhere else (and therefore GCable).  
Usecase: when you want to map potential object references to values, without needing to iterate over them

  var wm = new WeakMap();
  var o1 = {};
  wm.set(o1, 'booo');
  wm.get(o1);     // 'booo'
  wm.has(o1);     // true
  wm.delete(o1);
  wm.has(o1);     // false
