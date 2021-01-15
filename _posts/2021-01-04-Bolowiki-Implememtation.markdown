---
layout: post
# categories: Flask, web applications
title: Implementing my website Bolowiki
data: 2021-04-01
---

Bolowiki is a website where one can convert wikipedia articles to natural-sounding speech. User can search either via voice or typing in multiple languages, for some wikipedia article and then convert that article to natural-sounding speech. For users who sign up the other benefit is that they can write something via voice or typing, and then translate that text to other language and then convert that translated text to natural-sounding speech in female or male voice. Bolowiki is made of 2 words "bolo" meaning speak and "wiki" short for wikipedia i.e speak wikipedia. 




### <b> Hacks </b>

  To search wikipedia articles I figured out the api that wikipedia uses by making a few requests on the wikipedia page itself, so I implemented the search bix and any query made is passed on to real wikipedia. So the way to pass the query to wikipedia can go through server or from frontend itself. If I decided to route this through backend then speed would be slow and other issue is, if enough requests are made from same IP then it could get banned. So the best solution to do this from client side only.
### <b> Why ? </b>

  - Technical reason:
    - Learn and implement how to make a website and host it on a machine
    - Learn how to use api's designed by others.
    - Learn about api's design and implementation, authentication, scraping data from HTML, orm (object relationl mapping)
    - Implement frontend without any big framework and in process figure out why do we need them.
    - Implement something different from what tutorials implement but in the same spirit i.e learn and implement without training wheels.

  - Non-technical reason:

    - Learn pronunciation.
    - Do homework while someone reads to you.
    - Bored of listening to music and want to learn something new.
    - Can be used as a learning tool for kids for eg. convert some story to audio and then ask kids to listen and repeat. With some additional work can make an app or website which listens to kids speak and match what they spoke with the correct pronunciation.

## <b><u>Frontend Implementation</u></b>

### <b> Design decisions </b>

   Why not implement your own full text search ?
   Wikipedia's data is huge and I don't have enough money as a learner to buy a server and put the data in then use full text search on it. 

### <b>What are the feature difference between logged in and non-logged in user</b>
  Different permissions for different class of users:
  - If user is not logged in then they can only search for wikipedia articles and convert only the introduction of any wikipedia page to speech.
  - If user is logged in then they can convert the article to speech and also use text translate and then convert that to speech. 
  
### <b> How does search work ? </b>
   <i> Note: I did not use a full text search service I just have a box and every query is passed to wikipedia search, the reasoning for this is covered in design decisions </i>

   - User inputs (a letter via typing and word if speaking) something in the search box.
   - On each input I make a request to wikipedia and show the result that comes back as suggestions.
   - User clicks on any result, presses enter or clicks search button.
   - A request is send to backend containing details: link to wikipedia article and the search language. 
  
  <b> How is search via speech implemented ?</b>

  <i> Note this feature is only implemented by chrome </i>


  This is implemented by using [Web Speech API][webSpeech], this API provides functionality for speech recognition and text to speech, I am using the speech recognition functionality here. Scenario:

  - The voice button is clicked.
  - Event handler calls `listenToVoiceQuery` method.
  - Create a new speech recognition instance.
    ``` const recognition = new webkitSpeechRecognition();``` 
  - Do some configuration like what language, need intermediate results [etc.][speechRecProp] 
  - Set up event handlers based on properties like what to do onstart, onresult, onend, onerror [etc.][speechRecEve]

  There are different types of errors like `no-speech`, `audio-capture`, `not-allowed` etc. and based on them one might show different results. One can also set animations, some message etc. when starting the recognition like a gif of listening.    
  - If some speech is recognised then the browser engine returns the result as text.

   
### <b> How does text translate work ? </b>
   
   - User selects from and to languages to traslate.
   - Starts giving the input.
   - On each input a request to backend is sent for text translate containing details : from language, to language, the text that needs to be translated.
   - After user is done with the translation they can choose gender voice and some name for the resulting file.
   - A request to backend is made containing data : voice gender, filename, text to convert to speech, language they want the voice in.



### How to start ?
 

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
[webspeech]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API/Using_the_Web_Speech_API
[speechRecProp]: https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition#Properties
[speechRecEve]: https://developer.mozilla.org/en-US/docs/Web/API/SpeechRecognition#Events