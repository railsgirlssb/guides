---
layout: default
title: Web-App um Kommentarfunktion erweitern
permalink: commenting
---

# Web-App um Kommentarfunktion erweitern

Wir werden nun die zuvor gebaute Web-App erweitern, indem man Kommentare zu Ideen erstellen kann.



## Gerüst für Kommentare erstellen

Wir erstellen im folgenden ein Gerüst für Kommentare.
Wir benötigen den Namen des Kommentators (`user_name`), den Kommentartext (`body`) und eine Referenz zur kommentierten Idee (`idea_id`).

{% highlight sh %}
rails g scaffold comment user_name:string body:text idea_id:integer
{% endhighlight %}

Dieser Befehl wird eine Migration erstellen, die deine Datenbank um die erwähnten Merkmale erweitert.
Um die Migration ablaufen zu lassen, müssen wir noch folgenden Befehl ausführen:

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

Wir müssen sicherstellen, dass Rails die Beziehung zwischen den Objekten (Ideen und Kommentare) kennt.
Eine Idee kann dabei mehrere Kommentare haben.
Öffne die Datei `app/models/idea.rb` und füge nach der Zeile:

{% highlight ruby %}
class Idea < ActiveRecord::Base
{% endhighlight %}

folgendes hinzu:

{% highlight ruby %}
has_many :comments
{% endhighlight %}

Ein Kommentar muss auch wissen, dass er zu einer Idee gehört.
Wir öffnen also die Datei `app/models/comment.rb` und fügen nach der Zeile:

{% highlight ruby %}
class Comment < ActiveRecord::Base
{% endhighlight %}

die folgende Zeile hinzu:

{% highlight ruby %}
belongs_to :idea
{% endhighlight %}



## Kommentarformular erstellen

Um Kommentare erstellen zu können, brauchen wir noch ein Formular.

Dazu öffnen wir die Datei `app/views/comments/_form.html.erb`. Nach den Zeilen

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



## Kommentare anzeigen

Um die Kommentare anzeigen zu lassen, müssen wir sie noch unter einer Idee anzeigen lassen.

Öffne die Datei `app/views/ideas/show.html.erb`.
Füge nach der Zeile:

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

In der Datei `app/controllers/ideas_controller.rb` fügen wir in die `show` Methode folgendes hinzu:

{% highlight erb %}
@comments = @idea.comments.all
@comment = @idea.comments.build
{% endhighlight %}

Öffne nun eine beliebige Idee unter <http://localhost:3000/ideas> in deinem Browser. Dort solltest du ein Formular finden um einen Kommentar zu erstellen sowie eine Auflistung von erstellten Kommentare, die auch gelöscht werden können.


## Erweiterung der Web-App abgeschlossen

Gratulation, du hast deine Web-App um eine Funktionalität erweitert!

Gehe auf die Übersichtsseite (<http://guides.railsgirlssb.de>) zurück, um weiter an deiner Applikation zu arbeiten.
