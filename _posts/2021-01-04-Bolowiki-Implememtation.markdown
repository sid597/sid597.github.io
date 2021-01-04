---
layout: post
# categories: Flask, web applications
title: Implementing bolowiki
data: 2021-04-01
---

## How to start ?
 

 - Install python
 - Learn best practices on how to installing python packages 
   
   Official package manager for python is called [PyPi][pypi]. Installing a package from this manager is easy because it comes with a tool called ```pip``` (python3 comes with this tool by default, for python2 you have to install it manually). 
   
   How to use ```pip``` ?
   First we need to understand 
    To install a package just use :

   ``` pip install some-package-name``` 
   When there are packages there are different versions of that package, and different projects might need different versions. Python's solution to this issue is <span style="color:green">virtual</span> environment (python sandboxes) where you can install packages needed for current project without affecting the global installation.

   How to make a vitual environment and use it ?

   For python3 use ```python3 -m venv venv``` where first venv is name of python's virtual environment pacakage and second one is the name of your environment you are creating, for python2 install virtualenv and do ```virtualenv venv```. Now tell the system to use that environment ```source venv/venv/activate```, once inside the environment you can install packages as a normal user.
- Install Flask ```pip install flask```
- Create a ```__init__.py``` file and write your first minimal application :
  ```
  from flask import Flask
  app = Flask(__name__)
  
  @app.route('/')
  def hello_world():
      return 'Hello, World!'
    ```

- Tell your terminal the application to work with by exporting ```FLASK_APP``` environment variable. 
-- Do ```flask run`` and your application is running.

   


[pypi]: https://pypi.org/