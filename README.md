This is an example project to experiment with poetry update.

Let's assume this project was made in early 2023, and the dependencies in the lock file are now outdated:

- numpy 1.26.0 is a main dependency, and
- scipy 1.10.1 is in the optional dependency group called 'test'.

But they work together:


```
$ poetry install --with=test
  - Installing numpy (1.26.0)
  - Installing scipy (1.10.1)

$ pip check
No broken requirements found.

$ python -c "import scipy"
```

Now, almost 2 years later, the user wants to update, but forgot that any optional dependency groups are installed, and is left with a broken installation:

```
$ poetry update
  - Updating numpy (1.26.0 -> 2.1.3)

$ pip check
scipy 1.10.1 has requirement numpy<1.27.0,>=1.19.5, but you have numpy 2.1.3.

$ python -c "import scipy"
scipy/__init__.py:143: UserWarning: A NumPy version >=1.19.5 and <1.27.0 is required for this version of SciPy (detected version 2.1.3)
  warnings.warn(f"A NumPy version >={np_minversion} and <{np_maxversion}"
```

It is rather confusing that an `poetry update` can leave the environment in a broken state.
