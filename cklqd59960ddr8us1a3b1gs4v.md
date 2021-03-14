## Scaleable Authorization Management Made Easy With CASL.JS 😀

## Introduction 
[CASL.JS](https://casl.js.org/v5/en/) at its core is an Isomorphic Authorization
JavaScript library ".  🤔

 
> 
The fancy word “isomorphic” means that you can use the library on both the frontend and backend in exactly the same way. * - *Sergii Stotskyi



> 
CASL (pronounced /ˈkæsəl/, like a castle) is an isomorphic authorization JavaScript library that restricts what resources a given client is allowed to access. It's designed to be incrementally adoptable and can easily scale between a simple claim-based and fully featured subject and attribute-based authorization. It makes it easy to manage and share permissions across UI components, API services, and database queries.

CASL is versatile, you can start with simple role-based access control and scale your apps to include fully-featured attribute-based access control.

CASL is declarative, it allows you to define permissions in the memory of the server-side using a domain-specific language that matches your business requirements almost word for word.

CASL is TypeSafe, it’s written in TypeScript, this makes apps safer and the developer experience more enjoyable.

CASL is small, it’s just ~4.5KB mingzipped and can be even smaller, thanks to tree-shaking! The minimum size is ~1.5KB.


## Backstory
<iframe src="https://giphy.com/embed/ljQg9SxQLlggDtrPoj" width="480" height="392" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

So while working on a Nodejs application at work; the team spent weeks researching and working on a hybrid access control framework that will be both scalable and dynamically change as the business needs require it.  

Recent developments have shown that attribute-based access control (ABAC) can provide flexible and fine-grained access control in both dynamic distributed systems and enterprise applications. As only the attributes of the subject, object, and environment are considered, most current solutions of typical Role based-access-control (ABAC) schemes cannot scale permissions to resources for a range of users with varied attributes.

Also in an (ABAC) framework,  the objective attributes are gained or defined by the resources themselves under certain conditions, such as time, location, IP address, and E.T.C. 

So in the hybrid access control framework, permission to resources is modeled across roles, claims,  attributes of users on the platform; such as user-defined attributes specified by the profile owner, such as gender, name, job, hometown, hobbies, and etc.



Well building a custom hybrid access control framework,  as versatile as discussed above can be quite resource-heavy, especially for small teams 🙄. 

We found [CASL JS](https://github.com/stalniy/casl) authorization library to be awesome, easy to adopt, heavily maintained, and efficiently capable of scaling with the business for dynamically managing permission to resources 😃. 

**ASSUMPTIONS**. I assume you already have some knowledge of [Nodejs](https://nodejs.org/en/) and managing permissions in your apps by claims or roles 😌. 
 
## Get started - TALK IS CHEAP 😆


1). Install @casl/ability as a dependency in your Nodejs application: 

```npm i @casl/ability```

2).  Define Abilities

There are 3 ways you can define abilities:

    - using defineAbility function
    - using AbilityBuilder class
    - using JSON objects

In this example, we'll be using the ```  defineAbility function ```

This function allows the creation of an Ability instance using can and cannot methods. It allows to define and use Ability instance without writing too much code.




```
//ability/defineAbility.js


const { AbilityBuilder, Ability } = require("@casl/ability");

/**
 * @param user contains details about logged in user: its id, name, email, etc
 */
// Define abilities for subjects here

// permissions on Organization
exports.defineAbilitiesOnOrganizationFor = (user) => {
  const { can, cannot, rules } = new AbilityBuilder(Ability);

  // condition == True
  if (user.isAdmin) {
    // can manage (i.e., do anything) own Oganization
    can(
      "manage", // can do everything
      "Oganization", //  Organization collection
      ["email",
        "phone",
        "password",
        "firstName", // feilds that can be managed
        "lastName"
      ],
      { _id: user._id } // condition , if OrgId=user.Id (belongs to an Org)
    );

    // But cannot delete Oganization
    cannot(
      "delete", // cannot delete Organization any Organization
      "Organization" // collection Organization
    );
  }

  if (user.isSuperAdmin) {
    // define the abilities for superAdmin on organization(subject)
    can(
      "manage",
      "Organization"
    );
  }

  return new Ability(rules);
};

```

in the above implementation we defined permissions for an "admin" and "super admin", we further set conditions for the organization as a resource that can be managed by both users.

CASL operates on the abilities level, that is what a user can actually do in the application. An ability itself depends on the 4 parameters (the last 3 are optional):

 **User Action**
    Describes what users can actually do in the app. User action is a word (usually a verb) that depends on the business logic (e.g., prolong, read). 
Very often it will be a list of words from CRUD - create, read, update and delete.


**Subject**
    The subject or subject type on which you want to check user action. Usually, this is a business (or domain) entity (e.g., Subscription, Article, User, Organization). 
The relation between subject and subject type is the same as the relation between an object instance and its class.

 **Fields**
    Can be used to restrict user action only to matched subject's fields (e.g., to allow the admin to update a field of a resource and disallow to update certain fields)

** Conditions**
    Criteria that restricts user action only to matched subjects. 
This is useful when you need to give permission on specific subjects (e.g., to allow the user to manage their own profile accounts)

YES;  easily business requirements can be translated into CASL's rules 🤩.

3).  Database integration
CASL has a complementary package [@casl/mongoose] which provides easy integration with MongoDB and [mongoose].

To install dependency: ``` npm install @casl/mongoose ```

```
// Organization.js


import { AbilityBuilder } from '@casl/ability';
import { accessibleRecordsPlugin } from '@casl/mongoose';
import mongoose from 'mongoose';
import {defineAbilitiesOnOrganizationFor} from 'ability/defineAbility.js'

mongoose.plugin(accessibleRecordsPlugin);

const user = getUserLoggedInUser(); // app specific function

const ability = defineAbilitiesOnOrganizationFor(user);


const Organization = mongoose.model('Organization', mongoose.Schema({
  email: String,
  isAdmin: Boolean, 
  phone: Number,
  password: String,
  content: String,
  createdAt: Date,
  firstName: String,
  lastName: String

}))

Organization.plugin(accessibleRecordsPlugin)

module.exports = mongoose.model('Organization', Organization)

```

4).   Check abilities

```
// ../controllers/organizationController.js

const { NotFound, Unauthorized, InternalServerError } = require("http-errors");
const Organizations = require("../models/organization");

async function permissionChecker(model, defineAbilitiesOnSubjectFor, userId) {
  try {

   // get user and all attributes
    const user = await model.findOne({ _id: userId });
    if (!user) {
      throw new NotFound("user not found");
    }
    // get ability
    const ability = defineAbilitiesOnSubjectFor(user);
     return ability;

  } catch (error) {
    throw new Unauthorized(error.message);
  }
}

let action = "update" // specify the type of action on the resource, to check for
let subject = "Organization"  // resource that needs permission and access control

// check for permission using the permission checker
    const ability = await permissionChecker(
      Organization,
      defineAbilitiesOnOrganizationFor,
      user
    );

if(ability) {
   try {
        const organization = new Organization();
        organization.set(data); // app specific data
        ForbiddenError.from(ability).throwUnlessCan(action, organization);
        await organization.save();
   }catch(error){
      throw new Unauthorized(error.message);
   }
} else{
   throw new InternalServerError(error.message);
}

```

Yea, it's that simple to implement a scaleable in memory permission management and resource authorization with CASL in your [Nodejs](https://nodejs.org/en/) apps 😊.

CASL is progressively adaptable, which means you can start your project with the simple role-based authorization and evolve it later when your app functionality evolves.

Thanks for the audience and I hope you found this article helpful 🤗. Feel free to reach out to me on  [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.


### FIND OUT MORE
- [CASL JS Documentation](https://casl.js.org/)





