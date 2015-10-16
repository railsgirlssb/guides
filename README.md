# Rails Girls Saarbrücken Guides

### Installing jekyll

```
$ bundle install
```

### Pygments and Code Highlighting

The guides use the [pygments](http://pygments.org/) library to do syntax highlighting. If you don't have it installed you won't be able to see the highlight sections like the following:

```
{% highlight %}
{% endhighlight %}
```

If you aren't editing the code blocks, you can safely ignore this. If you want pygments, you can follow the [install instructions](https://github.com/mojombo/jekyll/wiki/Install) in the "Pygments" section.

### Run jekyll

```
$ jekyll server --watch
```

### Styling

Wrap keyboard shortcuts with [kbd](https://www.w3.org/wiki/HTML/Elements/kbd) HTML tag.

To make posts consistent in style use `Ctrl+C` over `CTRL-c`/`ctrl+c`

```
To shut down the server you can hit <kbd>Ctrl</kbd>+<kbd>C</kbd>
```
