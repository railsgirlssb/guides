---
layout: default
title: App Tutorial
permalink: app
---

# Anleitung zum Bauen deiner ersten Ruby on Rails App

## Lerne deine Werkzeuge kennen

<div class="indent" markdown="1">

<h3><i class="icon-text-editor">&nbsp;</i></h3>

### Texteditor

* Für unseren Workshop verwenden wir [Atom](https://atom.io/) oder [Sublime Text](http://www.sublimetext.com/3) als Texteditor. Diesen nutzen wir um Code zu schreiben und Dateien zu editieren.

<h3><i class="icon-prompt">&nbsp;</i></h3>

### Terminal

* Im Terminal starten wir einen Webserver und führen Befehle aus.

<h3><i class="icon-browser">&nbsp;</i></h3>

### Webbrowser

* In unserem Workshop verwenden wir Chrome oder Firefox, um unsere Anwendung zu betrachten.

</div>

### Wichtig

Wähle die Instruktionen für dein Betriebssystem.
Denn die Befehle, die man unter Windows ausführt, unterscheiden sich leicht von der Mac- oder Linux-Version der Befehle.



## *1.* Erstelle die Anwendung

Als Erstes erstellen wir eine neue Rails Anwendung mit dem Namen *railsgirls*.

Dazu müssen wir das Terminal öffnen:

* OS X: Öffne Spotlight, tippe *Terminal* ein und klicke auf die Anwendung *Terminal*.
* Windows: Klicke auf Start und suche nach dem Eintrag *Command Prompt with Ruby on Rails*.
* Linux (Ubuntu/Fedora): Suche im Dash nach *Terminal* und klicke auf *Terminal*.

Dann erstellen wir im Terminal unsere Rails Anwendung:

{% highlight sh %}
rails new railsgirls
{% endhighlight %}

Es wird eine neue Anwendung im Verzeichnis <code>railsgirls</code> generiert.
Mit Hilfe des Befehls `cd` wechseln wir in dieses Verzeichnis:

{% highlight sh %}
cd railsgirls
{% endhighlight %}

Anschließend starten wir den Webserver:

{% highlight sh %}
rails server
{% endhighlight %}

Öffne nun <http://localhost:3000> in deinem Webbrowser.
Wenn du eine Seite mit dem Text "Welcome aboard" vorfindest, hat die Generierung deiner neuen Anwendung funktioniert.

**Mögliche Fragen an deinen Coach:**

* Was machen die einzelnen Befehle?
* Was wurde dabei generiert?
* Welche Aufgaben hat der Server?


### Der Webserver

Wenn wir zurück in das Terminal wechseln, läuft dort weiterhin der Webserver.
Deshalb können wir keine weiteren Befehle ausführen.

Um den Webserver zu beenden, drückst du die Tastenkombination <kbd>Strg</kbd>+<kbd>C</kbd>.

Wenn der Webserver beendet ist, siehst du das Symbol für die Eingabeaufforderung:

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

Den Webserver kannst du dann erneut starten mit:

{% highlight sh %}
rails server
{% endhighlight %}



## *2.* Erstelle das Ideen Grundgerüst

Wir generieren nun mit Hilfe der Rails Scaffold Funktion ein Grundgerüst. Mit diesem können wir Dinge (hier Ideen), auflisten, hinzufügen, löschen, bearbeiten und anschauen.

Um das Ideen Grundgerüst zu erstellen, führen wir den folgenden Befehl aus:

{% highlight sh %}
rails generate scaffold idea name:string description:text picture:string
{% endhighlight %}

Das Grundgerüst erstellt neue Dateien in deinem Projektverzeichnis.
Wir müssen jedoch noch ein paar andere Befehle ausführen, um die Funktionalität zu gewährleisten.
Mit den folgenden Befehlen aktualisierst du die Datenbank und startest den Server neu:

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

Öffne nun <http://localhost:3000/ideas> in deinem Browser.

Klicke etwas herum, um zu testen was du mit den wenigen Befehlen erstellt hast.

**Mögliche Fragen an deinen Coach:**

* Was ist Rails Scaffolding?
* Lasse dir den Befehl, den Namen des Models und die dazu gehörige Datenbanktabelle, sowie Konventionen zur Benennung, Attribute und Typen, etc. erklären
* Was sind Migrationen und wofür werden sie gebraucht?



## *3.* Füge ein neues Design hinzu

**Mögliche Fragen an deinen Coach:**

* Wie hängen HTML und Rails zusammen?
* Welcher Teil der Views ist HTML und welcher ist Embedded Ruby (ERB)?
* Was ist MVC und wie hängt das alles zusammen? (Models und Controllers sind für die Generierung der HTML Views zuständig.)

Als nächstes kümmern wir uns um ein schöneres Aussehen unserer Applikation.
Dazu verwenden wir Twitter Bootstrap.

Öffne `app/views/layouts/application.html.erb` in deinem Texteditor und über die Zeile:

{% highlight erb %}
<%= stylesheet_link_tag "application", media: "all", "data-turbolinks-track" => true %>
{% endhighlight %}

fügst du folgendes hinzu:

{% highlight erb %}
<link rel="stylesheet" href="//railsgirls.com/assets/bootstrap.css">
<link rel="stylesheet" href="//railsgirls.com/assets/bootstrap-theme.css">
{% endhighlight %}

und ersetzt:

{% highlight erb %}
<%= yield %>
{% endhighlight %}

mit:

{% highlight erb %}
<div class="container">
  <%= yield %>
</div>
{% endhighlight %}

Nun fügen wir noch eine Navigation und eine Fussleiste zum Layout hinzu. Dies machen wir in der selben Datei, indem wir unter `<body>` folgendes hinzufügen:

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

und vor `</body>`:

{% highlight html %}
<footer>
  <div class="container">
    Rails Girls 2015
  </div>
</footer>
<script src="//railsgirls.com/assets/bootstrap.js"></script>
{% endhighlight %}

Das Styling der Ideentabelle passen wir ebenfalls an. Öffne dazu `app/assets/stylesheets/application.css` und füge am Ende folgendes hinzu:

{% highlight css %}
body { padding-top: 100px; }
footer { margin-top: 100px; }
table, td, th { vertical-align: middle; border: none; }
th { border-bottom: 1px solid #DDD; }
{% endhighlight %}

Vergewissere dich, dass deine Dateien gespeichert sind.
Dann lädst du die Seite im Browser neu, um die Änderungen zu sehen.
Du kannst auch gerne noch weiter das HTML und CSS verändern.

**Mögliche Fragen an deinen Coach:**

* Was ist CSS?
* Was sind Layouts?



## *4.* Füge das Hochladen von Bildern hinzu

Um in Rails Bilder hochladen zu können, müssen wir noch eine Software installieren.

**Mögliche Fragen an deinen Coach:**

* Was sind Bibliotheken?
* Warum sind sie nützlich?
* Was ist Open Source Software?

Zum Installieren der Software öffne `Gemfile` in deinem Texteditor und unter die Zeile:

{% highlight ruby %}
gem 'sqlite3'
{% endhighlight %}

fügst du folgendes hinzu:

{% highlight ruby %}
gem 'carrierwave'
{% endhighlight %}

Beende dann den Server im Terminal mit <kbd>Strg</kbd>+<kbd>C</kbd>.

Und führe folgendes aus:

{% highlight sh %}
bundle
{% endhighlight %}

Danach fügen wir Code hinzu, der das Hochladen von Bildern ermöglicht. Im Terminal führen wir folgendes aus:

{% highlight sh %}
rails generate uploader Picture
{% endhighlight %}

Jetzt starten wir wieder den Rails server.

Öffne `app/models/idea.rb` und unter die Zeile:

{% highlight ruby %}
class Idea < ActiveRecord::Base
{% endhighlight %}

fügst du folgendes hinzu:

{% highlight ruby %}
mount_uploader :picture, PictureUploader
{% endhighlight %}

Öffne `app/views/ideas/_form.html.erb` und ändere die Zeile:

{% highlight erb %}
<%= f.text_field :picture %>
{% endhighlight %}

in folgendes:

{% highlight erb %}
<%= f.file_field :picture %>
{% endhighlight %}

Öffne `app/views/ideas/_form.html.erb` und ändere die Zeile:

{% highlight erb %}
<%= form_for(@idea) do |f| %>
{% endhighlight %}

in folgendes:

{% highlight erb %}
<%= form_for @idea, :html => {:multipart => true} do |f| %>
{% endhighlight %}

Erstelle nun eine neue Idee und lade ein Bild dazu in deinem Browser hoch.
Nach dem Speichern siehst du jedoch kein Bild, sondern nur den Pfad zum Bild.
Um dies zu ändern öffnen wir `app/views/ideas/show.html.erb` und ändern:

{% highlight erb %}
<%= @idea.picture %>
{% endhighlight %}

in folgendes:

{% highlight erb %}
<%= image_tag(@idea.picture_url, :width => 600) if @idea.picture.present? %>
{% endhighlight %}

Lade nun die Seite in deinem Browser neu und betrachte die Änderung.

**Mögliche Fragen an deinen Coach:**

* Was macht HTML?



## *5.* Passe die Routen an

Öffne <http://localhost:3000> und du sieht immer noch die "Welcome aboard" Seite. Wir erstellen nun eine Weiterleitung zur Ideen Seite.

Öffne `config/routes.rb` und füge nach der ersten Zeile folgendes hinzu:

{% highlight ruby %}
root :to => redirect('/ideas')
{% endhighlight %}

Teste die Änderung, in dem du den Rootpfad (<http://localhost:3000/>) im Browser öffnest.

**Mögliche Fragen an deinen Coach:**

* Was sind Routen? Lasse dir dabei Details wie z.B. die Reihenfolge der Routen und deren Relation zu statischen Dateien erklären.

## Erstelle eine statische Seite in deiner App

Als Letztes erstellen wir eine statische Seite in unserer Applikation, die Informationen über dich (des Autors) beinhaltet.

{% highlight sh %}
rails generate controller pages info
{% endhighlight %}

Dieser Befehl erstellt einen neuen Ordner unter `app/views` namens `/pages` und darin eine Datei namens `info.html.erb`.
Diese wird deine Info Seite sein.

Außerdem wird noch eine neue einfache Route in deiner `config/routes.rb` hinzugefügt:

{% highlight ruby %}
get "pages/info"
{% endhighlight %}

Öffne `app/views/pages/info.html.erb` und füge Informationen über dich in HTML hinzu. Um die Seite anschliessend anzusehen, gehst du in deinem Browser auf  <http://localhost:3000/pages/info>.


## Hauptteil des Workshops beendet!

Gratulation, du hast den Hauptteil des Workshops abgeschlossen!

Gehe auf die Übersichtsseite (<http://guides.railsgirlssb.de>) zurück, um weiter an deiner Applikation zu arbeiten.
