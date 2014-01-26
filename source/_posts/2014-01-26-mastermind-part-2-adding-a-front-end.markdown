---
layout: post
title: "MasterMind Part 2: Adding a front end"
date: 2014-01-26 16:31:49 +0100
comments: true
categories: 
---

The [previous post](/blog/2013/12/14/ruby-rspec-and-git-a-guessing-game/) was an exercise in building well-tested logic. To turn this in to a workable application, two things need to happen:

1. A front-end needs to be built
2. The back-end needs to be hosted in a web app

In fact, you could build the whole thing as a front-end app, but then you would miss out on building a minimalistic Ruby web app. 

So, let's mock up a front end in [Balsamiq](http://www.balsamiq.com/).

![mockup](https://raw2.github.com/tomv564/mastermind/master/mockup.png)

### HTML template

Because I don't want to work hard to get pretty fonts and buttons, [download Bootstrap](http://getbootstrap.com) and use their basic html template.

The board is made up of nested lists, the markup looks roughly like this:

``` html 
<div id="board">
  
    <ul id="answer" class="code list-unstyled">
      <li>?</li>
      <li>?</li>
      <li>?</li>
      <li>?</li>
    </ul>

    <ul id="guesses" class="list-unstyled">
      <li>
        <ul class="code list-unstyled">
          <li></li>
          <li></li>
          <li></li>
          <li></li>
        </ul>
        <ul class="score list-unstyled">
          <li class="black">&nbsp;</li>
          <li class="white">&nbsp;</li>
          <li class="white">&nbsp;</li>
          
        </ul>
      </li>
    </ul>

  </div>

  <div id="addguess">
    <ul class="code list-unstyled">
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
    <button id="addguessbutton" type="button" class="btn btn-xs btn-primary"><span class="glyphicon glyphicon-ok"></span></button>
  </div>

  <ul id="turns" class="list-unstyled">
    <li>
      <ul class="code list-unstyled">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </li>
     <li>
      <ul class="code list-unstyled">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </li>
  </ul>

  </div>
```

The following CSS turns those list items into circles:

``` css
ul.code li {

        float: left;
        border: 2px solid #999;
        margin-left: 5px;
        width: 30px;
        height: 28px;
        border-radius: 20px;

}
```

### Tying it together: the javascript

To encapsulate the logic for the whole page, I started with this basic pattern:

``` javascript

Mastermind = new function() {

        // private variables
        var selectedPeg;
        var lastGuess;
        var gameId;

        // private functions 

   		var addGuess = function () {

   		}

        var reset = function() {

        }

        // first load: attach handlers and reset game.

        $('#newgamebutton').click(reset);
        $('#addguessbutton').click(addGuess);

        reset();

        return {
                // public functions and variables

        };

}();


```

It is based on [Crockford's private members in Javascript](http://javascript.crockford.com/private.html) and helps keep your objects tight and neat. 

Filling in the details is a mix of DOM manipulation using JQuery and lots of trial-and-error. See the [complete javascript](https://github.com/tomv564/mastermind/blob/d01d3bf8de635b97462c6aa7060f7b679a6f37ef/js/app.js) for some ideas.

The front end implementation is deliberately dumb - it should only allow picking colors and submitting guesses. The game logic will be hooked up in the next step, when we convert our Ruby class into a web app.

The implementation at this point can be seen at [this commit in GitHub](https://github.com/tomv564/mastermind/tree/d01d3bf8de635b97462c6aa7060f7b679a6f37ef)
