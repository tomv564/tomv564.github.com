---
layout: post
title: "Mastermind part 3: A web app"
date: 2014-01-26 17:23:40 +0100
comments: true
categories:
---

So you have tested server logic in Ruby, and a working front-end prototype. What is the easiest way to hook them up?

I had some experience with [Ruby on Rails](http://rubyonrails.org/) so I first generated a rails app to handle the backend. A large directory structure filled with boilerplate appeared, Turbolinks broke my event handlers, and I decided a microframework like [Sinatra](http://www.sinatrarb.com) was much better suited to the task.

### Install sinatra

Run this in your project directory

```
gem install sinatra
```

### Move your frontend

Sinatra expects all your HTML, CSS and javascript to live in a subfolder named *public*.

- public (index.html)
	- js (app.js, jquery.min.js, bootstrap.min.js)
	- css (app.css, bootstrap.css)
	- fonts (Bootstrap glyphicons)

### Create a controller.rb in the root to serve it all up


``` ruby controller.rb

require 'sinatra'
require 'json'


get '/' do
	redirect '/index.html'
end


```

And fire it up using

```
ruby controller.rb
```

Webrick should be fired up and serving index.html on http://localhost:4567/


### Add the New Game action

How do we keep the multiple games running on the server apart?

Add the *securerandom* gem, and add a read-only id property on our Game object:

``` ruby game.rb
require 'securerandom'

  class Game
    @answer = []
    attr_reader :id

    def initialize(answer)
      validate_code(answer)
      @answer = answer
      @id = SecureRandom.hex(4)
    end

```
We need to store this game object between requests, and we're not going to use a database for this. Instead, we'll set up a hash in Sinatra's configuration when the app loads:

``` ruby controller.rb
require_relative 'game'
require_relative 'generator'

configure do
  set :games, Hash.new
end
```

In your app.js, your reset function should request a new game:

``` javascript

$.post('/game', onGameCreated);

```

Your server creates, stores and returns the new game:

``` ruby controller.rb

post '/game' do
	# create a new game
	answer = Generator.generate
	game = Game.new answer

	# store the game to be looked up by id
	settings.games[game.id] = game

	# tell Sinatra we are speaking JSON
	content_type :json
	game.id.to_json
end

```

Back on the client, your onGameCreated function should extract the id and store it for future requests

``` javascript app.js
var gameId;

var onGameCreated = function(data) {
        gameId = data;
}
```

Restart your sinatra host, and you should see an AJAX request going out to your server as soon as the page hits your reset() method.

### Add the Guess action


Once the player has selected four colors I allow then to submit the guess. See here the use of gameId, and colors is passed as an array in the POST payload.

``` javascript app.js
$.post('/game/' + gameId + '/guesses', {'colors[]': colors}).done(onGuessAdded);
```

Your web handler should look something like this:

``` ruby controller.rb
post '/game/:id/guesses' do

        # find the game
        game = settings.games[params[:id]]

        # parse the parameters (convert to symbols)
        colors = params[:colors]
        guess = colors.map {|c| c.to_sym }

        # add a guess
        score = game.guess guess

        # return the score
        content_type :json
        score.to_json
end
```

Things I think are great about this code:

- You can pull params from the requested URL or the postdata
- The RESTful method signature is very descriptive: Post this guess to the guesses of the game identified by id.

Only the converting of strings to symbols is a bit unproductive.

### Deploy to a server

Because I don't want to set up my own server, and Platform-as-a-Service providers often have restricted free accounts, we will deploy the app to [Heroku](http://www.heroku.com/).

Their [Getting Started](https://devcenter.heroku.com/articles/quickstart) page walks you through signing up, installing the client tools and deployment. Once you're ready to deploy, check the rest of this blog post.

Using their [instructions for Rack apps](https://devcenter.heroku.com/articles/rack) we will set up the Gemfile and config.ru in the root:

``` ruby Gemfile
source 'https://rubygems.org'
gem "bundler"  , "~> 1.3.5"
gem "rspec"    , "~> 2.14.1"
gem "sinatra"
gem "securerandom"
```

``` ruby config.ru
require './controller'
run Sinatra::Application
```

You should have run heroku login at this point, so we are ready to create and deploy the app using a git push:

```
heroku create
git push heroku master
heroku open
```

You should be up and running! The free Heroku instances tend to go to sleep every once in awhile.
If the app is still running, the url is at: [Tom's MasterMind](http://tomv-mastermind.herokuapp.com)

