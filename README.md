# Arch Linux Documentation

Arch Linux Documentation built using Sphinx and reStructuredText.

## Dependencies

* [Python 3](https://www.python.org)
* [GNU Make](https://www.gnu.org/software/make)
* [Sphinx](https://www.sphinx-doc.org/en/master/usage/installation.html)
* [Read the Docs Sphinx Theme](https://sphinx-rtd-theme.readthedocs.io)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purpose.

Clone the repo:

```
$ git clone https://github.com/xcession/sphinx-archlinux.git
$ cd sphinx-archlinux
```

To compile HTML document:

```
$ sphinx-build -b html . _build/html
```

Or simply using `Makefile`:

```
$ make html
```

### Sphinx AutoBuild

Compile document using `sphinx-autobuild` (For auto rebuild and reload feature).

Install `sphinx-autobuild`:

```
$ pip install sphinx-autobuild
```

To compile HTML document:

```
$ sphinx-autobuild -b html . _build/html
```

## Translation

To translate the document. first, you'll need to install `sphinx-intl`.

```
$ pip install sphinx-intl
```

Generate `.pot` files that contain unique "source strings" (`msgid`) for translation:

```
$ sphinx-build -b gettext . _build/gettext
```

Or using `Makefile`:

```
$ make gettext
```

### Add new locale

You can add new locale by parsing `-l <locale>` argument to the `sphinx-intl` command.

For example:

```
$ sphinx-intl update -p _build/gettext -l es
```

The above command will generate "Spanish" locale in `./locale` directory (As specified in `conf.py`). Which is contain `.po` files ready for translation.

### Compile Specific Locale

By default, the document will compile using `en` locale (Also specified in `conf.py`).

To compile document using a different locale. Just parse `-D language=<locale>` argument:

```
$ sphinx-build -b html . _build/html -D language=th_TH
```

Using `sphinx-autobuild`:

```
$ sphinx-autobuild -b html . _build/html -D language=th_TH
```

## Links
* [Installing Sphinx](https://www.sphinx-doc.org/en/master/usage/installation.html)
* [reStructuredText](https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html)
* [sphinx-autobuild](https://github.com/executablebooks/sphinx-autobuild)
* [Sphinx Configuration - Language](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language)

## License
[MIT](/LICENSE)
