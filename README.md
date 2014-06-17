# Dindent

[![Build Status](https://travis-ci.org/gajus/dindent.png?branch=master)](https://travis-ci.org/gajus/dindent)
[![Coverage Status](https://coveralls.io/repos/gajus/dindent/badge.png?branch=master)](https://coveralls.io/r/gajus/dindent?branch=master)
[![Latest Stable Version](https://poser.pugx.org/gajus/dindent/version.png)](https://packagist.org/packages/gajus/dindent)
[![License](https://poser.pugx.org/gajus/dindent/license.png)](https://packagist.org/packages/gajus/dindent)

Dindent (aka., "HTML beautifier") will indent HTML for development and testing. Dedicated for those who suffer from reading a template engine produced markup.

Try it in the [sandbox](http://gajus.com/dindent/sandbox/).

## Abuse Case

Dindent will not sanitise or otherwise manipulate your output beyond indentation.

If you are looking to remove malicious code or make sure that your document is standards compliant, consider the following alternatives:

* [HTML Purifier](https://github.com/Exercise/HTMLPurifierBundle)
* [DOMDocument::$formatOutput](http://www.php.net/manual/en/class.domdocument.php)
* [Tidy](http://www.php.net/manual/en/book.tidy.php)

If you need to indent your code in the development environment, beware that earlier mentioned libraries will attempt to fix your markup (that's their primary purpose; indentation is a by-product).

## Regex

There is a [good reason not to use regular expression to parse HTML](http://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags/1732454#1732454). However, DOM parser will rebuild the whole HTML document. It will add missing tags, close open block tags, or remove anything that's not a valid HTML. This is what Tidy does, DOM, etc. This behavior is undesirable when debugging HTML output. Regex based parser will not rebuild the document. Dindent will only add indentation, without otherwise affecting the markup.

The above is also the reason why [Chrome DevTools](https://developers.google.com/chrome-developer-tools/) is not a direct replacement for Dindent.

## Use

`Indenter` implements a single method `indent`:

```php
$indenter = new \Gajus\Dindent\Indenter();
$indenter->indent('[..]');
```

In the above example, `[..]` is a placeholder for:

```html
<!DOCTYPE html>
<html>
<head></head>
<body>
    <script>
    console.log('te> <st');
    function () {
        test; <!-- <a> -->
    }
    </script>
    <div>
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
    <div><table border="1" style="background-color: red;"><tr><td>A cell    test!</td>
<td colspan="2" rowspan="2"><table border="1" style="background-color: green;"><tr> <td>Cell</td><td colspan="2" rowspan="2"></td></tr><tr>
        <td><input><input><input></td></tr><tr><td>Cell</td><td>Cell</td><td>Ce
            ll</td></tr></table></td></tr><tr><td>Test <span>Ce       ll</span></td></tr><tr><td>Cell</td><td>Cell</td><td>Cell</td></tr></table></div></div>
</body>
</html>
```

Dindent will convert it to:

```HTML
<!DOCTYPE html>
<html>
    <head></head>
    <body>
        <script>
    console.log('te> <st');
    function () {
        test; <!-- <a> -->
    }
    </script>
        <div>
            <script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
            <div>
                <table border="1" style="background-color: red;">
                    <tr>
                        <td>A cell test!</td>
                        <td colspan="2" rowspan="2">
                            <table border="1" style="background-color: green;">
                                <tr>
                                    <td>Cell</td>
                                    <td colspan="2" rowspan="2"></td>
                                </tr>
                                <tr>
                                    <td>
                                        <input>
                                        <input>
                                        <input>
                                    </td>
                                </tr>
                                <tr>
                                    <td>Cell</td>
                                    <td>Cell</td>
                                    <td>Ce ll</td>
                                </tr>
                            </table>
                        </td>
                    </tr>
                    <tr>
                        <td>Test <span>Ce ll</span></td>
                    </tr>
                    <tr>
                        <td>Cell</td>
                        <td>Cell</td>
                        <td>Cell</td>
                    </tr>
                </table>
            </div>
        </div>
    </body>
</html>
```

## Options

`Parser` constructor accepts the following options that control indentation:

|Name|Description|
|---|---|
|`indentation_character`|Character(s) used for indentation. Defaults to 4 whitespace characters.|

# CLI

Dindent can be used via the CLI script `./bin/dindent.php`.

|Name|Description|
|---|---|
|`input`|Input file.|
|`output`|(optional) Output file. Defaults to the STDOUT.|
|`indentation_character`|(optional) Character(s) used for indentation. Defaults to 4 whitespace characters.|

## Known issues

* Does not treat comments nicely and IE conditional blocks.

## Installation

The recommended way to use Dindent is through [Composer](https://getcomposer.org/).

```json
{
    "require": {
        "gajus/dindent": "2.0.*"
    }
}
```
