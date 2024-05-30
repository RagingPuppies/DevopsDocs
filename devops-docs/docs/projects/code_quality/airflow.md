# Ruff linter on Apache Airflow
In my organization, we use Apache Airflow for ETL jobs and run multiple instances of Airflow per tenant. To maintain consistency in the code of Airflow DAGs across tenants, we decided to enforce this via CI. To further assist developers, we integrated pre-commit hooks. Here, I will describe how I added CI and pre-commit hooks to enhance the code quality of Airflow DAGs and common packages.

## Set up The local environemnt
The below can either be installed on a new git repo, or an old one...

First step is to create a virtual environment:

    python3 -m venv .env
    source .env/bin/activate

Now let's create `dev-requirements.txt` file with the following packages:

    pre-commit
    ruff

* ruff - a rust based lightning fast linter and formatter
* pre-commit - python package that let you hook ruff, this will run before the commit applied.

Let's install the requirements:

    pip3 install -r dev-requirements.txt 

Now we can create `.pre-commit-config.yaml` file with our hooks (for now, ruff):

    repos:
    - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.4.6
    hooks:
        - id: ruff
    #      args: [ --fix ]
        - id: ruff-format

You can remove the comment from the `args` if you want any violated rule to get auto-fixed, Rembmer to always review the code!

Now we can install the pre-commit hooks:

    pre-commit install

We can also modify the default ruff settings, create `ruff.toml`:

    [lint]
    ignore = ["E402"]
    select = [
        # pycodestyle
        "E",
        # Pyflakes
        "F",
        # pyupgrade
        "UP",
        # flake8-bugbear
        "B",
        # flake8-simplify
        "SIM",
        # isort
        "I",
    ]

Here, we selected to ignore a certain rule, and added a bunch of linters, you can also run `ruff check .` to run on the whole repository.
Another thing you can do is run `ruff rule E402` to get some information on the rule we excluded.