---
title: "Learning about Test-Driven Development in Python"
layout: inner
post_link: 'nicholas-ko-zy.github.io/blog/python_tdd.html'
publish_date: 2025-02-22
---

<!-- omit in toc -->
# Learning about Test-Driven Development in Python 
 
Recently, I hopped onto this video tutorial: ["TDD Full Course (Learn Test Driven Development with Python)"](https://www.youtube.com/watch?v=eAPmXQ0dC7Q) by Wes Doyle.

In the video, he teaches a few principles of test-driven development while building a simple web app to do Named Entity Recognition. Named Entity Recognition is a natural language processing (NLP) task that classifies words into some abstract categories like Bill Evans $\rightarrow$ Person and 1968 $\rightarrow$ Date. 'Bill Evans' and '1968' would be our entities. 'Person' and 'Date' would be the types/categories that the NLP model outputs.

Wes Doyle used the open source [`en_core_web_sm` Named Entity Recognition model from `spaCy` for his web app.](https://spacy.io/models/en) **All credit goes to Wes Doyle for writing the code. I merely forked his repo and fixed a few lines of code due to upgrades in the dependencies.**

![Web App](/img/blog/python_tdd/bill_evans_ner.gif)
*A little demo gif.*

To run my fork of Wes Doyle's project on your own PC: [Link Here](https://github.com/nicholas-ko-zy/flask-ner)

1. Create a new virtual environment and activate it. You could do run something like this in your terminal:
   
    ```
    python -m venv ./flask-ner
    source flask-ner/bin/activate
    ```

2. Run the requirement text file in your terminal.
   
    ```
    pip install -r requirements.txt
    ```

3. Run this in your terminal to install the `flaskner` module. (see the `setup.py` for details) 
   
    ```
    pip install .
    ```

4. Download the `spaCy` Named Entity Recognition model. Run this in your terminal: 
   
    ```
    python -m spacy download en_core_web_sm
    ```


# What I learnt...

I've written a hodgepodge of lesson learnt as I followed along the tutorial. It's for my future reference and also to help anyone who might run into similar problems in this tutorial too. 🐾


- [What I learnt...](#what-i-learnt)
  - [What I learnt: 3 Rules of Test-Driven Development](#what-i-learnt-3-rules-of-test-driven-development)
  - [What I learnt: Dependency Inversion Principle](#what-i-learnt-dependency-inversion-principle)
  - [What I learnt: Creating a mock for the Named Entity Client Model.](#what-i-learnt-creating-a-mock-for-the-named-entity-client-model)
  - [What I learnt: Terminal Command to run a single test.](#what-i-learnt-terminal-command-to-run-a-single-test)
  - [What I learnt: Another way to run a single test.](#what-i-learnt-another-way-to-run-a-single-test)
  - [What I learnt: You have to run app.py before running your `test_index_e2e.py` file](#what-i-learnt-you-have-to-run-apppy-before-running-your-test_index_e2epy-file)
  - [What I learnt: Automatically installing the packages used in the project](#what-i-learnt-automatically-installing-the-packages-used-in-the-project)
  - [What I learnt: I git cloned Wes Doyle's project from Github but it didn't work out of the box for me.](#what-i-learnt-i-git-cloned-wes-doyles-project-from-github-but-it-didnt-work-out-of-the-box-for-me)
  - [What I learnt: Summary of test types done](#what-i-learnt-summary-of-test-types-done)
  - [Concluding Remarks](#concluding-remarks)


## What I learnt: 3 Rules of Test-Driven Development

1. Write a failing test first.

2. Write just enough tests to fail.

3. Write just enough code to pass.

To run a set of tests, run this in your terminal:

```
python3 -m pytest
```

Note to self: Read up about SOLID principles for Object Oriented Programming.

## What I learnt: Dependency Inversion Principle

[(Timestamp: 22:59)](https://youtu.be/eAPmXQ0dC7Q?si=sXnDUc8BLDbDXPLh&t=1379)
At this time, Doyle shows a live coding example of the "Dependency Inversion Principle". It's the 'D' in SOLID Principles of object oriented programming. The principle is about making sure classes depend on abstractions not concretes. Here's an example using the `ner_client.py` file from the tutorial.

```python
# Before
import spacy

class NamedEntityClient:
    def __init__(self):
        # Initialise ner instance with a SpaCy model, as specified below
        self.model = spacy.load("en_core_web_sm")

    def get_ents(self, sentence):
        doc = self.model(sentence)
        return {}
```

Issue: Here, there is a tight coupling of the `spaCy` language model and the `NamedEntityClient` class. Each time you create an instance of `NamedEntityClient`, you're also loading the language model into memory. Now this is not flexible, because what if you want to load another language model in the future? Or you may think it's kinda pointless to load a spacy model just to test for the existence of `NamedEntityClient`.

Fix: What does it look like to exercise the 'Dependency Inversion Principle' here? Instead of having `NamedEntityClient` depend on some detail like `"en_core_web_sm"`, it should depend on an abstraction that gets passed to it. Here's what the code looks like after we modify it. (see chunk below)

```python
# After
import spacy

class NamedEntityClient:
    def __init__(self, model):
        # Initialise ner instance with a spaCy model, as specified below
        self.model = model

    def get_ents(self, sentence):
        doc = self.model(sentence)
        return {}
```

The difference here is that we still get to preserve the functionality of the `get_ents` method. Meaning, you can still pass a sentence to the model to generate some output. But now, instead of instantiating the specific model you want to use when you create a fresh instance of `NamedEntityClient`, you sort of push the responsibility of specifying your model to a 'higher' level of abstraction. You do this by adding a `model` argument to the initialisation method. Of course, this decision will influence the way your test cases are written. The test case will now need a placeholder `model` variable after we've abstracted `model`. This brings us to our next lesson on using 'mocks' in tests.

## What I learnt: Creating a mock for the Named Entity Client Model.

[(Timestamp 25:36)](https://youtu.be/eAPmXQ0dC7Q?si=vmfhnsVKDj1iCHdG&t=1531)

```python
class TestNerClient(unittest.TestCase):

    def test_get_ents_returns_dictionary_given_empty_string(self):
        # Create a dummy NER model 
        model = NerModelTestDouble('eng') # <- Change was made here
        # Create a NER instance
        ner = NamedEntityClient(model)
        # Assign the entity a variable name "ent"
        ents = ner.get_ents("")
        # Check if the python type is a 'dictionary'
        self.assertIsInstance(ents, dict)
#...
```
Now that we've abstracted the actual NER model, we want something that behaves like the `spaCy` model to use in our tests. So, we create a mock of the `spaCy` model with just enough behaviours to mimic the actual `spaCy` model.  

Since we are using a mock in our test, we need to create a class to represent the mock. We do that in the file `test_doubles.py`. Our mock can't be completely made up since it's supposed to have the subset (not necessarily proper subset) of behaviours that the actual `spaCy` model has. One thing to check out is what kind of attributes the model output gives. In our case, the model output has type `<class 'spacy.tokens.doc.Doc'>` and has an attribute (no parentheses) `ents`. Now let's look at `ents`. We observe that `ents` has two attributes - `text` and `label` respectively. See screenshot below.

![types and attributes of model](/img/blog/python_tdd/ent_attributes.png)

With this knowledge, we can now create a mock model that has an `ents` attribute and a mock output with attributes `text` and `label`.

```python
class NerModelTestDouble:
    """
    Test double for spaCy NLP model
    """
    def __init__(self, model):
        self.model = model

    def returns_doc_ents(self, ents):
        # Specify what entity your model returns in the argument of this 
        # method. i.e. for an empty string input, you can specify an
        # empty list as the ents
        self.ents = ents

    def __call__(self, sent):
        return DocTestDouble(sent, self.ents)


class DocTestDouble:
    """
    Test double for docs, which is the variable assigned to the
    output of the nlp model as in: 
        doc = nlp("string")
    """
    def __init__(self, sent, ents):
        # A Span is the data structure of
        # [('Madison', 'PERSON'), ('Wisconsin', 'GPE')]
        # In our mockup, we use list comprehension to reverse engineer
        # [(ent.text, ent.label_) for ent in doc.ents]
        self.ents = [SpanTestDouble(ent['text'], ent['label_']) for ent in ents]
```

Note to self: Check out ways of automating mocks in `pytest-mock` and `monkey-patch`.

## What I learnt: Terminal Command to run a single test.

```
pytest test/test_ner_client.py::TestNerClient::test_get_ents_returns_dictionary_given_empty_string_causes_empty_spacy_doc_ents 
```
^ Hint: You can use tab completion along the way. Remember to use the double-colons `::` after the `.py` file. First set of `::` specifies the test class name and the second one to specifies the test name.

## What I learnt: Another way to run a single test.

Find a unique substring in the name of your test to replace `"your_test_name"` and run this in your terminal.
```
pytest -k "your_test_name"
```

## What I learnt: You have to run app.py before running your `test_index_e2e.py` file
Run this to get the flask app running before running the end-to-end test.

```
python3 app.py
```

## What I learnt: Automatically installing the packages used in the project

If the project doesn't have a requirements file you can...

1. Install package `pipreqs` to generate pip requirements.
   
    ```
    pip install pipreqs
    ```

2. Run this command in your terminal to scan through your project to look for packages. It outputs the `requirements.txt` file in your project directory.
   
    ```
    pipreqs --ignore bin,etc,include,lib,lib64
    ```

3. Install the required packages using `requirements.txt` file.
   
    ```
    pip install -r requirements.txt
    ```

## What I learnt: I git cloned Wes Doyle's project from Github but it didn't work out of the box for me.

I asked ChatGPT for help, here's what I've found

- I had to create a new environment and check that I have all the
packages in needed installed. To do so, follow the lessons above.
- After that I needed to run `pip install -e .` in my terminal.
- I tried running `app.py` and found the front-end was not working. ChatGPT's advice was to check the console in the web browser which was hosting the app.

This was what I saw:

![frontend not updating error](/img/blog/python_tdd//flask_ner_front_end_not_updating_console_log.png)

I found the fix from this [Stack Overflow post](https://stackoverflow.com/questions/28461001/python-flask-cors-issue). You need the add the following lines of code into your `app.py` file.

- `pip install flask-cors`
- import `CORS` from `flask_cors`
- Initialise the Cross Origin Resource sharing for the flask app with `CORS(app)`

```python
from flask_cors import CORS  # Import CORS
.
.
.
app = Flask(__name__)
CORS(app)
ner = spacy.load("en_core_web_sm")
ner = NamedEntityClient(ner, displacy)
```
It worked for me after that.

## What I learnt: Summary of test types done
- Unit test
  
- End-toEnd (e2e) browser automation test
  
- Integration test on the API


Note to self: Things to explore next are

* Creating web apps using Flask/Django, with front and backend
  
* SOLID Principles for Object-Oriented Programming
  
* Creating/hosting a dynamic website (preferably for free)

## Concluding Remarks

Learning about test-driven development made me think of what I expect my functions to do before I start writing them. Hmm...I like it.





