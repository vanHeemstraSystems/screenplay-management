# Documentation

Read the Docs: [REPOSITORY-NAME](https://vanheemstrasystems-REPOSITORY-NAME.readthedocs.io/en/latest/)

## 100 - Install ReadTheDocs

Run the following command:

```
$ pip install sphinx sphinx_rtd_theme recommonmark
```

Next to create the docs run:

```
$ cd docs
$ make html
```

Your docs will be created inside ```docs/build```.

## 200 - Autogeneration of ReadTheDocs

1) Make sure there is a ```.readthedocs.yml``` file at the roor of your GitHub repository.

2) Connect your GitHub repository to ReadTheDocs at https://readthedocs.org/

3) Trigger a build on ReadTheDocs.