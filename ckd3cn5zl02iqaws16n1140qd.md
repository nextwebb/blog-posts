# How to build an email listings API with  Googledrive + Googlesheets  + Nodejs


![lukas-blazek-mcSDtbWXUZU-unsplash.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1595780170244/J3Fz59d_d.jpeg)

This will be a simple tutorial of how i integrated google-sheets in NodeJs for an email listings API.
This is also a unique use case, peculiar to my needs while working on a small Nodejs project. But this methodology can be applied for broader use cases and of-course modified. 

## First challenge

**Google sheets package has great documentation; but there wasn't enough resources to learn how to generate your unique API key for Authorization, to access the google drive resources  programmatically.**
Let's start off:

- Login to  google console platform create a new project
- Navigate to the dashboard and click on Enable APIs and services

![VUanNnxH5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1595777555139/oiO68EB5J.png)

- Search for Googledrive API and enable it 

![ihSRSK07z.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1595777408013/tfev6iDlp.png)

- Click on manage API ,  create create credentials and setup

![FnQENfJiH.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1595777000821/l9KaPe_Zk.png)

- Head back to APIs and services tab and enable the google sheets API

![2ArTBh5Qc.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1595777206660/kA8mfGPh5.png)

Thats done , let's head to the next challenge 

 ## Second challenge 
- install the google-sheets npm package [here](http://npmjs.com/package/google-spreadsheet)
```
 http://npmjs.com/package/google-spreadsheet
``` 
i want to make my code modular and employ the separation of concerns(SOC) concept,
so my EmailListings class has three methods; namely 
     - accessGoogleSheet
     - addToSheet(data) 
     - retreiveList()

```
class EmailListings {

}
module.exports = EmailListings

``` 

this can can instantiated and the methods called from node server.js file

### Let's load up all the configurations

** This method loads up the spreadsheet unique identifier and the secret credential.json file we generated from google console.**
```
// import the google spread sheets package
 const { GoogleSpreadsheet } = require('google-spreadsheet');

class EmailListings {

    async accessGoogleSheet(){
        // this doc object loads up an instance of the spreadsheet by ID
        // the ID is defined as a url parameter on the google spreadsheet 
        const doc = new GoogleSpreadsheet('7s_9aHRSBNYTGrUYvjTETH8rskNciVRdFsZb2tgC89kw');
         // load directly from json file if not in secure environment
         await doc.useServiceAccountAuth(require('./client_secret.json'));
         await doc.loadInfo(); // loads document properties and worksheets
         return doc
        
    
    }

``` 
###  Select and update data to  the spreadsheet

**For this tutorial we will statically define our sheet ID for the spreadsheet. This methods takes in the data sent from the api and adds it to the sheet.**
```

async addToSheet(data){
        try {
           // select the inputed email
           const email = data.Emails 
   
           // invoke the method to access the particular googlesheet
            const doc = await this.accessGoogleSheet() //sheet 
            // update the google sheet with a title
            await doc.updateProperties({ title: 'Feedback email list' });
            
          // the spreadsheet can have many sheets, select a particular sheet by id or index
           const sheet = doc.sheetsByIndex[0]; // or use doc.sheetsById[id]
          // rename the sheet with a title
           sheet.updateProperties({title: 'email listings'})
           await sheet.setHeaderRow(['Emails']) //Set first row of the sheet
           await sheet.addRow({Emails: email}) // 
            return 'success!'
        } catch (error) {
            console.log(error)
            return error
        }
    }   
``` 
###  Select and retrieve data from  the spreadsheet

**This method essentially retrieves an update list of emails added to the spreadsheet.
the spreadsheet can also be viewed across the integrated google drive account.  **
```
 

async retreiveList(){
       // invoke the method to access the particular googlesheet
        const doc = await this.accessGoogleSheet() //sheet 
      //  select a particular sheet by id or index
        const sheet = doc.sheetsByIndex[0]; 
        try {
            // Returns a range reference from a starting row reference
            const getRow = await sheet.getRows({offset:1})
           
            // Return an array of emails
            const emailList = getRow.map((row) => {
                return row.Emails
            })
            return emailList;
        } catch (error) {
            return error
        }
    }
}
``` 
that's all for that email listings methods :).

## Third challenge
 
**We need a simple node server to make network requests to google-drive in this context, 
the goal here is not to spin up a major Nodes production server but one that proves a concept.
So let's write one.**


```
const express = require('express');
const app = express();
const cors = require('cors')
const Emailmodels = require('./email_listings');


app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// whitelist origins
const whitelist = ['http://localhost:3000', 'liamgadgets.com']

const corsOptionsDelegate = function (req, callback) {
    let corsOptions;
  if (whitelist.indexOf(req.header('Origin')) !== -1) {
    corsOptions = { origin: true } // reflect (enable) the requested origin in the CORS response
  } else {
    corsOptions = { origin: false } // disable CORS for this request
  }
  callback(null, corsOptions) // callback expects two parameters: error and options
}

app.use(cors(corsOptionsDelegate))

// api routes

app.get('/',  (req, res) => {
  res.send('welcome!')
})

// this api route updates the spreadsheet on the drive , with new data
app.post('/api/v1/update', (req, res) => {
    let emailmodel = new Emailmodels()
    emailmodel.addToSheet(req.body).then(()=>{
        res.status(201).json({
            status: true,
            message: 'email listing updated'
    })
}).catch(
    (err) =>res.status(400).json({
        status:false
    }) )
})

// this api route retreives a list of upated emaill listings from spreadsheet on the drive
app.get('/api/v1/retrieve',  (req, res) => {
    let emailmodel = new Emailmodels()
    emailmodel.retreiveList().then((emails)=>{
        res.status(200).json({
            status: true,
            message:emails
    })
}).catch(
    (err) =>res.status(400).json({
        status:false
    }) )
})

// catch 404 and forward to error handler
app.use((req, res, next) => {
  next(res.status(400).json({
    status: false,
    data: 'Bad request'
  }));
});

// error handler
app.use((err, req, res) => {
  // render the error page
  res.status(err.status || 500).json({
    status: false,
    data: 'internal server error'
  });
});
const port = process.env.PORT || 3000
app.listen( port, () => 
console.log(` listening at http://localhost:${port}`)
)
``` 

We learnt a great deal about using one of google drive resources, i.e google spreadsheets.
The google drive API provides alot of resources at our disposal, we could automate a ton of stuff and improve our productivity . Check them out [here! ](https://developers.google.com/drive)

Reach out and connect with me on [Twitter](https://www.twitter.com/i_am_nextwebb),  [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144) and [Github ](https://www.github.com/nextwebb).


Resources:

https://developers.google.com/sheets/api

https://console.cloud.google.com/

https://developers.google.com/drive

https://www.npmjs.com/package/google-spreadsheet





