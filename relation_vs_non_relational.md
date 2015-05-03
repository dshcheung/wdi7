#Relational vs Non-Relational Databases
<a name="non-relational">
## Non-Relational Databases
Before making a model and database, there are something you must understand.

Imagine you are trying to create Reddit and you have stored everything in a big hash
``` js
var myReddit = {
  users: {
    denis: {
      username: "denis",
      email: "test@test.com",
      mobile: 12345678,
      post: [{
        title: "Reddit Sucks",
        content: "Yeah, reddit really sucks",
        author: "denis"
      },
      {
        title: "Actually, I love Reddit",
        content: "I just don't use it",
        author: "denis"
      }]
    },
    harry: {
      username: "harry",
      email: "test2@test.com",
      mobile: 87654321
      post: [{
        title: "I love RoR",
        content: "I am a RoR master",
        author: "harry"
      }]
    }
  }
  posts: [
    {
      title: "Reddit Sucks",
      content: "Yeah, reddit really sucks",
      author: "denis"
    },
    {
      title: "Actually, I love Reddit",
      content: "I just don't use it",
      author: "denis"
    },
    {
      title: "I love RoR",
      content: "I am a RoR master",
      author: "harry"
    }
  ]
}
```

Lets say we want all the post that belongs to "denis". We simply use 
``` js
myReddit.users["denis"].posts
```

The use of non-relational database is very simple and straight forward. Therefore a lot of times you will see a lot of "sloppy" database with very messy data because its too "free". Therefore, it does require some amount of work to keep the database in-line.

<a name="relational">
##Relational Database
Using the previous example, what if you can simply type
``` ruby
Posts.first.user.username
```

What this line means is to 
 1. **find the first post** 
 2. **find the user associated to it** 
 3. **return you the username**

Unlike non-relational database, relational have tables for each "subject". For example user data like email and username will only be saved to the **User** table. While post title and post content will be saved to **Post** table

Then relational database will help you associate tables with each other.

A simple association
 - User **has_many** Posts
 - Post **belongs_to** User

![Table](https://raw.githubusercontent.com/dshcheung/wdi7/master/images/Simple%20Table.png)

Given the simple association decloration, rails will **automatically add a column user_id to the Post Table** hence associating them. Meaning that **the post now belongs to user with id x**. This is similar to what the non-relational database example did, but instead of using **author** we now use **user_id**

Now this is a two-way association meaning whenever you are refering to ```Post.first.user``` or ```User.first.posts```, rails will automatically give you the associated information.

```Post.first.user``` giving you first post's user

```User.first.posts``` giving you all of first user's posts

<a name="conclusion">
##Conclusion
There are some clear advantages of both relational and non-relational database. Non-relational are free form but could be messy without management, while relational database might be itimidating but very structured.
