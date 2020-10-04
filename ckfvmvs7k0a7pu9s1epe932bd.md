## A Smart Way To Secure Your Django Apps


![cog-wheels-2125183_1280.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1601847853767/eb0QdJfRN.jpeg)
*Image by <a href="https://pixabay.com/users/qimono-1962238/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2125183">Arek Socha</a> from <a href="https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=2125183">Pixabay</a>*


Many times, we have to integrate 3rd party services into our applications; this often prompts the need to store sensitive data for authentication of different modules such as database credentials, secret key, encryption payloads, and API keys.

These sensitive keys should not be hard-coded in the ```settings.py ``` file or ``` views.py ``` file in a Django project. If these keys become compromised from a public repository or other location, the internet could easily find these keys and abuse them for their gains 😟 ; such as using up your cloud resources and credits, illegal access to your application back-end, and even dumping your live database 😰.

Instead, your keys should be loaded up with Environment variables in runtime.

It won't hurt to make our applications one level more secure 😃 and you'll find it quite useful to work with environments variables on different stages of application development.

## What are Environmental variables 🤔?

Environment variables are predetermined key-value pairs that typically provide the ability to configure a value or variable in your code from outside of your application for the current user environment.

They provide a greater degree of flexibility when switching between a local development environment as well as a production environment on a live server.

You can think of environment variables as a dictionary, where the key is the environment variable name and the value is the environment variable value.

## How to create and read  Environment Variables in Django projects

let's get started by creating environmental variables. we store our key-value pairs within an ```ini or .env file``` in a Django project.

In Python applications, we could use the Python os module “environ” property to get the dictionary of all the environment variables. But since os.environ only returns strings, it’s tricky. Let’s say you have an envvar ```DEBUG=False```.  If you run:
```
if os.environ['DEBUG']:
    print True
else:
    print False
```
It will print True, because os.environ['DEBUG'] returns the string "False". Since it’s a non-empty string, it will be evaluated as True. But that variable should be a boolean, right 🤷‍♂️.

i've found an awesome package to help out with this inconsistency and properly convert values to the correct data type 😃.

**Usage**
- Install
```
pip install python-decouple
 ```
- Then use it on your ```settings.py and views.py``` basically wherever we need these envvars, Import the config object:
``` 
from decouple import config
```
![Screenshot from 2020-10-04 20-33-27.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601841133858/VNXBRoOac.png)
- Retrieve the configuration parameters:
```
SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', default=False, cast=bool)
EMAIL_HOST = config('EMAIL_HOST', default='localhost')
EMAIL_PORT = config('EMAIL_PORT', default=25, cast=int)
```

![Screenshot from 2020-10-04 20-34-05.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1601841152424/S0IvSgLko.png)
Don’t forget to add   ```.env```  in your ```.gitignore file``` . to keep your secret keys out of version control.

We've learned about Environmental Variables and learned how to add another level of security to a Django application. It is a necessary step for any truly professional Django project.

It's a wrap everyone . 

Thanks for the audience and I hope you found this article helpful 🤗. feel free to reach out to [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.


 







