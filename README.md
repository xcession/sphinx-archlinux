# Arch Linux Documentation
Arch Linux Documentation built using Sphinx and reStructuredText.

## Prerequisites

* Python + Pip
* [Sphinx](https://www.sphinx-doc.org/en/master/usage/installation.html)
* [Read the Docs Sphinx Theme](https://github.com/readthedocs/sphinx_rtd_theme)
* *(Optional)* [sphinx-autobuild](https://github.com/executablebooks/sphinx-autobuild)

## Getting Started
These instructions will get you a copy of the project up and running on your local machine for development and testing purpose.

* Clone the repo:

```
$ git clone https://github.com/xcession/sphinx-archlinux.git
$ cd sphinx-archlinux
```

* To compile HTML document:

```
$ make html
```

* To compile HTML document using `sphinx-autobuild` (For auto rebuild and reload feature)

```
$ sphinx-autobuild -b html . _build/html
```

## Links
* [Installing Sphinx](https://www.sphinx-doc.org/en/master/usage/installation.html)
* [reStructuredText](https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html)

## License
[MIT](/LICENSE)
