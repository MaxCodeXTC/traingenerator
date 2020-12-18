<h1 align="center">
    traingenerator
</h1>

<p align="center">
    <strong>🧙&nbsp; A web app to generate template code for machine learning</strong>
</p>

<p align="center">
    <a href="https://www.buymeacoffee.com/jrieke"><img src="https://img.shields.io/badge/Buy%20me%20a-coffee-orange.svg?logo=buy-me-a-coffee&logoColor=orange"></a>
    <a href="LICENSE"><img src="https://img.shields.io/github/license/jrieke/traingenerator.svg"></a>
    <a href="https://github.com/psf/black"><img src="https://img.shields.io/badge/code%20style-black-000000.svg" alt="Code style: black"></a>
</p>

<br>

<p align="center">
    <img src="docs/assets/demo.gif" width=600>
</p>

<br>

<h3 align="center">
    🎉 traingenerator is now live! 🎉
    <br><br>
    Try it out: <br>
    <a href="https://traingenerator.jrieke.com">https://traingenerator.jrieke.com</a>
</h3>

<!--
<p align="center"><strong>
    Try it out: <br>
    >>> <a href="https://traingenerator.jrieke.com">https://traingenerator.jrieke.com</a> <<<</strong>
</p>
-->

<br>

Generate custom template code for PyTorch & sklearn, using a simple web UI built with [streamlit](https://www.streamlit.io/). traingenerator offers multiple options for preprocessing, model setup, training, and visualization (using Tensorboard or comet.ml). It exports to .py, Jupyter Notebook, or  [Google Colab](https://colab.research.google.com/). The perfect tool to jumpstart your next machine learning project! ✨

<br>

---

<br>

**Note: The steps below are only required if you want to contribute to traingenerator or run it locally.**

## Adding new templates

You can add your own template in 4 easy steps:

1. **Create a folder under `./templates`.** 
The folder name should be the task that your template solves (e.g. 
`Image classification`). Optionally, you can add a framework name (e.g. 
`Image classification_PyTorch`). Both names are automatically shown in the first two 
dropdowns in the sidebar. 
✨ *Tip: Copy the [example template](templates/example) to get started more quickly.* 
2. **Add a file `sidebar.py` to the folder.** 
It needs to contain a method `show()`, which displays all template-specific streamlit 
components in the sidebar (i.e. everything below *Task*) and returns a dictionary of 
user inputs. [See example](templates/example/sidebar.py).
3. **Add a file `code-template.py.jinja` to the folder.** 
This [Jinja2 template](https://jinja.palletsprojects.com/en/2.11.x/templates/) is used 
to generate the code. You can write normal Python code in it and modify it 
(through Jinja) based on the user inputs in the sidebar (e.g. insert a parameter 
value from the sidebar or show different code parts based on the user's selection). 
[See example](templates/example/code-template.py.jinja).
4. **Optional: Add a file `test-inputs.yml` to the folder.** 
This simple YAML file should define a few possible user inputs that can be used for 
testing. If you run pytest (see below), it will automatically pick up this file, render 
the code template with its values, and check that the generated code runs without 
errors. This file is optional – but it's required if you want to contribute your 
template to this repo. [See example](templates/example/test-inputs.yml).

That's it! 🎈 You don't have to change any code in the app itself. Your new template 
will be automatically discovered by traingenerator and shown in the sidebar. 

**Want to share your magic?** 🧙 PRs with new templates are welcome! Please have a look 
at [CONTRIBUTING.md](CONTRIBUTING.md). 


## Installation

```bash
git clone https://github.com/jrieke/traingenerator.git
cd traingenerator
pip install -r requirements.txt
```

*Optional: For the "Open in Colab" button to work you need to set up a Github repo 
where the notebook files can be stored (Colab can only open public files if 
they are on Github). After setting up the repo, create a file `.env` with content:*

```bash
GITHUB_TOKEN=<your-github-access-token>
REPO_NAME=<user/notebooks-repo>
```

*If you don't set this up, the app will still work but the "Open in Colab" button 
will only show an error message.*


## Running locally

```bash
streamlit run app/main.py
```

Make sure to run always from the `traingenerator` dir (not from the `app` dir), 
otherwise the app will not be able to find the templates.

## Deploying to Heroku

First, [install heroku and login](https://devcenter.heroku.com/articles/getting-started-with-python#set-up). 
To create a new deployment, run inside `traingenerator`:

```
heroku create
git push heroku main
heroku open
```

To update the deployed app, commit your changes and run:

```
git push heroku main
```

*Optional: If you set up a Github repo to enable the "Open in Colab" button (see above),
you also need to run:*

```
heroku config:set GITHUB_TOKEN=<your-github-access-token>
heroku config:set REPO_NAME=<user/notebooks-repo>
```

## Testing

First, install pytest and required plugins via:

```bash
pip install -r requirements-dev.txt
```

To run all tests: 

```bash
pytest ./tests
```

Note that this only tests the code templates (i.e. it renders them with different 
input values and makes sure that the code executes without error). The streamlit app 
itself is not tested at the moment.

You can also test an individual template by passing the name of the template dir to 
`--template`, e.g.:

```bash
pytest ./tests --template "Image classification_scikit-learn"
```
