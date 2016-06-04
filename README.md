# Minestack Documentation

Documentation for Minestack

![ReadTheDocs Badge](https://readthedocs.org/projects/minestack/badge/?version=latest)

[Read The Docs](http://minestack.readthedocs.io/en/latest/)

## Viewing the documentation locally

```
git clone https://github.com/minestack/docs.git
cd docs
pip install -r requirements.txt
make livehtml
```

## Developing the documentation

```
git clone https://github.com/minestack/docs.git
cd docs
pip install -r requirements.txt
sphinx-autobuild . _build/html
```