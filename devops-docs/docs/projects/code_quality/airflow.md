# Ruff Linter on Apache Airflow

In my organization, we use Apache Airflow for ETL jobs and run multiple instances of Airflow per tenant. To maintain consistency in the code of Airflow DAGs across tenants, we decided to enforce this via CI. To further assist developers, we integrated pre-commit hooks. Here, I will describe how I added CI and pre-commit hooks to enhance the code quality of Airflow DAGs and common packages.

## Set Up the Local Environment

The following steps can be applied to a new or existing git repository.

First, create a virtual environment:

    python3 -m venv .env
    source .env/bin/activate

Next, create a `dev-requirements.txt` file with the following packages and specific versions to avoid compatibility issues:

    pre-commit==3.7.1
    ruff==0.4.6

* `ruff`: A Rust-based, lightning-fast linter and formatter.
* `pre-commit`: A Python package that allows you to hook Ruff, which will run before commits are applied.

Install the requirements:

    pip3 install -r dev-requirements.txt 

Create a `.pre-commit-config.yaml` file with our hooks (for now, Ruff):

    repos:
    - repo: https://github.com/astral-sh/ruff-pre-commit
      rev: v0.4.6
      hooks:
        - id: ruff
    #      args: [ --fix ]  # Uncomment to enable auto-fixing
        - id: ruff-format

You can remove the comment from the `args` if you want any violated rules to be auto-fixed. Remember to always review the code!

Now, install the pre-commit hooks:

    pre-commit install

To modify the default Ruff settings, create a `ruff.toml` file:

    [lint]
    ignore = ["E402"]  # E402: Module level import not at top of file (adjust based on project needs)
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

Here, we selected to ignore a certain rule and added a set of linters. You can also run `ruff check .` to lint the entire repository. Additionally, you can run `ruff rule E402` to get more information on the excluded rule.

### Scalability and Feedback

As the project scales, consider implementing more comprehensive CI/CD pipelines that include testing, security checks, and performance metrics alongside linting. Implement a feedback mechanism within the CI pipeline to provide developers with detailed reports on linting issues and how to fix them, enhancing the overall development experience.

By following these steps, you can maintain high code quality in your Airflow DAGs, ensuring consistency and reliability across your organization's projects.
