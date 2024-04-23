Like [scripted actions](actions-scripts.md), reusable actions are logical units of analytic code.
However, whereas a scripted action is written to solve a problem for one study and must be copied-and-pasted to solve a similar problem for another study, a reusable action is written to solve a problem for several studies *without copying-and-pasting between them*.
This makes reusable actions ideal for tasks that must be completed by several studies, such as joining datasets or producing deciles charts.

## Running reusable actions

You can browse existing reusable actions at <https://actions.opensafely.org>.
Although each is different, they have a common API.
Consider the following extract from a study's *project.yaml*:

```yaml
actions:
  generate_dataset:
    run: ehrql:v1 generate-dataset analysis/dataset_definition.py --output output/dataset.csv.gz
    outputs:
      highly_sensitive:
        dataset: output/dataset.csv.gz

  run_a_reusable_action:
    # We will run version `v1.0.0` of the reusable action called `a_reusable_action`.
    # The reusable action accepts an argument; in this case, a path to a file.
    run: a_reusable_action:v1.0.0 output/dataset.csv.gz
    # The reusable action accepts a configuration option;
    # in this case, an output format.
    config:
      output-format: PNG
    needs: [generate_dataset]
    outputs:
      moderately_sensitive:
        output: output/output_from_a_reusable_action.png
```

In the above extract, the `run` and `config` properties of `run_a_reusable_action` are the common API.
The `run` property, which is required, describes how the reusable action is run.
It comprises the name and the version of the reusable action, and, optionally, one or more arguments.
The `config` property, which is optional, describes configuration options.

## Developing reusable actions

!!! note
    If you're thinking about developing a reusable action, then start by creating a new study within the [`opensafely`](https://github.com/opensafely) organisation that encapsulates the problem.
    As a minimum, the study should [extract](/ehrql/) and operate on a dataset:
    indeed, the code that operates on the dataset *is* the reusable action.

    At this point, you should [open an issue](https://github.com/opensafely-actions/.github/issues).
    Below, we describe how to convert the study into a reusable action.

A reusable action is a public repo within the [`opensafely-actions`](https://github.com/opensafely-actions) organisation.
It has a `main` branch, which is the release branch;
versions are marked with [tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging).
In the above extract from a study's *project.yaml*, for example, we will run version `v1.0.0` of the reusable action called `a_reusable_action`.

The repo has the following minimal structure:

```sh
.
├── README.md
└── action.yaml
```

*README.md* contains information about the reusable action, which is displayed by <https://actions.opensafely.org>.

*action.yaml* contains a run command, which is composed of a runtime image (either `python`, `r`, or `stata-mp`) and an entry point.
The entry point is a path to a script.
For example:

```yaml
run: python:v2 python action/cli.py
```

Where *action/cli.py* is:

```python
def main():
  print("A reusable action")

if __name__ == "__main__":
  main()
```

When developing a reusable action, just as when developing a scripted action, the action's dependencies are made available by the runtime; they are not made available by the action.

* The Python runtime is provided by [`python-docker`](https://github.com/opensafely-core/python-docker).
  Its dependencies are in [*requirements.txt*](https://github.com/opensafely-core/python-docker/blob/main/v1/requirements.txt).
  In practice, this means that a Python action's *requirements.txt* is ignored.
* The R runtime is provided by [`r-docker`](https://github.com/opensafely-core/r-docker).
  Its dependencies are in [*packages.csv*](https://github.com/opensafely-core/r-docker/blob/master/packages.csv).
* The Stata runtime is provided by [`stata-docker`](https://github.com/opensafely-core/stata-docker).
  Its dependencies are in [*libraries*](https://github.com/opensafely-core/stata-docker/tree/main/libraries).

---8<-- 'includes/glossary.md'
