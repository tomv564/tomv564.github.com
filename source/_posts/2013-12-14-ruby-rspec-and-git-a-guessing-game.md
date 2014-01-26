---
layout: post
title: "Ruby, rspec and git, a guessing game"
description: ""
category: ""
tags: []
---

Let's make a game. Better yet, let's copy one. Let's implement [MasterMind](http://en.wikipedia.org/wiki/Mastermind_(board_game) in Ruby.

The living project for this article is on [GitHub](https://github.com/tomv564/mastermind)

### Prep

First, set up a ruby on your machine using [this guide](http://railsapps.github.io/installrubyonrails-mac.html) about RVM.

Create an empty class, we'll call it game.rb. 
For Ruby, RSpec and TestUnit are popular. We'll use RSpec for now:

First, install the rspec gem:

{% highlight bash %}
gem install rspec
{% endhighlight %}

Rspec seems happiest when you put all the tests in a spec folder, so let's go ahead create our test file in **spec/game_spec.rb**

### Write some tests

Here is our first test scenario:

{% highlight ruby %}

require_relative '../game'

describe Game do 
	it "requires an answer" do
		expect{Game.new}.to raise_error(ArgumentError)
	end		
end

{% endhighlight %}

This file tells RSpec

1. Load a Ruby object called *game* from the parent directory
2. We are going to test an object called *Game*
3. This test case is called *requires an answer*
4. **expect** warns RSpec that *Game.new* will throw an Exception
5. We expect this exception to be an *ArgumentError*.

### Run them

As long as all your spec files live in a spec directory, you can simply type the following your project directory:

{% highlight bash %}
rspec
{% endhighlight %}

You should see 1 failed test.

### Write some code

You're on your own here. Read up on the Mastermind rules on Wikipedia, then code:

1. Write a failing test
2. Write the minimal amount of code to fix the test
3. Goto 1
4. Refactor to clean up as necessary

## So, you're done

You have a working game class. We want to take a moment to save our progress.

### Create a git repository

First, convert your project to a local git repository

{% highlight bash %}
git add .
git commmit -m 'initial commit'
{% endhighlight %}

### Push it to github

1. Create the repository on GitHub [Instructions here](https://help.github.com/articles/create-a-repo)
2. Add the github repository as the origin remote ([See this help page](https://help.github.com/articles/adding-a-remote)

{% highlight bash %}
git remote add origin https://github.com/user/repo.git # replace user and repo with your own repository details
{% endhighlight %}

3. Push-pull to sync

{% highlight bash %}
git pull origin master
git push origin master
{% endhighlight %}

## Automating the tests

Your work is now saved outside your machine, you can give others access to your repository and collaborate. But with everyone stirring the pot at the same time, how do you know everything's still working? The answer is continuous integration: we're going to run a service to run the tests whenever someone checks in code. 

### Set up a rake task

We need a way to tell a 3rd party how to run the tests on our project. In Ruby, this is called the **Rakefile**:

{% highlight ruby %}

require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec)

task :default => :spec

{% endhighlight %}

This does the following:

1. Tell Rake we want to use rspec
2. Creates an RSpec task, naming it :spec
3. Tells Rake that the default task is our :spec task.

Create and save this file in the project's directory, and run the following:

{% highlight bash %}
rake
{% endhighlight %}

You can see that rake starts up RSpec, the output should be the same as running rspec.

### Set up a gemfile for Travis

The service we're going to use, [Travis](https://travis-ci.org), now knows how it can run our tests. But it has no idea what our ruby environment looks like. We need to create these files in our project directory.

#### .travis.yml
{% highlight ruby %}
language: ruby
rvm:
  - 2.0.0
{% endhighlight %}

This file tells Travis that we are a ruby project, and we want Ruby 2.0.0 

#### Gemfile
{% highlight ruby %}
source 'https://rubygems.org'
gem "rspec"    , "~> 2.14.1"
{% endhighlight %}

This file tells Travis that we want to use a recent version of RSpec.
Create these two files for now, we will send them to GitHub after the next step.

### Sign up for Travis

Visit [Travis](https://travis-ci.org), sign up with your GitHub account and tell it to sync your MasterMind repository.

To tell Travis to make its first build, add the two files we just created using **git add** and **git push** them to GitHub.

### Watch your inbox.

This can take a few minutes, but eventually Travis will pick up the changes and try to run the tests on their own server. If it worked, you'll see this reward:

[![Build Status](https://travis-ci.org/tomv564/mastermind.png?branch=master)](https://travis-ci.org/tomv564/mastermind)