# Introduction


In this practice lab, we are going to create a minimal RESTful API using the Flask framework. Our application will let the user get a list of software testing types or add a new type.

# Preparing Our Application

Start by going to the lab environment
[Katacoda](https://katacoda.com/courses/ubuntu/playground)


We are going to use:

- Python 3.5
- Python Virtualenv to create an isolated environment for our application

This is the structure of our project folder:

```
app/
├── app.py
└── __init__.py
__init__.py
.gitignore
```

You can also download the .gitignore file from [this repository](https://github.com/github/gitignore/blob/master/Python.gitignore).

Let's start by creating the virtual environment for our Flask application.

```bash
mkdir rest-api
python3 -m venv rest-api
```

If everything is fine, let's activate it:

```bash
. rest-api/bin/activate
```

In order to use Flask, you need to install it from the Python Package Index using PIP:

```bash
pip install flask
```

Let's create our application files:

```bash
mkdir project
wget -O .gitignore "https://github.com/github/gitignore/blob/master/Python.gitignore" 2> templog
touch __init__.py
mkdir app
touch app/app.py
touch app/__init__.py
```

Paste this code inside the app.py file:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

testing_types = [
  { 'name': 'unit testing', 'description': 'testing individual units of source code' }
]


@app.route('/tests')
def get_tests():
  return jsonify(testing_types)


@app.route('/tests', methods=['POST'])
def add_test():
  testing_types.append(request.get_json())
  return '', 204



if __name__ == '__main__':
    app.run(debug=True)

```

The above code has two functions:
- get_tests : Used to get the list of tests from the testing\_types dictionary
- add_tests : Used to add a test to the testing\_types dictionary

Our initial dictionary contains one test type:

```python
testing_types = [
  { 'name': 'unit testing', 'description': 'testing individual units of source code' }
]
```

In order to run our API, type:

```bash
python app/app.py
```

You should see something similar to the following output:

```bash
python app/app.py
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 179-596-761
```

Our web server runs on: http://127.0.0.1:5000/ and in order to test it we can use a curl request.  Open a new terminal in Katacoda by clicking the `+` and choosing "Open New Terminal"

Run the following in the new terminal to get the testing types list:

```bash
curl -X GET  http://127.0.0.1:5000/tests
```

At this stage, it should return:

```json
[
  {
    "description": "testing individual units of source code",
    "name": "unit testing"
  }
]
```

Let's try adding another type of test:

```bash
curl -X POST -H "Content-Type: application/json" -d '{
  "name": "load testing",
  "description": "checking if a software can handle the expected load"
}' http://localhost:5000/tests
```

After executing the last POST request, we can verify using another GET:

```bash
curl -X GET  http://127.0.0.1:5000/tests
```

We should see the two types of tests:

```json
[
  {
    "description": "testing individual units of source code",
    "name": "unit testing"
  },
  {
    "description": "checking if a software can handle the expected load",
    "name": "load testing"
  }
]
```

## Bonus lab
Now using what you learned about Docker, containerize the rest-api Python application.

# Conclusion

In this practice lab, we created a RESTful API and, optionally, containerized it.
