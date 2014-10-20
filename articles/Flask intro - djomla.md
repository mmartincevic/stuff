Flask for masses
===================

#### What is flask?

Here's what the official flask documentation says :

> Flask is a microframework for Python based on Werkzeug, Jinja 2 and good intentions.

So to explain this quote a bit further, werkzeug is python utility library and jinja 2 is template engine for Python.


#### Why flask?

Year ago i was looking into another programming language outside of PHP. I was an active PHP developer for 10 years, and, to be honest, doing apps only in PHP get's a bit boring. Also, having not really standardised frameworks, libraryes also didn't helped. I found myself spending half of my time reinventing the wheel just for the sake of reinventing something. Thinking i know better than the other PHP developer often leads in spending too much time for the project. 
This is so common in PHP world. Almost every other developer is building his own framework. And this leads to chaos, but let's not go there now.

Someone would say but "But nowadays you have composer"
Sure, nowadays you have things like composer and you can base your project on that one and fully depend on other people libraries, just like in Python. Again, using composer i very often had troubles with lack of documentation, tests, strange bugs.

So i was searching for something new and at work we just started to use Django for serious web development. Django at that point seemed like too big of a framework and i wanted something small that i can build upon. And i stumbled upon Flask. 
Since then i've been using flask for most of mine personal development projects. 

In this article i will cover installing and configuring flask and basic hello world example. Also i will persume that you have linux machine installed (or mac). If you are using windows machine for development - we need to have a serious talk.


### Download & Install 
To get started you will need Python 2.6 or higher, so please make sure you have a valid python installation, and this will not cover Python 3.x although the differences should be minimal.

First we need **virtualenv**.

Brief intro on virtualenv:
As a Python developer you will most likely build different application with different dependencies, maybe even on different python versions. To solve dependencies and used libraries stacking on your machine we use virtualenv. Virtualenv creates a separate environment for your application. In that way you can keep you application environments fully isolated. For more info go to :

But let's install it

> $ sudo pip install virtualenv

You could even do something like (if on Ubuntu)

> sudo apt-get install python-virtualenv

Once the virtualenv is installed fire up shell and create a new environment.

> mkdir flaskproject
> cd flaskproject
> virtualenv env
> New python executable in env/bin/python
Installing distribute............done.

Now you have a directory called "flaskproject" and inside that directory another one (with you environment) called "env". To start
using your environment you need to "activate" it.

> . env/bin/activate

And finally let's install Flask.

> pip install Flask

At the end you should see something like:

>Successfully installed Flask Werkzeug Jinja2 itsdangerous markupsafe
Cleaning up...

If you inspect your folder notice that you will still see only your env directory. And that's ok, because Flask is installed inside you env directory.

All the packages installed will be inside site-packages directory.

> env/lib/python2.6/site-packages

Or you can do "pip list" that will list all installed packages inside your environment. Don't be afraid if you see more than Flask, because Flask installs it's own dependencies too.

> pip list

This is it, now you have your environment and application ready to build upon.

### Your first Flask application

Let's go and create app.py inside our directory and this file will contain this:

 

    from flask import Flask
    app = Flask(__name__)
    app.debug = True
    
    @app.route('/')
    def main_method():
        return "Hello everyone"
    
    if __name__ == '__main__':
        app.run()


If we run this code from the console, we will get something like 

> $ python app.py
 * Running on http://127.0.0.1:5000/

If you fireup your browser and go to given address you should see your message.

Now what have we done here :

From flask library import Flask class
>    from flask import Flask

Then we create an instance of the Flask class

> app = Flask(__name__)

I often use, while development, debug mode on. It gives me plenty
of messages and also when i save some files server gets automatically reloaded. 

> app.debug = True

Next we create a method that will return simple string.

     def main_method():
	     return "Hello everyone"

And on top of our method we use routing decorator to specify
url for this method. Any request comming to "/" will be redirected
to main_method. Simple?

> @app.route('/') 


Final piece of code makes sure if the script is run from within shell directly with python interpreter, and not used like a module for import then run it.

    if __name__ == '__main__':
            app.run()

### Templates fun

Now let's create a url that will receive parameter and pass it to the template. /fun/params/optional_parameter

Add these lines in our app.py

    @app.route('/fun/urls/<param>')
    def fun_url(param=None):
        return render_template('fun_url.html')

In first directive we have new route that will redirect to our
new method. Because in route we defined a parameter at the end, our method needs to expect that parameter.
After we return template with render_template method. 

If you save app.py, and have active server running it will break with an error of unknown method render_template. Simply because we are calling method that does not exist or at least it was not imported. In our case we still need to import this method. 
On top of our file add this:

> from flask import render_template

By default Flask expects template files to be located in templates directory relative to our app.py. So we need to create a new directory there.

> mkdir templates
> cd templates
> touch fun_url.html

This will create templates directory and inside it will create an empty
fun_url.html file.

Now if we restart our application

> $ python app.py
 * Running on http://127.0.0.1:5000/

And we open url, for example:

> http://127.0.0.1:5000/fun/url/test

We should see an empty page. Please try to add something to that file and restart server. You should be able to see your updated template file results.

Now let's pass parameter to a template. Check this line :

> return render_template('fun_url.html')

If we slightly modify that line and add following:

    return render_template('fun_url.html', fun_param=param)

With this we direct method render_template to render our template and pass param into it as fun_param. So if we open fun_url.html and add :

    Parameter passed is: {{ fun_param }}


Save results and restart server. You should get this response:

> http://127.0.0.1:5000/fun/url/test
> Parameter passed is: test

>http://127.0.0.1:5000/fun/url/test1
> Parameter passed is: test1


### Where to go from here

Things written in this article, in my opinion, are not the things that Flask development really shines. These are more beginner intro so you can have a feeling of how Flask works. As soon as you start working with databases, user access, rest api that's where things get really nice and simple. But for now the best place to start is 

[Official Flask documentation](http://flask.pocoo.org/docs/0.10/)

Also really great article that covers almost everything you will need in further developement

[The Flask Mega-Tutorial](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)