# Backend-Template-Flask

Backend template using HTML5, CSS3, JavaScript, Flask and connected to a mongoDB database.

## Creating A MongoDB database

* A key step is to sign up to MongoDB if you have not already, once you have signed up or logged in, you need to create a new project.

* Give the project a unique name, i called mine for demo purposes `AI-Flask-Template`, then give access to other member is you want and click create project.

* Now its time to create the database, click on the build a database button and choose the Shared box which is free and click create.

* Then find choose you cloud platform, for the demo i have chosen AWS and my region is Europe and i chose Ireland since that is closer to the UK.

* Down at the bottom click the Cluster name dropdown and give your cluster a unique name, i called mine `AI-Flask-Temp-Cluster`.

* Now set the database username and password and make sure they are different from you MongoDB username and password and then click the finish and close. We do not want
to allow a specific IP address because this will mess with Heroku and wont allow Heroku to connect to the database.

* To make sure that IP address is not set, on the left navigation click on `Network Access` and if you see `0.0.0.0/0` that means it can be accessed by any IP Address
that has the database username and password.

* If you do not see the above, then click on `Add IP Address`, and then click on in the pop up window `Allow Access From Anywhere` and then click confirm 
and let mongoDB do it things and on the left navigation click on `Databases` under the Deployment section and process to the next step.

* Once that is done, now click on Browse Collections, this is where you will create your database to store your website info in a collection.

* Now click on Add My Own Data, now give your database a name and collection a name, for the demo i have set the database name as: `AI-Flask`
and the collection as: `info`, then click on create.

* Now we need to insert data into the collection, you do this by clicking on the `insert document` button located in the top right.

* Once clicked enter the following data:

```
fullname: Ifti Khan
position: Web Developer
company: AI Web Devs
description: AI Web Devs is a up and coming Web Development company. 
```

* Once this data has been inserted, you can add more if you like but its time to move onto the next step.

## Creating your GitHub repo and setting up the basics

* With the database now created we need to the new repo for the flask app in your github repository page, login in to your github or sign up if you have not already.

* For demo purposes i called my repo backend-template-flask.

* Once the repo is created, open in VSCode or gitpod, for demo purposes i am using VSCode and i cloned the repo to my local computer.

* Once open in VSCode or gitpod, open a new terminal and type the following command to install the flask libraries:

`pip3 install flask`

* Once flask is installed, now you need to create a few files and directories.

* The first file you need to create is a python file which will be the foundation for the app. To do this you can either right click on the file panel of VSCode or gitpod and create new file or type in the CLI:

`touch app.py`

* The next file is the environment file to hide sensitive date, again can be done by right clicking on the file panel or type into the CLI:

`touch env.py`

* The next file that needs to be created is the gitignore file, once a file is added to the ignore list it will not be pushed to github and the env.py needs to be added to this and never pushed to github for security reasons. Again can be done by right clicking on the file panel or type into the CLI:

`touch .gitignore`

* To add the env.py to the .gitignore list you can either manually type in the list when .gitnore file is opened:

```
env.py
__pycache__/ 
```

* Another method to add file to the .gitignore can be found in the changes section of VSCode and gitpod, you can right click on the file you want to ignore and click on the option to add to .gitignore

* The next thing to do is add these lines of code to the env.py file, these are to setup the default environment variables:

```
import os

# Setting the default IP server address, port number and secret key
os.environ.setdefault("IP", "0.0.0.0")
os.environ.setdefault("PORT", "5000")
os.environ.setdefault("SECRET_KEY", "go this address: https://randomkeygen.com/ and copy a random key, i chose fort knox passwords and paste here between the double quotes")

# This connects to the overall mongo database
os.environ.setdefault("MONGO_URI", "")
# This connects to the specific mongo database name
os.environ.setdefault("MONGO_DBNAME", "AI-Flask")
```

* I have done my IP and Port differently for my VSCode live server extension to work when i run the app, you do not need to do this, just ignore this is a personal choice for me.

```
os.environ.setdefault("IP", "127.0.0.1")
os.environ.setdefault("PORT", "8000")
```

* Now the next thing that needs to be done is open the app.py file and add these lines of code:

```
import os
from flask import Flask

if os.path.exists("env.py"):
    import env


app = Flask(__name__)


@app.route("/")
def hello():
    return "My Backend Basic Flask Template"


if __name__ == "__main__":
    app.run(host=os.environ.get("IP"),
            port=int(os.environ.get("PORT")),
            debug=True)
```

* Once this is done, you can then run the code by typing in the following command in the CLI:

`python3 app.py`

* Within the CLI you will see the local host address, press ctrl on your keyboard and then click the address a browser tab will open and show you the basic flask app running.

## Deploying the basic flask app to Heroku

* Now the next big step is to deploy the flask app to heroku, make sure you have a heroku account to hand for this, if not then create one.

* In the CLI of the flask template you need to type the following command to freeze the app dependencies:

`pip3 freeze --local > requirements.txt`

your requirements file should contain this:

```
click==8.0.3
Flask==2.0.2
itsdangerous==2.0.1
Werkzeug==2.0.2
```

* The next thing that needs to be done is create the Procfile either using the CLI or GUI, the CLI method you need to type into the CLI:

`echo web: python app.py > Procfile`

>**The Procfile is needed for Heroku, so it know which file to run, in this case it is the app.py file.
Make sure in the procfile there is not a 2nd blank line under the 1st line, the procfile should only contain one line and no blank lines.**

* The next thing to do now is push all the changes to github, this can be done using the CLI method by typing the following commands:

```
git status - to see what files have been modified
git add . or git add -A - to add the modified files to the staging area
git commit -m "message here" - getting files ready to push to github
git push - this pushes the modified files to the github repo
```

Or use the simpler GUI method that can be found on the left hand side panel with VSCode or Gitpod.

* Now Log into your heroku and create and app, either name it the same as your github repo name or similar using lowercase letter and dashes, for demo purposes i called mine `aiwebdevs-flask-template` and make sure you choose the closest region to you from the dropdown list, i chose Europe since i am in the UK.

* Once you click create app it will take you to the deployment page and from there we can connect our github repo to heroku. There are 2 methods to doing this, the first is the CLI and is a bit tedious to do.

* Here are the following commands to use the CLI:

```
Install the Heroku CLI
Download and install the Heroku CLI. Link Below
https://devcenter.heroku.com/articles/heroku-command-line

If you haven't already, log in to your Heroku account and follow the prompts to create a new SSH public key.

$ heroku login
Create a new Git repository
Initialize a git repository in a new or existing directory

$ cd my-project/
$ git init
$ heroku git:remote -a <appnamehere>
Deploy your application
Commit your code to the repository and deploy it to Heroku using Git.

$ git add . or git add -A
$ git commit -m "add message here"
$ git push heroku main
```

* The second method is by clicking the `connect to github` button in the deployment method section, this way is much easier and simpler.

* Once clicked, connect to your github by logging into your github when the popup window appears, if not already logged in.

* Then search and find the github repo that you created earlier for the flask app and then press connect button. Once the connect button has been click, a bunch of options will appear underneath, do not click on the `automatic deploy` button or the `deploy` button just yet, there are still some extra things to do.

* Next thing that needs doing is click on the settings button at the top, once clicked then click on Reveal Config Vars.

* In the reveal config var section you need to add all of the env.py files variables here like for like, exactly the same:

```
IP = 0.0.0.0
Port = 5000
SECRET_KEY = same as the secret key in the env.py file
MONGO_URI = Blank for now
MONGO_DBNAME = whatever you named your Mongo database
```

* Once all the config vars have been entered, go back to the deploy tab and click on enable automatic deployment which means, once a new git push is done to the GitHub repo, Heroku will also update, now click on deploy branch.

* Heroku will start to build the app this will take a bit of time, once done you will see your app was successfully deployed, under that message click on the view button and it will open a new tab and show you your deployed app up and running.

## Connecting Flask template to MongDB

* Now the next big step is to connect the Flask template to the Mongo Database

* In order to do this we need to install a few third part libraries, so in the CLI type the following commands:

```
pip3 install flask-pymongo - this library communicates with MongoDB
pip3 install dnspython - so that we can use the mongo SRV string
```

* Once these libraries are installed you now need to update the requirements txt file for heroku to update, type this command in the CLI:

`pip3 freeze --local > requirements.txt`

* Once the requirements have been frozen, then go to app.py file and add the following import lines under the flask import:

```
from flask_pymongo import PyMongo
from bson.objectid import ObjectId
```

* Now we need to do some configuration, the first we need to configure is the mongoDB name, then the mongoURI, and the secret key, add this code below under the app = Flask(__name__)

```
app.config["MONGO_DBNAME"] = os.environ.get("MONGO_DBNAME")
app.config["MONGO_URI"] = os.environ.get("MONGO_URI")
app.secret_key = os.environ.get("SECRET_KEY")
```

* Once this is done, we now need to get the mongoURI, so go to your mongoDB web page and login.

* If you do not have a mongoDB account go back to the top and follow the steps provided.

* Once your logged, you will be taken to the database deployments page, once on this page click on connect button and choose the option `connect to your application` in the popup window.

* Once that option is selected, then choose the correct driver, in this case we are using python and select the correct version.

* In the same window underneath you will see a line of code that you need to copy into the env.py file in the MONGO_URI setting line, between the double quotation marks.

>**It is important to note about this line you need to use the database username and password you set earlier not your mongoDB login username and password. Also you need to add the database name you want to connect to, please read the notice within the popup to make better sense of this.**

For the demo here is what the line should look like

```
os.environ.setdefault(
    "MONGO_URI", "mongodb+srv://<DB-Username>:<DB-Password>@ai-flask-temp-cluster.neahf.mongodb.net/<DB-Name>?retryWrites=true&w=majority")
```

* Once the MONGO_URI string is copied and you have added your database username, database password and database name, then go to your Heroku website, login if not already logged in and go click on settings, then click on reveal config var and then edit the MONGO_URI variable and paste
in the same string from the env.py file without the double quotation marks.

* Now you need to setup an instance of pymongo, add this line of code below the config settings in the app.py file.

`mongo = PyMongo(app)`

* Now we need to make sure that we can get the info from the database and display it within the app and to do this, you need to add the following code to the app.py file.

* At the top of the app.py file add the following imports:

`from flask import (Flask, flash, render_template, redirect, request, session, url_for)`

* Now add a new app route under the current app route which is the index page, use code below:

```
@app.route("/")
@app.route("/get_info")
def get_info():
    info = mongo.db.info.find()
    return render_template("info.html", info=info)
```

* Once this is added to app.py file, we now need to create the template folders in the root directory of the app, this can be done using the CLI or done in the left hand panel in VSCode. If you want to use the CLI method type the following commands:

```
mkdir templates - this makes the new folder
touch templates/info.html = this creates the html file within the templates folder
```

* Once the info HTML document is created, open it and add the following code:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Template Info</title>
</head>
<body>
    {% for i in info %}
    <strong>Fullname:</strong> {{ i.fullname }}<br><br>
    <strong>Position:</strong> {{ i.position }}<br><br>
    <strong>Company Name:</strong> {{ i.company }}<br><br>
    <strong>Description:</strong> {{ i.description }}<br>
    {% endfor %}
</body>
</html>
```

Within the body you will find jinja templating language and this code you see if a for loop. So the `i` in the loop is the iteration variable and the `info` is the backend variable within the app.py file we created before this page and are pushing from the backend to the frontend using the name info.

## Building the template files and directories

* Now we need to create the template inheritance, so you have one base html template and when you create a new html file,
you can call upon that base html and inherit the design and if changes are made to the base template all html files that inherit the
base html file will update.

* So the first thing to do is create the base.html within the templates dir, this can be done using the CLI method or the GUI method, type the following line for the CLI method:

`touch templates/base.html`

* Within the base.html file we need to add the following code, this will be a mix of HTML5 and the Jinja templating language:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flask Template</title>
</head>
<body>

    <h1>I am the base.html template</h1>
    {% block content %}
    {% endblock %}

</body>
</html>
```

>**The block content is the main block within the body, so when you create a new html file it needs to be wrapped within the block content to show up in the body, the rest of the code will extend from the base. You can name the block content to whatever you want but you have to make sure that the name matches in all html files.**

* So the next step is to modify the info.html code to the code below and you will get a better understanding of what is happening.

```
{% extends "base.html" %}
{% block content %}

<h2>I am extending the base.html template</h2><br>

{% for i in info %}
<h3>Database Info</h3>
<strong>Fullname:</strong> {{ i.fullname }}<br><br>
<strong>Position:</strong> {{ i.position }}<br><br>
<strong>Company Name:</strong> {{ i.company }}<br><br>
<strong>Description:</strong> {{ i.description }}<br>
{% endfor %}

{% endblock %}
```

>**As you can see the standard HTML code is gone now and at the top and this is key you need to extend from the base.html and then wrap you content within the block content for this all to work on every page html page you want to inherit the base template.**

* Now we need to start building the base templates by adding in the relevant cdn links, static file link to the css and js folders.

* So now its time to create the static folder and all of its files, this can be done using the CLI method or GUI method, for CLI method please type the following commands:

```
mkdir static - this makes the new folder
mkdir static/css - this makes the new folder within parent dir
mkdir static/js - this makes the new folder within parent dir
touch static/css/style.css = this creates the style css file within the css folder
touch static/css/responsive.css = this creates the responsive css file within the css folder
touch static/js/script.js = this creates the script js file within the js folder
```

Additional folders that i created are:

```
icons = for icons and favicon
images = for website images

mkdir static/icons - this makes the new folder within parent dir
mkdir static/images - this makes the new folder within parent dir
```

* Now the file structure for the static folder has been created, it is time now to build the base.html template accordingly. Just copy and paste the code below into the base.html file:

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <!---------------------------------------- CSS -->
    <!-- Bootstrap CSS v5.1.3 -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous"
        type="text/css" >
    <!-- Font Awesome v6.0.0 Beta 3 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css"
        integrity="sha512-Fo3rlrZj/k7ujTnHg4CGR2D7kSs0v4LLanw2qksYuRlEzO+tcaEPQogQ0KaoGN26/zrn20ImR1DfuLWnOo7aBA=="
        crossorigin="anonymous" referrerpolicy="no-referrer" type="text/css" />
    <!-- CSS Files -->
    <link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}" type="text/css" />
    <link rel="stylesheet" href="{{ url_for('static', filename='css/responsive.css') }}" type="text/css" />
    <!---------------------------------------- Other  -->
    <!-- Google Font-->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

    <!-- Custom internal CSS Block -->
    {% block styles %}
    {% endblock %}

    <title>AI Web Devs Flask Template</title>
    <link rel="shortcut icon" type="image/png" href="{{ url_for('static', filename='icons/favicon.png') }}" />
</head>

<body>

    <h1>I am the base.html template</h1>
    {% block content %}
    {% endblock %}

    <!---------------------------------------- Bottom JS  -->
    <!-- jQuery v3.6.0 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.js"
        integrity="sha512-n/4gHW3atM3QqRcbCn6ewmpxcLAHGaDjpEBu4xZd47N0W2oQ+6q7oc3PXstrJYXcbNU1OHdQ1T7pAP+gi5Yu8g=="
        crossorigin="anonymous"></script>
    <!-- Bootstrap JS v5.1.3 -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p"
        crossorigin="anonymous">
    </script>

    <!-- JS Files -->
    <script src="{{ url_for('static', filename='js/script.js') }}"></script>
    
    <!-- Custom internal JS Block -->
    {% block scripts %}
    {% endblock %}

</body>

</html>
```

* Important note here, to link to a css, js or any static file within your app that you have created you need to use url for method:

`{{ url_for('static', filename='css/style.css') }}`

* If you dont use this, you wont be able to connect them to the base.html file, here is the full code example below

`<link rel="stylesheet" href="{{ url_for('static', filename='css/style.css') }}" type="text/css" />`

* Another thing that has been added to the base.html are the styles and scripts blocks, these block are here to inject custom css and js code into the template.

```
{% block styles %}
{% endblock %}

{% block scripts %}
{% endblock %}
```

* Once all the above is done, now we need to test the app is working properly and all static files are working. A simple test that can be done is in the style.css file copy and paste this code below into the style.css file:

```
body {
    background-color: aqua;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
}
```

* Now run you app by typing into the terminal:

`python3 app.py`

* Then open the development server to view your app, if the background has change to the aqua blue and font has changed then everything is working ok. 

* If not you need to hard reload the page by holding the shift key and clicking on the page refresh button. If the page background colour changes and font then everything is working fine.

* Now go back to the style.css file and remove the code pasted in earlier and push all changes to github using CLI or GUI.
  
```
git status - to see what files have been modified
git add . or git add -A - to add the modified files to the staging area
git commit -m "message here" - getting files ready to push to github
git push - this pushes the modified files to the github repo
```

* Once this is done, you are all set to modify the Flask template to your needs.