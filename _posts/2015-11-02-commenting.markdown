---
layout: default
title: Kommentarfunktion für deine Web App
permalink: commenting
---

# Kommentarfunktionalität für deine Web App

Wir werden im folgenden die Möglichkeit hinzufügen, Ideen mit Kommentaren zu versehen.
Im ersten Teil müssen wir ein Gerüst für die Kommentarfunktionalität erstellen.
Im zweiten Schritt müssen wir eine Beziehung zwischen Ideen und Kommentaren herstellen.
Im letzten Schritt geht es um die Aneige von Kommentaren.

## Gerüst für Kommentare erstellen

Wir erstellen im folgenden ein Gerüst für Kommentare.
Wir benötigen den Namen des Kommentators (`user_name`), den Kommentartext (`body`) und eine Referenz zur kommentierten Idee (`idea_id`).

{% highlight sh %}
rails g scaffold comment user_name:string body:text idea_id:integer
{% endhighlight %}

Dieser Befehl wird eine Migration erstellen, die deine Datenbank um die erwähnten Merkmale erweitert.
Um die Migration ablaufen zu lassen, müssen wir noch ausführen:

<div class="os-specific">
  <div class="nix">
{% highlight sh %}
bin/rake db:migrate
{% endhighlight %}
  </div>

  <div class="win">
{% highlight sh %}
ruby bin/rake db:migrate
{% endhighlight %}
  </div>
</div>


## Beziehungen herstellen

Wir müssen sicherstellen, dass Rails über die Beziehung zwischen den Objekten (Ideen und Kommentare) Bescheid weiß.
Da eine Idee mehrere Kommentare haben kann, stellen wir sicher, dass das Model `Idea` dies weiß.
Öffne die Datei `app/models/idea.rb` und füge nach der Zeile

{% highlight ruby %}
class Idea < ActiveRecord::Base
{% endhighlight %}

folgende hinzu:
{% highlight ruby %}
has_many :comments
{% endhighlight %}

Ein Kommentar muss auch wissen, dass er zu einer Idee gehört.
Wir öffnen also die Datei `app/models/comment.rb` und fügen nach der Zeile

{% highlight ruby %}
class Comment < ActiveRecord::Base
{% endhighlight %}

die folgende Zeile hinzu:

{% highlight ruby %}
belongs_to :idea
{% endhighlight %}

## Kommentarformular und Kommentare anzeigen

Öffne die Datei `app/views/ideas/show.html.erb`.
Füge nach dem `image_tag` Befehl

{% highlight erb %}
<%= image_tag(@idea.picture_url, :width => 600) if @idea.picture.present? %>
{% endhighlight %}

die folgenden Zeilen hinzu:

{% highlight erb %}
<% @comments.each do |comment| %>
  <div>
    <strong><%= comment.user_name %></strong>
    <br />
    <p><%= comment.body %></p>
    <p><%= link_to 'Delete', comment_path(comment), method: :delete, data: { confirm: 'Are you sure?' } %></p>
  </div>
<% end %>
<%= render 'comments/form' %>
{% endhighlight %}

In der Datei `app/controllers/ideas_controller.rb` fügen wir in die `show` Methode folgende Zeilen hinzu:

{% highlight erb %}
@comments = @idea.comments.all
@comment = @idea.comments.build
{% endhighlight %}

Zuletzt öffnen wir die Datei `app/views/comments/_form.html.erb`.

Nach den Zeilen

{% highlight erb %}
<div class="field">
    <%= f.label :body %><br />
    <%= f.text_area :body %>
  </div>
{% endhighlight %}

fügen wir folgendes an:

{% highlight erb %}
<%= f.hidden_field :idea_id %>
{% endhighlight %}

Wir entfernen noch die folgenden Zeile aus der Datei:

{% highlight erb %}
<div class="field">
  <%= f.label :idea_id %><br>
  <%= f.number_field :idea_id %>
</div>
{% endhighlight %}


Das wars schon. Schau dir nun eine Idee deiner Web App im Browser an und dort solltest du ein Formular finden, um einen Kommentar zu erstellen, aber auch ältere zu entfernen.
