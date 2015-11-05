---
layout: default
title: Web App ins Internet stellen
permalink: deployment
---

# Web App ins Internet stellen

Es gibt viele Möglichkeiten seine Anwendung online zu stellen.
Wir möchten euch einen Weg mit Hilfe von `Cloud Foundry` und anynines zeigen, da Cloud Foundry die Zukunft ist.

## Cloud Foundry / anynines

[Cloud Foundry](https://www.cloudfoundry.org) ist eine Computer Plattform, die Entwicklern von Webanwendungen zur Verfügung gestellt wird.
Der Vorteil hierbei ist, dass man sehr schnell eine produktive Umgebung erstellen kann, ohne extra Hard- und Software anschaffen zu müssen.
Man vermeidet auch teuren administrativen Aufwand.
Das sorgt natürlich dafür, dass die App sehr schnell online gehen kann.

![Cloud Foundry](/images/cloud-foundry.png "Cloud Foundry")

In Deutschland gibt es eine öffentliche Installation von Cloud Foundry namens `anynines`.
[anynines](http://anynines.com) wird sogar von Saarbrücken aus entwickelt.

![anynines](/images/anynines.png "anynines")

## anynines einrichten

1. [Erstelle einen kostenlosen anynines Zugang](http://anynines.com/).

2. [Lade und installiere das Kommandozeilenprogramm cf](https://anynines.zendesk.com/entries/60241846-How-to-install-the-CLI-v6), um mit anynines arbeiten zu können.

3. Anschließend müssen wir uns gegenüber anynines authentifizieren, indem wir die erhaltenen Zugangsdaten ausnutzen:

{% highlight sh %}
cf api https://api.de.a9s.eu
cf login
{% endhighlight %}

Du wirst nach der E-Mail-Adresse und dem Passwort gefragt, mit denen du dich registriert hast.

Anschließend fragt das Tool noch, welchen `space` (Umgebung) du nutzen möchtest.

{% highlight sh %}
Select a space (or press enter to skip):
1. production
2. staging
3. test
{% endhighlight %}

Wir wählen `1` für "production" aus, d.h. wir wollen in die Produktionsumgebung unsere Anwendung laden.

## Änderungen für anynines

Öffne die Datei `config/environments/production.rb` und setze die Zeile mit dem Text `config.serve_static_files` auf:
{% highlight sh %}
config.serve_static_files = true
{% endhighlight %}

In den nächsten Schritten werden deine lokale Dateien, die die Applikation ausmachen, direkt zu anynines hochgeladen.
Wir müssen verhindern, dass gewisse Verzeichnisse mit hochgeladen werden, die nicht zu anynines gelangen sollten.
So wird z.B. beim Hochladen eines Bildes für eine Idee die hochgeladene Datei lokal in deinem Projektverzeichnis abgelegt.
Diese Datei benötigen wir aber nicht auf dem anynines Server.
Führe also folgende Befehle aus, um das Hochladen dieser temporären Dateien zu verhindern:

{% highlight sh %}
echo "public/uploads" >> .cfignore
echo "tmp" >> .cfignore
echo "logs" >> .cfignore
{% endhighlight %}


## Web App online stellen
Nun können wir unsere Applikation zum ersten Mal auf anynines `ausliefern` bzw. `deployen`.

Wir müssen anynines einen Namen für unsere Applikation mitteilen.
Es ist wichtig, dass wir im folgenden anynines einen eindeutigen Namen (kein anderer Teilnehmer sollte den gleichen wählen) für die Applikation mitteilen, da andere Teilnehmerinnen nicht den gleichen Namen benutzen dürfen.
Überleg dir also einen Namen, wie etwa `rails-girls-ZUFALLSNAME`, wobei du ZUFALLSNAME durch ein eindeutiges Wort ersetzt (ohne Sonderzeichen), das die anderen Teilnehmer nicht wählen würden. Vielleicht ist dein Vor- oder Nachname eine Möglichkeit.

Nun ist es an der Zeit, die Applikation auf anynines zu veröffentlichen:
{% highlight sh %}
cf push rails-girls-ZUFALLSNAME -c "bundle exec rake db:migrate && bundle exec rails s -p $PORT" -b http://github.com/cloudfoundry/ruby-buildpack.git#v1.3.1
{% endhighlight %}


Deine Applikation ist nun, wenn keine Fehler auftraten, unter der Adresse `rails-girls-ZUFALLSNAME.de.a9sapp.eu` im Internet erreichbar.

![Deployment](/images/windows-deployment.png "Deployment")




## Probleme

Die Applikation ist nun zwar öffentlich im Web verfügbar, aber es gibt noch Probleme, die einen richtigen Livegang verhindern.

* Dateien hochladen: Beim nächsten `Deploy` der Applikation sind alle Bilddateien weg
* Datenbank: Beim nächsten `Deploy` ist die Datenbank wieder leer

Im nächsten Schritt könnte man diese Probleme angehen, aber das sollte nicht das Thema für heute sein :-)
