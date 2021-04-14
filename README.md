# SyntaxHighlighter v4

SyntaxHighlighter is a client side highlighter for the web and web-apps. It's been around since 2004 and it's used virtually everywhere to seamlessly highlight code for presentation purposes.

<img src="screenshot.png" width="640"/>

SyntaxHighlighter is currently used and has been used in the past by Microsoft, Apache, Mozilla, Yahoo, Wordpress, Bug Labs, Freshbooks and many other companies and blogs.

## Building Instructions

SyntaxHighlighter lets you build a single `.js` file that will include the core, CSS theme and the syntaxes that you wish to use. The process is very simple and consists of just a few steps.

### Project Setup

```
$ git clone https://github.com/syntaxhighlighter/syntaxhighlighter.git
$ cd syntaxhighlighter
$ npm install

$ ./node_modules/gulp/bin/gulp.js setup-project
```

The `./node_modules/gulp/bin/gulp.js setup-project` command clones ALL of the repositories from https://github.com/syntaxhighlighter and places them into the `repos` subfolder. You are now ready to build your own distribution file.

### Building `syntaxhighlighter.js`

By running `./node_modules/gulp/bin/gulp.js build` you will make `syntaxhighlighter.js`, `theme.css`, and associated files. Note that the options `--brushes` and `--theme` **do not** have default values. If you will not specify them, you will get a script that doesn't do any syntax highlighting on its own, and you will not get the CSS file.

```
$ ./node_modules/gulp/bin/gulp.js build --help

Options:
  --brushes  Comma separated list of brush names or paths to be bundled.
  --theme    Name or path of the CSS theme you want to use.
  --compat   Will include v3 brush compatibility feature.
             See http://bit.ly/1KCaUq6 for complete details.
  --output   Output folder for dist files.
    [default: ".../syntaxhighlighter/dist"]
  --help     Show help

Available brushes are "all" or applescript, as3, base, bash, coldfusion, cpp,
csharp, css, delphi, diff, erlang, groovy, haxe, java, javafx, javascript, perl,
php, plain, powershell, python, ruby, sass, scala, sql, swift, tap, typescript,
vb, xml.
```

You may also pass paths to brush JavaScript files and theme SASS files.

#### --brushes

`--brushes` takes a comma separated list of brushes. Here's how brush resolution works:

```
--brushes=X

1. IF X = `all`
  a. BUNDLE repos/brush-*/brush.js
  b. USE_SAMPLE repos/brush-*/sample.txt
  c. STOP

2. IF EXISTS repos/brush-X/brush.js
  a. BUNDLE repos/brush-X/brush.js
  b. USE_SAMPLE repos/brush-X/sample.txt
  c. STOP

3. IF EXISTS X
  a. BUNDLE X
  b. USE_SAMPLE DIR(X) + `/sample.txt`
  c. STOP
```

Examples:

```
--brushes=all
--brushes=css
--brushes=css,javascript
--brushes=./my-brush.js
--brushes=/full/path/to/my-brush.js
--brushes=/full/path/to/my-brush.js,css,javascript
```

List of [supported brushes](https://github.com/syntaxhighlighter?utf8=%E2%9C%93&q=brush)

#### --theme

`--theme` takes a single theme name. Here's how theme resolution works:

```
--theme=X

1. IF EXISTS repos/theme-X/theme.scss
  a. BUNDLE repos/theme-X/theme.scss
  b. STOP

1. IF EXISTS X
  a. BUNDLE X
  b. STOP
```

Examples:

```
--theme=default
--theme=django
--theme=./my-theme.scss
--theme=/full/path/to/my-theme.scss
```

List of [supported themes](https://github.com/syntaxhighlighter?utf8=%E2%9C%93&q=theme)

#### --output

By default all build files are places into the `./dist` folder. You can change that by supplying this option.

## --compat

Specifying this flag will make SyntaxHighlighter v4 work with all existing v3 brushes out of the box, without bundling. See [[Migration Guide]] for more details

### Usage

To get SyntaxHighlighter to work on you page, you need to do the following:

1. Follow above building instructions to assemble your own `syntaxhighlighter.js`
2. Reference the script on your page using a `<script src="syntaxhighlighter.js" />` tag
3. Optionally reference the theme css on your page using a `<link rel="stylesheet" href="theme.css">` tag

#### `<pre />` method

##### Advantages

Works everywhere, graceful fallback if there are script problems, shows up in all RSS readers as regular `<pre />`

##### Problems

Major issue with this method is that all right angle brackets **must be HTML escaped**, eg all `<` must be replaced with `&lt;` This will ensure correct rendering.

SyntaxHighlighter looks for `<pre />` tags which have a specially formatted `class` attribute. The format of the attribute is the same as the CSS `style` attribute. The only required parameter is `brush`.

Here’s an example:

```html
<script type="text/javascript" src="syntaxhighlighter.js"></script>

<pre class="brush: js">
function foo()
{
}
</pre>
```

#### `<script />` method

The benefit of this method is ability to place virtually anything inside the CDATA **without having to escape anything***, so this allows for a straight 'cut and paste' experience from your favorite text editor.

##### Advantages

Doesn’t require escaping of the right angle bracket.

##### Problems

1.  No fallback, `<script />` tag is stripped out by most RSS readers, so if you are using SyntaxHighlighter on a blog, you are better off with the `<pre />` method.
2.  If you include a closing script tag, eg `</script>`, even inside CDATA block, most browsers will incorrectly close `<script type="text/syntaxhighlighter">` tag prematurely.

SyntaxHighlighter looks for `<script type="syntaxhighlighter" />` which have a specially formatted `class` attribute. The format of the attribute is the same as the CSS `style` attribute. The only required parameter is `brush`.

Here’s an example (**note thw required CDATA tag**):

```html
<script type="text/javascript" src="syntaxhighlighter.js"></script>

<script type="text/syntaxhighlighter" class="brush: js"><![CDATA[
  function foo()
  {
      if (counter <= 10)
          return;
      // it works!
  }
]]></script>
```

## License
This project is licensed under the terms of the MIT license.

[Migration Guide]: https://github.com/syntaxhighlighter/syntaxhighlighter/wiki/Migration-Guide
