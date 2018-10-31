## A Test Driven Development Approach
I **really** struggled with testing and test driven development while I was learning Ruby. While working through Gordon's course he does a really great job of walking his students through the test driven process. His course uses Javascript but I believe the principles carry over language to language. Admittedly, I'm still really bad at test driven development but at least I now have a framework and a methodology I can now use to improve.

In his course we followed test driven development to write our own versions of Javascript's native array methods such as: forEach, filter, map, and reduce. Before moving forward in the course Gordon encourages his students to try and rewrite the rest of Javascript's native array methods using test driven development. I took a stab at writing Array.find on my own. Here's the methodology I followed.

### The Testing Framework

We used a modified version of [tinytest.js][tinytest.js] which we called `simpletest.js`. It has a couple modifications to display passing and failing messages in a slightly cleaner manner but tinytest.js works well all the same.

### Getting Setup

To begin I setup a base file as a sort of template to work from based on some of the things I'd learned in the course. Most of what I'd included in the below template is taken directly from the [MDN Docs on Array.find][MDN Docs on Array.find]. I included commented sections where I'd go on to include things such as:

- link to the native docs for the Javascript method - [MDN Docs on Array.find][MDN Docs on Array.find]
- link to the specs - [ECMA specs][ECMA specs]
- description of what the method is to actually do
- basic steps I should be taking for following test driven development
- the function signature
- list of the callback parameters
- the return value to be expected
- a list of requirements for Array.find from reading the docs
- a place where the actual function would be written
- a place where the tests would be written

Below is an example of the file I'd setup to begin my test driven development workflow.

{% highlight javascript %}
  <script src="simpletest.js"></script>
  <script>

    // Docs:
      // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find

    // Specs:
      // https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.find

    // Description:
      // Returns the found value in the array,
      // if an element in the array satisfies the provided testing function or undefined if not found.


    // The Steps
      // 1 write requirements based on docs
      // 2 write description for test
      // 3 write test to fail
      // 4 write code to make the test pass
      // 5 rinse and repeat

    // Function Signature:
      // find(array, callback[, opitonalThis])


    // Callback Parameters:
      // currentValue
      // currentIndex
      // array


    // Return Value:
      // The value of the first element in the array that satisfies the provided testing function;
      // otherwise, undefined is returned.


    // Requirements:
      // It should run the callback function array.length number of times.
      // It should pass currentValue as the first argument to the callback.
      // It should pass currentIndex as the second argument to the callback.
      // It should pass the array as the third argument to the callback.
      // It should accept an optionalThis.
      // It should not skip holes.
      // If the currentValue passed to the callback returns true, then the function should return that element.
      // If none of the elements passed as currentValue evaluate to true, then the function should return undefined.
      // It should not mutate the original array.


    // This is where the function will go.

      find(){}

    // This is where the tests will go.

      tests({

      });



  </script>

{% endhighlight %}


### Test Descriptions

The next step is to write the descriptions for the tests. You'll note each test is setup to fail from the start. I've listed these below:

{% highlight javascript %}
  tests({

  '1. It should run the callback array.length number of times.': function() {
    fail();
  },
  '2. It should pass currentValue as the first argument to the callback.': function() {
    fail();
  },
  '3. It should pass currentIndex as the second argument to the callback.': function() {
    fail();
  },
  '4. It should pass the array as the third argument to the callback.': function() {
    fail();
  },
  '5. It should accept an optionalThis.': function() {
    fail();
  },
  '6. It should not skip holes.': function() {
    fail();
  },
  '7. If the currentValue passed to the callback returns true, then the function should return that element.': function() {
    fail();
  },
  '8. If none of the elements passed as currentValue evaluate to true, then the function should return undefined.': function() {
    fail();
  },
  '9. It should not mutate the original array.': function() {
    fail();
  }
  });
{% endhighlight %}

### All Red (Failing)

Now that we have written descriptions for each of the tests and forced them to fail we should see all red, failing tests when we open the file in the console. Our goal is to eventually have all green, passing tests.

![All Failing Tests](/images/all-failing-tests.png)

### How to Write Failing Tests

Our next step is to write the actual tests for each requirement.

#### Test One

Our first requirement is **'It should run the callback array.length number of times.'**. Let's write a test for this.

1. We're defining the variable `numberOfTimesCallbackHasRun` to keep track of the number of times the callback has run.
2. We then define an array with a couple elements.
3. Next we call the find method and pass it the array and a callback function.
4. The callback function should run once for each element in the array so we increment `numberOfTimesCallbackHasRun` each time.
5. Lastly, we make an equality assertion. This is to say that we expect the value saved to `numberOfTimesCallbackHasRun` should be equal to the length of the array.

{% highlight javascript %}
  tests({

  '1. It should run the callback array.length number of times.': function() {
    var numberOfTimesCallbackHasRun = 0;
    var array = [1,2];
    find(array, function() {
      numberOfTimesCallbackHasRun++;
    });
    eq(numberOfTimesCallbackHasRun, array.length)
  }

  });
{% endhighlight %}

When we run the tests we get our first failure:

`simpletest.js:67 Error: assertEquals() "0" != "2"`

##### Make it Pass

This tells us the callback function did not run array.length number of times. Let's write some code for our function to make this test pass.

1. We define the function and have it accept an array and a callback.
2. We'll write a for loop to run `array.length` number of times.
3. We run the callback function inside the for loop which will run `array.length` number of times according to the for loop we created.

{% highlight javascript %}
  function find(array, callback) {
    for(var i = 0; i < array.length; i++){
      callback();
    }
  }
{% endhighlight %}

We run the test again and it passes.

![One Passing Test](/images/one-passing-test.png)

#### Test Two

Our second requirement is **'It should pass currentValue as the first argument to the callback.'**. Let's write a test for this.

1. We pass the find method an array.
2. We pass it a callback function with a `currentValue` parameter.
3. We make an equality assertion that `currentValue` should equal 1.

{% highlight javascript %}
  tests({

  '2. It should pass currentValue as the first argument to the callback.': function() {
    find([1], function(currentValue){
      eq(currentValue, 1);
    })
  }

  });
{% endhighlight %}

When we run the tests we get our second failure:

`Error: assertEquals() "undefined" != "1"`

##### Make it Pass

This tells us that `currentValue` is undefined. The only thing we need to do is pass `array[i]` to the callback.

{% highlight javascript %}
  function find(array, callback) {
    for(var i = 0; i < array.length; i++){
      callback(array[i]);
    }
  }
{% endhighlight %}

We run the test again and this time it passes.

![Two Passing Tests](/images/two-passing-tests.png)

#### Test Three

Our third requirement is **'It should pass currentIndex as the second argument to the callback.'**. Let's write a test for this.

1. We write the callback function to accept `currentIndex` as the second argument.
2. We make another equality assertion that the `currentIndex` should be equal to 0.

{% highlight javascript %}
  tests({

  '3. It should pass currentIndex as the second argument to the callback.': function() {
    find([1], function(currentValue, currentIndex){
      eq(currentIndex, 0);
    })
  }

  });
{% endhighlight %}

When we run the tests we get our third failure:

`Error: assertEquals() "undefined" != "0"`

##### Make it Pass

This tells us that `currentIndex` is undefined. The only thing we need to do is pass `i` to the callback.

{% highlight javascript %}
  function find(array, callback) {
    for(var i = 0; i < array.length; i++){
      callback(array[i], i);
    }
  }
{% endhighlight %}

We run the test again and this time it passes.

![Three Passing Tests](/images/three-passing-tests.png)

#### Test Four

Our fourth requirement is **'It should pass the array as the third argument to the callback.'**. Let's write a test for this.

1. We create a variable `testArray` and set it equal to an array of elements.
2. We write the callback function to accept `originalArray` as the third argument.
3. We make another equality assertion that `originalArray` should be equal to `testArray`.

{% highlight javascript %}
  tests({

  '4. It should pass the array as the third argument to the callback.': function() {
    var testArray = [1,2,3];
    find(testArray, function(currentValue, currentIndex, originalArray){
      eq(originalArray, testArray);
    })
  }

  });
{% endhighlight %}

When we run the tests we get our fourth failure:

`Error: assertEquals() "undefined" != "1,2,3"`

##### Make it Pass

This tells us that `originalArray` is undefined. We can simply pass `array` to the callback.

{% highlight javascript %}
  function find(array, callback) {
    for(var i = 0; i < array.length; i++){
      callback(array[i], i, array);
    }
  }
{% endhighlight %}

We run the test again and this time it passes.

![Four Passing Tests](/images/four-passing-tests.png)

#### Test Five

Our fifth requirement is **'It should accept an optionalThis.'**. Let's write a test for this.

1. We provide an array.
2. We provide the callback function.
3. We provide an optional this argument after the callback.
4. Inside the callback function we write another equality assertion. We say that `this.thisArg` should be equal to the value of the `thisArg`.

{% highlight javascript %}
  tests({

  '5. It should accept an optionalThis.': function() {
    find([1], function(){
      eq(this.thisArg, 'Object to use as this when executing callback.');
    },{thisArg: 'Object to use as this when executing callback.'})
  }

  });
{% endhighlight %}

When we run the tests we get our fourth failure:

`Error: assertEquals() "undefined" != "Object to use as this when executing callback."`

##### Make it Pass

This tells us that `thisArg` is undefined. Here's what we do to make it pass.

1. We'll make `optionalThis` the third argument in the function.
2. We'll create a new variable `findCallback` and assign it to the callback.
3. Now we use an `if` statement to check if `optionalThis` exists and if it does we bind `callback` to it and then assign this value to `findCallback`.
4. In the `for` loop we need to update the variable `callback` to `findCallback`.

{% highlight javascript %}
  function find(array, callback, optionalThis) {
    var findCallback = callback;

    if (optionalThis) {
      findCallback = callback.bind(optionalThis);
    }

    for(var i = 0; i < array.length; i++){
      findCallback(array[i], i, array);
    }
  }
{% endhighlight %}

We run the test again and this time it passes.

![Five Passing Tests](/images/five-passing-tests.png)

#### Test Six

Our sixth requirement is **'It should not skip holes.'**. A hole would be a blank space in an array. For example in `var array = [1,,2]`, there is a hole at `array[1]`. Our find function should not skip over holes. Let's write a test for this.

1. We create a variable `numberOfTimesCallbackHasRun` to keep track of the number of times the callback has run.
2. We create an array with a hole (an empty value) and assign it to `array`.
3. We increment `numberOfTimesCallbackHasRun` inside the callback function.
4. We make another equality assertion that `numberOfTimesCallbackHasRun` is equal to `array.length`.

{% highlight javascript %}
  tests({

  '6. It should not skip holes.': function() {
    var numberOfTimesCallbackHasRun = 0;
    var array = [1,,2];
    find(array, function(currentValue){
      numberOfTimesCallbackHasRun++;
    });
    eq(numberOfTimesCallbackHasRun, array.length);
  }

  });
{% endhighlight %}

When we run the tests.. whoa, it passed. This one was a freebie.


##### Make it Pass

We got this passing test for free. Enjoy it.

![Six Passing Tests](/images/six-passing-tests.png)

#### Test Seven

Our seventh requirement is **'If the currentValue passed to the callback returns true, then the function should return that element.'**.

1. Once again we create a variable `numberOfTimesCallbackHasRun` to keep track of the number of times the callback has run.
2. We create an a variable `returnValueOfFind` to store the return value of the find function.
3. We call the find function and pass it an array and pass `currentValue` to the callback function.
4. Inside the callback function we increment `numberOfTimesCallbackHasRun`
5. We want to return the first element we find that is equal to 3. Again, this value will be saved to  `returnValueOfFind`.
6. We make an equality assertion that the `returnValueOfFind` will be 3.
7. We also make another equality assertion that `numberOfTimesCallbackHasRun` will be 3 because the function should stop after it has found its match which in this test will be after the third iteration.

{% highlight javascript %}
  tests({

    '7. If the currentValue passed to the callback returns true, then the function should return that element.': function() {
      var numberOfTimesCallbackHasRun = 0;
      var returnValueOfFind = find([1,2,3,4,5], function(currentValue){
        numberOfTimesCallbackHasRun++
        return currentValue === 3;
      })
      eq(returnValueOfFind, 3);
      eq(numberOfTimesCallbackHasRun, 3);
      }

  });
{% endhighlight %}

When we run the tests we get our seventh failure:

`Error: assertEquals() "undefined" != "3"`

##### Make it Pass

We see that `returnValueOfFind` is undefined. Here's what we do to make it pass.

1. We want to return the `currentValue` if the callback evaluates to true so let's wrap the callback in an `if` statement.
2. Then inside the callback function we add `return array[i]`.


{% highlight javascript %}

  function find(array, callback, optionalThis) {
    var findCallback = callback;

    if (optionalThis) {
      findCallback = callback.bind(optionalThis);
    }

    for(var i = 0; i < array.length; i++){
      if (findCallback(array[i], i, array)) {
        return array[i];
      }
    }
  }

{% endhighlight %}

We run the test again and this time it passes.

![Seven Passing Tests](/images/seven-passing-tests.png)


#### Test Eight

Our eigth requirement is **'If none of the elements passed as currentValue evaluate to true, then the function should return undefined.'**.

2. Again we create an a variable `returnValueOfFind` to store the return value of the find function.
3. We call the find function and pass it an array and pass `currentValue` to the callback function.
5. We want to return the first element we find that is equal to 4. Again, this value will be saved to  `returnValueOfFind`.
6. We make an equality assertion that the `returnValueOfFind` will be 4.

{% highlight javascript %}
  tests({

      '8. If none of the elements passed as currentValue evaluate to true, then the function should return undefined.': function() {
      var returnValueOfFind = find([1], function(currentValue){
        return currentValue === 4;
      })
      eq(returnValueOfFind, undefined);
      }

  });
{% endhighlight %}

When we run this test we see that it also passes. Another freebie.

`Error: assertEquals() "undefined" != "3"`

##### Make it Pass

We didn't have to write any additional code to make this pass. The reason is because the function didn't find a match, no value is saved to `returnValueOfFind` so therefore it will return `undefined`. This is exactly the outcome we want.

1. It's not required, but we could explicit and write `return undefined;` at the end of the function which which would be returned if no match is found. It's not required but you could do this to be more clear.


{% highlight javascript %}

  function find(array, callback, optionalThis) {
    var findCallback = callback;

    if (optionalThis) {
      findCallback = callback.bind(optionalThis);
    }

    for(var i = 0; i < array.length; i++){
      if (findCallback(array[i], i, array)) {
        return array[i];
      }
    }
    return undefined;
  }

{% endhighlight %}

We run the test again and it still passes.

![Eight Passing Tests](/images/eight-passing-tests.png)

#### Test Nine

Our ninth requirement is **'It should not mutate the original array.'**.

1. We'll create an array of numbers and assign it to variable `originalArray`.
2. We call the function passing it `originalArray` and then call the callback function passing it `currentValue`.
3. Within the callback function we want to return `currentValue` if it's equal to 3.
4. We're going to now make some assertions to test that `originalArray` is not mutated after the function runs.
5. We check that the length of `originalArray` is still 3.
6. We check that the value of of each of the indices are still the same.

{% highlight javascript %}
  tests({

      '9. It should not mutate the original array.': function() {
        var originalArray = [1,2,3];
        find(originalArray, function(currentValue){
          return currentValue === 3;
        });
        eq(originalArray.length, 3);
        eq(originalArray[0], 1);
        eq(originalArray[1], 2);
        eq(originalArray[2], 3);
      }

  });
{% endhighlight %}

When we run the tests once again we'll see this test passes as well.

##### Make it Pass

Another freebie. This does confirm that nothing we've written will cause the `originalArray` to be changed which is exactly what we want. Nice work.

![Nine Passing Tests](/images/nine-passing-tests.png)

### Our Find Function

Here's a look at the final version of our `find()` function:

{% highlight javascript %}

  function find(array, callback, optionalThis) {
    var findCallback = callback;

    if (optionalThis) {
      findCallback = callback.bind(optionalThis);
    }

    for(var i = 0; i < array.length; i++){
      if (findCallback(array[i], i, array)) {
        return array[i];
      }
    }
    return undefined;
  }

{% endhighlight %}

### Our Tests

And here's a look at all of our tests together.

{% highlight javascript %}

  tests({

    '1. It should run the callback array.length number of times.': function() {
      var numberOfTimesCallbackHasRun = 0;
      var array = [1,2];
      var length = array.length;
      find(array, function() {
        numberOfTimesCallbackHasRun++;
      });
      eq(numberOfTimesCallbackHasRun, length)
    },
    '2. It should pass currentValue as the first argument to the callback.': function() {
      find([1], function(currentValue){
        eq(currentValue, 1);
      })
    },
    '3. It should pass currentIndex as the second argument to the callback.': function() {
      find([1], function(currentValue, currentIndex){
        eq(currentIndex, 0);
      })
    },
    '4. It should pass the array as the third argument to the callback.': function() {
      var testArray = [1,2,3];
      find(testArray, function(currentValue, currentIndex, originalArray){
        eq(originalArray, testArray);
      })
    },
    '5. It should accept an optionalThis.': function() {
      find([1], function(){
        eq(this.thisArg, 'Object to use as this when executing callback.');
      },{thisArg: 'Object to use as this when executing callback.'})
    },
    '6. It should not skip holes.': function() {
      var numberOfTimesCallbackHasRun = 0;
      var array = [1,,2];
      find(array, function(currentValue){
        numberOfTimesCallbackHasRun++;
      });
      eq(numberOfTimesCallbackHasRun, array.length);
    },
    '7. If the currentValue passed to the callback returns true, then the function should return that element.': function() {
      var numberOfTimesCallbackHasRun = 0;
      var returnValueOfFind = find([1,2,3,4,5], function(currentValue){
        numberOfTimesCallbackHasRun++
        return currentValue === 3;
      })
      eq(returnValueOfFind, 3);
      eq(numberOfTimesCallbackHasRun, 3);
    },
    '8. If none of the elements passed as currentValue evaluate to true, then the function should return undefined.': function() {
      var returnValueOfFind = find([1], function(currentValue){
        return currentValue === 4;
      })
      eq(returnValueOfFind, undefined);
    },
    '9. It should not mutate the original array.': function() {
      var originalArray = [1,2,3];
      find(originalArray, function(currentValue){
        return currentValue === 3;
      })
      eq(originalArray.length, 3);
      eq(originalArray[0], 1);
      eq(originalArray[1], 2);
      eq(originalArray[2], 3);
    }
  });

{% endhighlight %}

### All Green (Passing)

We've come a long way so let's take this in one more time.

![Nine Passing Tests](/images/nine-passing-tests.png)

### Final Thoughts

If you've made it this far, congrats. This isn't an easy piece to read. As I said at the start, I'm pretty bad at testing but I'm using this methodology to get better.

### Next Steps

If you found this content helpful and you're trying to learn or improve your programming skills then I can't recommend enough that you consider working through [Watch & Code][Watch & Code].

Gordon, the creator of the course, offers a free course called Practical Javascript. Start there and if you feel like you are getting value from it then keep with it. It's been the best resource I've found for learning how to read code, write code, and think like a programmer.


<!-- Links -->
[Watch & Code]: https://watchandcode.com/
[tinytest.js]: https://github.com/joewalnes/jstinytest
[MDN Docs on Array.find]:https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find
[ECMA specs]: https://www.ecma-international.org/ecma-262/6.0/#sec-array.prototype.find
