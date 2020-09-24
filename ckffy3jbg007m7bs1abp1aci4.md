## How to Fix failed Django  database migrations on Heroku --- Programming error


![heroku_share.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1600873409753/tKIYofd2p.png)

[Heroku](https://devcenter.heroku.com) is a cloud application platform that provides deployment services for multiple technologies such as Django/python. It is essentially a Platform-as-a-Service (PaaS) cloud infrastructure, which simplifies hosting applications when compared to a cloud provider like AWS Elastic Compute Cloud(EC2) or others.  Heroku makes deploying applications quite simple and for good reasons 🤓.

This platform equips us with a ready runtime environment and application servers. we benefit from seamless app integration with various development instruments, a pre-installed operating system, terminal, Dynos ( isolated virtualized Linux containers ) and redundant servers 🏢 .

Therefore, we don’t need to think about infrastructure management, unlike with [AWS EC2](https://aws.amazon.com/ec2/). We only need to choose a subscription package and scale up when we need more resources.

We use [AWS EC2 and Heroku (PaaS)](https://rubygarage.org/blog/heroku-vs-amazon-web-services#:~:text=Heroku%20is%20a%20Platform%20as,application%20using%20these%20two%20solutions.)
 to reach different goals,
which is a subject worth reviewing .  

### A common error with heroku deploys can be failed database migrations within  your  Heroku Dyno 😞. 
which will be a case that when I run 
```
heroku local web
``` 
 the website works fine when I go to 
> 
localhost:5000

 or any other port that serves the app locally. However after I deploy the app to Heroku with ```git push heroku master,```  it returns  
> 
django-on-heroku-programmingerror-at-relation-does-not-exist

From personal experience , this issue might result from a change to a model and then deploying this change to your app's Heroku Dyno. 

**Assumptions**. The app in question is still in development and as such our database tables isn't large and thus easily dispensable. 

- Reset the database in Heroku
- Delete all the migrations files  for the broken app locally except  the __init__.py file.

- Run python ```python3 manage.py makemigrations``` and ```python3 manage.py migrate```. This will repopulate the migrations folder with clean migration files.

- Push changes to the default branch and ensure that you do not have the  migrations directories in .gitignore files.

- Deploy changes to Heroku
- Run the heroku shell with the command ```heroku run bash```
- Run ```python3 manage.py migrate```

That should fix the error. but I would try to avoid resetting the database if I had more data in my tables.

Thanks for the audience 🤗, i hope this fix saves  you alot of debugging time 😃 . feel free to drop a like , comment and share 😉. 


