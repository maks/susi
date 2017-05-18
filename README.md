# SuSi<sup>2</sup>
## Because static site generation shouldn't be rocket science.


This is the **Su**&#8203;per **Si**&#8203;mple **Si**&#8203;te generator.

You give it markdown files and (if you fancy) an HTML layout to render them into - and it gives you static HTML.

That's it.
No magic, no fancy build tools - Markdown and HTML. Now go and make that website!

## How to use it

To setup, you run

```shell
    npm install -g susi
```

and then to parse markdown files into HTML you can use:

```shell
    susi directory/with/markdown_files/ output_directory/ path/to/layouts/ path/to/partials
```

So, for example:

```shell
    susi /var/www/markdown /var/www/html  /var/www/layout.html /var/www/partials
```

will parse all markdown files in `/var/www/markdown/` and will create corresponding HTML files in `/var/www/html/` using the `/var/www/layout.html` file and the
partials files in `/var/www/partials`.

## Using a layout

Now parsing naked Markdown files into naked HTML often isn't enough, so there's the layouts which you pass in as the third parameter.

At least create a single layout file called `page.html`.

### A basic layout
Say you have a layout with a few navigation links and some css like this:

```html
  <!doctype html>
  <html>
    <head>
      <link rel="stylesheet" href="style.css">
      <title>{{title}}</title>
    </head>
    <body>
      <nav>
        <ul>
          <li><a href='home.html'>Home</a></li>
          <li><a href='projects.html'>Projects</a></li>
          <li><a href='contact.html'>Contact</a></li>
      </nav>
      <h1>{{title}}</h1>
      <h2>{{date}}</h2>
      <section id="main">{{contents}}</section>
    </body>
  </html>
```

When you pass in the path to this files directory as the third parameter, SuSi will render each markdown file into the place of ``{{contents}}``.
So if you do:

```shell
  susi input/ output/ path/to/layout/
```

A markdown file like this:
```markdown
{
  "title": "My Site",
  "date": "2014-11-27"
}

---

# Some headline

Some *text* of mine.
```

will be rendered into:

```html
  <!doctype html>
  <html>
    <head>
      <link rel="stylesheet" href="style.css">
      <title>My Site</title>
    </head>
    <body>
      <nav>
        <ul>
          <li><a href='home.html'>Home</a></li>
          <li><a href='projects.html'>Projects</a></li>
          <li><a href='contact.html'>Contact</a></li>
      </nav>
      <section id="main">
        <h1>My Site</h1>
        <h2>2014-11-27</h2>
        <p>Some <em>text</em>of mine.</p>
      </section>
    </body>
  </html>
```

which is pretty handy.

Each markdown file is expected to have a

### Using Frontmatter

Each markdown file is expected to have a a frontmatter section formatted in JSON like so:
```json
{
  "title": "new site gen",
  "date": "2014-11-27",
  "layout:" "page"
}
```

*Note:* A triple dash '---' on a new line is **required** to seperate the frontmatter from the markdown formatted text.

*Note:* You may leave out the `layout` field, which then defaults to `page`.

The layout attribute will be used to look for a file with a matching name and ".html" suffix in the directory
specified as the third parameter on the commandline to susi.

Of course additional, custom attributes can be optionally included in the frontmatter json and can then be used
in all the html layout files using the "Mustache" style syntax.

### Includes

You can use Mustache partials to include file from the `partials` directory, to help keep things DRY,.
*NOTE:* file names of partials will have period replaced with underscore characters.
 eg.

```html
  {{>meta_html}}
  {{>header_html}}
    <div class="container">
      <div class="starter-template">
        <h1>{{title}}</h1>
        <p class="lead">
          {{contents}}
        </p>
      </div>
    </div><!-- /.container -->
  {{>footer_html}}
```

*Note:* the partials files can use Mustache syntax as well.

Now, seriously, go make static websites!

# License
SuSi is available under the [ISC License](LICENSE).

# Contributing
If you have any suggestions, found bugs or want a new feature, don't hesitate to submit a pull request or open an issue.

Pull requests are always welcome, no matter how small. But if you're about to create large, sweeping changes, I suggest opening an issue *before* making those changes to check, if they're making sense in the scope of the project and to discuss them to avoid later frustration.
