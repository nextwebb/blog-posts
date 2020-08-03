## Offline Compatibility With Background Synchronization



# Offline Compatibility With Background Synchronization




![1_tPaXDNfSaiSRPwX2fPy8Sg.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1596451405189/W-PJYwxCL.jpeg)
*frustrated from poor internet connection*

ManyAtimes we’ve tried to submit a form, checkout of a cart, post a tweet or complete a task , only for the internet connection to drop 🙄 and the browser to presents you with an offline page and the dinosaur game 😀 .

Not until recently, we’ve been unable to better handle offline compatibility with web applications in situations like these and it’s being a rather crappy user experience 😣.

Fortunately, this is one area where Service Workers shines. It provides offline compatibility, better page functionality and with increased page load speeds using browser storage caching strategies and indexDB .

While service workers is great, we cant actually send data to a server without network synchronization or connectivity. Background Sync API is the feature that has been built to better handle a scenario were there’s no connectivity.

**According to google’s developer website ;**
 
 > *Background Sync API. Lets you defer actions until the user has stable connectivity. This is useful to ensure that whatever the user wants to send is actually sent. This API also allows servers to push periodic updates to the app so the app can update when it’s next online*
 
 In simple words the background sync API , listens for network synchronization or connectivity then the cached data or other stored data in the browser is sent to the server.

##  How to request a background sync

Sample implementation



```
if (‘serviceWorker’ in navigator && ‘SyncManager’ in window) {
 navigator.serviceWorker.ready
 .then(function(sw){
 // custom form data to be sent
 // its been stored in indexdb
 var post = {
 id: new Date().toISOString(),
 title: titleInput.value,
 location: locationInput.value
 };
 // data to the indexdb browser datastore
 // writeData() is custom utility function for writing data to in indexDB
 writeData(‘sync-posts’, post)
.then(function(){
 // regiter a certain synchronization task
 return sw.sync.register(‘sync-new-posts’);
 }).then(function(){
 console.log(“Post is syncing”)
 })
 .catch(function(err){
 console.log(err);
 })

 });
 } else {
 // heres the fall back in case , the browser doesnt support background sync event
 // custom
 sendData(post)
 }
 
 In service workers, sw.js;
 
 self.addEventListener(‘sync’, function(event) {
 if (event.tag == ‘myFirstSync’) {
 event.waitUntil(doSomeStuff());
 }
 });

``` 

Thanks for reading and anticipate more practical technical reads. :)

CONNECT with me [Twitter!](https://twitter.com/I_am_nextwebb)
Don't forget to share to like,  comment and share !  





**Useful Resources**

Introducing Background Sync:[https://developers.google.com/web/updates/2015/12/background-sync](https://developers.google.com/web/updates/2015/12/background-sync?authuser=0)

A Basic Guide to Background Sync:
 [https://ponyfoo.com/articles/backgroundsync](https://ponyfoo.com/articles/backgroundsync)

More about Firebase Cloud Functions: [https://firebase.google.com/docs/functions/](https://firebase.google.com/docs/functions/?authuser=0)

