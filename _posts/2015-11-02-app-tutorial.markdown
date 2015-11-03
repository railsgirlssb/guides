---
layout: default
title: App Tutorial
permalink: app
---

# Anleitung zum Bauen deiner ersten Ruby on Rails App

## Get to know the tools

<div class="indent" markdown="1">

<h3><i class="icon-text-editor">&nbsp;</i></h3>

### Texteditor

* [Atom](https://atom.io/) und [Sublime Text](http://www.sublimetext.com) sind Beispiele für Texteditoren. die man für das Schreiben von Code und zum Editieren von Dateien nutzen kann.

<h3><i class="icon-prompt">&nbsp;</i></h3>

### Terminal

* Im Terminal starten wir einen Rails Webserver und führen Befehle aus.

<h3><i class="icon-browser">&nbsp;</i></h3>

### Webbrowser

* Wir können Firefox, Safari oder Chrome benutzen, um unsere Anwendung zu betrachten.

</div>

### Important

Es ist wichtig, dass du die Instruktionen für dein Betriebssystem auswählst.
Die Befehle, die man unter Windows ausführt, unterscheiden sich leicht von der Mac- oder Linux-Version der Befehle.

## *1.* Die Anwendung erstellen

Wir geben unserer neuen Rails Anwendung den Namen *railsgirls*.
Hierzu führen wir den folgenden Befehl im Terminal aus:

<div class="os-specific">
  <div class="nix">
{% highlight sh %}
rails new railsgirls
{% endhighlight %}
    <div>
      <p>
      Es wird eine neue Anwendung im Verzeichnis <code>railsgirls</code> generiert.
      Wir können in das Verzeichnis mit Hilfe des folgenden Befehls wechseln:
      </p>
    </div>

{% highlight sh %}
cd railsgirls
{% endhighlight %}
    <div>
<p>Anschließend starten wir den Rails Webserver:</p>
    </div>

{% highlight sh %}
rails server
{% endhighlight %}
  </div>

  <div class="win">
{% highlight sh %}
rails new railsgirls
{% endhighlight %}

    <div>
<p>This will create a new app in the folder <code>railsgirls</code>, so we again want to change the directory to be inside of our rails app by running:</p>
    </div>

{% highlight sh %}
cd railsgirls
{% endhighlight %}

    <div>
<p>If you run <code>dir</code> inside of the directory you should see folders such as <code>app</code> and <code>config</code>. You can then start the rails server by running:</p>
    </div>

{% highlight sh %}
rails server
{% endhighlight %}
  </div>
</div>

Open <http://localhost:3000> in your browser. If you are using a cloud service (e.g. nitrous), use its preview functionality instead (see [installation guide](/install#using-a-cloud-service) for details).

You should see "Welcome aboard" page, which means that the generation of your new app worked correctly.

Notice in this window the command prompt is not visible because you are now in the Rails server, the command prompt looks like this:

<div class="os-specific">
  <div class="nix">
{% highlight sh %}
$
{% endhighlight %}
  </div>
  <div class="win">
{% highlight sh %}
>
{% endhighlight %}
  </div>
</div>

When the command prompt is not visible you cannot execute new commands. If you try running `cd` or another command it will not work. To return to the normal command prompt:

Hit <kbd>Ctrl</kbd>+<kbd>C</kbd> in the terminal to quit the server.

**Coach:** Explain what each command does. What was generated? What does the server do?


## *2.*Create Idea scaffold

We're going to use Rails' scaffold functionality to generate a starting point that allows us to list, add, remove, edit, and view things; in our case ideas.

**Coach:** What is Rails scaffolding? (Explain the command, the model name and related database table, naming conventions, attributes and types, etc.) What are migrations and why do you need them?

{% highlight sh %}
rails generate scaffold idea name:string description:text picture:string
{% endhighlight %}

The scaffold creates new files in your project directory, but to get it to work properly we need to run a couple of other commands to update our database and restart the server.

<div class="os-specific">
  <div class="nix">
{% highlight sh %}
bin/rake db:migrate
rails server
{% endhighlight %}
  </div>

  <div class="win">
{% highlight sh %}
ruby bin/rake db:migrate
rails server
{% endhighlight %}
  </div>
</div>

Open <http://localhost:3000/ideas> in your browser. Cloud service (e.g. nitrous) users need to append '/ideas' to their preview url instead (see [installation guide](/install#using-a-cloud-service)).

Click around and test what you got by running these few command-line commands.

Hit <kbd>Ctrl</kbd>+<kbd>C</kbd> to quit the server again when you've clicked around a little.


## *3.*Design

**Coach:** Talk about the relationship between HTML and Rails. What part of views is HTML and what is Embedded Ruby (ERB)? What is MVC and how does this relate to it? (Models and controllers are responsible for generating the HTML views.)

The app doesn't look very nice yet. Let's do something about that. We'll use the Twitter Bootstrap project to give us nicer styling really easily.

Open `app/views/layouts/application.html.erb` in your text editor and above the line

{% highlight erb %}
<%= stylesheet_link_tag "application", media: "all", "data-turbolinks-track" => true %>
{% endhighlight %}

add

{% highlight erb %}
<link rel="stylesheet" href="//railsgirls.com/assets/bootstrap.css">
<link rel="stylesheet" href="//railsgirls.com/assets/bootstrap-theme.css">
{% endhighlight %}

and replace

{% highlight erb %}
<%= yield %>
{% endhighlight %}

with

{% highlight erb %}
<div class="container">
  <%= yield %>
</div>
{% endhighlight %}

Let's also add a navigation bar and footer to the layout. In the same file, under `<body>` add

{% highlight html %}
<nav class="navbar navbar-default navbar-fixed-top" role="navigation">
  <div class="container">
    <div class="navbar-header">
      <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="/">The Idea app</a>
    </div>
    <div class="collapse navbar-collapse">
      <ul class="nav navbar-nav">
        <li class="active"><a href="/ideas">Ideas</a></li>
      </ul>
    </div>
  </div>
</nav>
{% endhighlight %}

and before `</body>` add

{% highlight html %}
<footer>
  <div class="container">
    Rails Girls 2015
  </div>
</footer>
<script src="//railsgirls.com/assets/bootstrap.js"></script>
{% endhighlight %}

Now let's also change the styling of the ideas table. Open `app/assets/stylesheets/application.css` and at the bottom add

{% highlight css %}
body { padding-top: 100px; }
footer { margin-top: 100px; }
table, td, th { vertical-align: middle; border: none; }
th { border-bottom: 1px solid #DDD; }
{% endhighlight %}

Now make sure you saved your files and refresh the browser to see what was changed. You can also change the HTML & CSS further.

In case your Terminal shows you an error message that *sort of* implies there is something wrong with your JavaScript or CoffeeScript, install [nodejs](http://nodejs.org/download/). This issue should not appear when you've used the RailsInstaller (but when you've installed Rails via ```gem install rails```).

**Coach:** Talk a little about CSS and layouts.


## *4.*Adding picture uploads

We need to install a piece of software to let us upload files in Rails.

Open `Gemfile` in the project directory using your text editor and under the line

{% highlight ruby %}
gem 'sqlite3'
{% endhighlight %}

add

{% highlight ruby %}
gem 'carrierwave'
{% endhighlight %}

**Coach:** Explain what libraries are and why they are useful. Describe what open source software is.

Hit <kbd>Ctrl</kbd>+<kbd>C</kbd> in the terminal to quit the server.

In the terminal run:

{% highlight sh %}
bundle
{% endhighlight %}

Now we can generate the code for handling uploads. In the terminal run:

{% highlight sh %}
rails generate uploader Picture
{% endhighlight %}

Please start the rails server now.

Note: Some people might be using a second terminal to run the rails server continuously.
If so you need to **restart the Rails server process** now.
This is needed for the app to load the added library.

Open `app/models/idea.rb` and under the line

{% highlight ruby %}
class Idea < ActiveRecord::Base
{% endhighlight %}

add

{% highlight ruby %}
mount_uploader :picture, PictureUploader
{% endhighlight %}

Open `app/views/ideas/_form.html.erb` and change

{% highlight erb %}
<%= f.text_field :picture %>
{% endhighlight %}

to

{% highlight erb %}
<%= f.file_field :picture %>
{% endhighlight %}

Sometimes, you might get an *TypeError: can't cast ActionDispatch::Http::UploadedFile to string*.

If this happens, in file `app/views/ideas/_form.html.erb` change the line

{% highlight erb %}
<%= form_for(@idea) do |f| %>
{% endhighlight %}

to

{% highlight erb %}
<%= form_for @idea, :html => {:multipart => true} do |f| %>
{% endhighlight %}

In your browser, add new idea with a picture. When you upload a picture it doesn't look nice because it only shows a path to the file, so let's fix that.

Open `app/views/ideas/show.html.erb` and change

{% highlight erb %}
<%= @idea.picture %>
{% endhighlight %}

to

{% highlight erb %}
<%= image_tag(@idea.picture_url, :width => 600) if @idea.picture.present? %>
{% endhighlight %}

Now refresh your browser to see what changed.

**Coach:** Talk a little about HTML.


## *5.*Finetune the routes

Open <http://localhost:3000> (or your preview url, if you are using a cloud service). It still shows the "Welcome aboard" page. Let's make it redirect to the ideas page.

Open `config/routes.rb` and after the first line add

{% highlight ruby %}
root :to => redirect('/ideas')
{% endhighlight %}

Test the change by opening the root path (that is, <http://localhost:3000/> or your preview url) in your browser.

**Coach:** Talk about routes, and include details on the order of routes and their relation to static files.

**Rails 3 users:** You will need to delete the index.html from the `/public/` folder for this to work.

## Create static page in your app

Lets add a static page to our app that will hold information about the author of this application — you!

{% highlight sh %}
rails generate controller pages info
{% endhighlight %}

This command will create you a new folder under `app/views` called `/pages` and under that a file called `info.html.erb` which will be your info page.

It also adds a new simple route to your routes.rb.

{% highlight ruby %}
get "pages/info"
{% endhighlight %}

Now you can open the file `app/views/pages/info.html.erb` and add information about you in HTML. To see your new info page, take your browser to <http://localhost:3000/pages/info> or, if you are a cloud service user, append '/pages/info' to your preview url.
