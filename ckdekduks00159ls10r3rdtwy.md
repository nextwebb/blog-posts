# When Do I Use CSRF Protection?

The Common network  requests, also termed as HTTP requests include GET and POST.  

![solen-feyissa-UWVJaDvXW_c-unsplash.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1596461584912/hVGD9f8Ag.jpeg)

 A GET request is simply a request for a page from a server, e.g. visiting www.google.com.

 A POST request is sent when you send data to the server, e.g submitting a login form on www.twitter.com or sending search query's on www.google.com to return indexed results.

If we never used or needed dynamic applications, We'll all be safe and at-least  one vulnerability down, when we interact and use  web products and applications.


![images.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1596461219393/R7ng9qAsI.jpeg)
**How?** 
In simple words, any application that allows a user to send or update data is potentially a target for an attacker or malicious user. The following is a list of potential uses for Cross site resource forgery (CSRF):

- Transfer money from one bank account to another.
- Use  content management systems to access and modify user data
- Change a user’s password.
- Add items to a user’s shopping cart.
- Change the delivery address of an order.
- Post a tweet on a user's account. 
E.T.C

Thus, from a developers point of view  implementing CSRF tokens on all types of form data sent to the server, is fundamental to the applications security.

### So When do i use CSRF protection ?

```
Formula: No cookies = No CSRF
``` 

Because HTTP is a stateless protocol, in which no session information is retained by the receiver  it cannot internally distinguish one user from another. By using cookies, servers instruct browsers to save a unique key or session data and then send it back with each request made to the server.

When a network request is sent from a browser to a  server, the browser checks if it has a stored cookie that belongs to that server. While carrying out this process, it checks to see whether the properties and flags of the cookies (domain, path, secure), match the website's data which has been requested. If they match, the browser sends the relevant cookies along with the request.

It really is that simple. Browsers send cookies along with all requests. CSRF attacks depend upon this behavior. If you do not use cookies, and don't rely on cookies for authentication, then there is absolutely no room for CSRF attacks, and no reason to put in CSRF protection. If you have cookies, especially if you use them for authentication, then you need CSRF protection.

 If all you want to know is "Do I need CSRF protection for my API routes?" you can stop right here and leave with your answer 😀. You've got your answer!

Personally for my applications i use [JWT](https://jwt.io/) for my Authentication on my API routes.

Reach out and connect with me on [Twitter](https://twitter.com/I_am_nextwebb), [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/) and [Github](https://github.com/nextwebb) .

