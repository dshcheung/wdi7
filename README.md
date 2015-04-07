#Table of Content
##Contents
 * [The Basic MVC Framework](#basicMVC)
  - [Trying It Out](#tryItOut)
 * [Advanced MVC Framework with Database](#advancedMVC)
  - [Relational vs Non-Relational Databases](#databaseType)
  - [Simple Model Creation](#simpleModel)
  - [Using Databases](#usingDatabases)
  - [Types of Associations for Models](#modelAssociations)
  - [Types of Data Types for Migration](#migrationTypes)
  - [Methods for Using the Database](#databaseMethods)
  - [Linking It All Together](#linkingAll)
  - [Using AJAX with ROR](#ajaxROR)
  - [Using Devise](#devise)
  - [Model Validations](#validations)

<a name="basicMVC"></a>
#The Basic MVC Framework
For the following session, we would start off with the simplist framework.

![The flowchart](https://raw.githubusercontent.com/dshcheung/wdi7/master/images/MVC%20Simple1.jpg)

The Components:
 1. **User**
 2. **Routes** - Redirecting the user request
 3. **Controller** - Processes the user request
 4. **View** - Create content for user to see

How it works:
 1. **User** enters "www.something.com/" in the browser
 2. **Routes** check the link and redirect to the appropriate controller. (similar to post office)
 3. **Controller** will ask the view to generate the HTML page
 4. **View** will send the HTML page to browser, and the browser will display the HTML

<a name="tryItOut"></a>
##Trying It Out
###Step 1
First fork this [repo](https://github.com/HK-WDI-November-2014/rails-basic-template)

After setting up your ROR, open folder in sublime.

###Step 2
In sublime, search for **"routes.rb"** under
```
config > routes.rb
```

Add line 
``` ruby
root 'static_pages#index'
```

You can check if you have done this correctly in iTerm by typing
```
rake routes
```

###Step 3
Lets create a controller!

In iTerm, type
```
rails g controller static_pages
```

You should be able to see that rails have created all the important files/folders for you!

###Step 4
In sublime, search for **"static_pages_controller.rb"** under 
```app > controllers```

add
``` ruby
def index

end
```

###Step 5
Create a file called **"index.html.erb"** under 
```
app > assets > views > static_pages
```

Note where we are putting this file because this is very important.

In this file, add
``` html
<h1>My first HTML page in ROR</h1>
```

###Step 6
Run the server!

Similar to ``` python -m SimpleHTTPServer 8080 ``` we can type ``` rails s ``` in iTerm to start the server

In your browser, go to ``` localhost:3000 ```

WOW! You just created your first page in ROR!

You might be thinking... Why go through all this trouble to do something I can already do with a simple setup?

Well... you will find the answer below.

<a name="advancedMVC"></a>
#The Advance MVC Framework With Database

![The flowchart](https://raw.githubusercontent.com/dshcheung/wdi7/master/images/MVC%20Simple%20Explained.jpg)

[Link to the clean version](https://raw.githubusercontent.com/dshcheung/wdi7/master/images/MVC%20Simple2.jpg)

The Components:
 1. **User**
 2. **Routes** - Redirecting the user request
 3. **Controller** - Processes the user request
 4. **Model** - Validates the data before querying the DB
 5. **Database** - Contains all the data
 4. **View** - Create content for user to see

How it works:

 1. **User** enters "www.something.com/" in the browser
 2. **Routes** check the link and redirect to the appropriate controller. (similar to post office)
 3. **Controller** will ask model to do something depending on the method. (Remember CRUD?)
 4. **Model** validates the request and query the database to Create/Read/Update/Delete
 5. **Database** process the request and respond to model with data
 6. **Model** responds to controller with data
 7. **Controller** might further process data, and then ask the View to generate the HTML/JSON
 4. **View** will send the HTML/JSON to browser, and the browser will display the HTML/JSON

<a name="databaseType"></a>
##Relational vs Non-Relational Databases
### Non-Relational Databases
Before making a model and database, there are something you must understand.

Imagine you are trying to create Reddit and you have stored everything in a big hash
``` js
var myReddit = {
  users: [
    {
      username: "denis",
      email: "test@test.com",
      mobile: 12345678
    }
    {
      username: "harry",
      email: "test2@test.com",
      mobile: 87654321
    ]
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
    }
  ]
}
```
Now imagine you want to find the author's email of the first post.

Wait what? That's so simple Denis, its not even a challenge! 

All you have to do is find the author of the first post then search for users with the same username and get his email.
``` js
var postAuthor = myReddit.posts[0].author;
var users = myReddit.users;
var findEmail = function () {
  for (var i = 0; i < users.length; i++){
    var user = users[i];
    if (user.username == postAuthor){
      return user.email;
    }
  }
};
var email = findEmail();
```

Simple right! You must be out of your damn mind! 

While non-relational DB is quick if you know where the data is it is messy if you want to search for stuff like **Find posts that belong to first user**, so good luck with it. 

###Relational Database
Using the previous example, what if you can simply type
``` ruby
Posts.first.user.username
```

This would find the first post then find the user associated to it and return you the username. So much quicker right!

What relational database does, is that they will help you associate tables with each other.

A simple association
 - User **has_many** Posts
 - Post **belongs_to** User

![Table](https://raw.githubusercontent.com/dshcheung/wdi7/master/images/Simple%20Table.png)

Given the simple association decloration, rails will **automatically** add a column **user_id** to the Post Table hence associating them.

Now whenever you are refering to ```Post.first.user``` or ```User.first.posts```, rails will automatically give you all the records.

If you still like non-relational database for these kinds of operations... You seriouslly have something wrong with you.

<a name="simpleModel"></a>
##Simple Model Creation
We will now create simple models and migration files.

###Step 1
In iTerm, type
```
rails g model post
rails g model comment
```

This should automatically generate both the model and migration file.

###Step 2
Open the model files
```
app > models > post.rb
app > models > comment.rb
```

In **post.rb** add ```has_many :comments```

In **comment.rb** add ```belongs_to :post```

Please note the singular and pural as this is very important.

###Step 3
Open the migration files
```
db > migrate > xxxxxxxx_create_posts.rb
db > migrate > xxxxxxxx_create_comments.rb
```

In **create_posts.rb** add
``` ruby
create_table :posts do |t|
  t.string :title
  t.string :content
  t.timestamps
end
```

In **create_comments.rb** add
``` ruby
create_table :comments do |t|
  t.string :content
  t.integer :post_id
  t.timestamps
end
```

Note that the column **id** is automatically put in and does not require you to create it.

###Step 4
Cooking the recipe / running the migration.

We have now created the recipe, models are the instructions and migration is the ingredients. All we need to do is to actually cook the recipe.

In iTerm type ```rake db:migrate``` and restart your server.

<a name="usingDatabases"></a>
##Using Databases
Before trying to use the controller to add entry to database, lets use the irb.

###Step 1
In iTerm, type ```rails c```

###Step 2
Try these few commands in sequence and pay attention to the results

Note that you always use Capital when refering to a table
``` ruby
Post.all
Post.new
post = Post.new(:title => "Reddit Sucks", :content => "Yeah, reddit really sucks")
post.save
Post.create(:title => "Actually, I love Reddit", :content => "I just don't use it")
Post.all
Post.first
Post.last
Post.find(1)
Post.find(2)
Post.find(3)
```

That was easy and fun

###Step 3
Since we associated posts and comments,we can now do this:
``` ruby
post = Post.find(1).comments.new(:content => "Reddit does not suck")
post.save
Post.find(1).comments.create(:content => "You are an idxxt")
Post.find(1).comments.create(:content => "You are the crazy one")
Post.find(1).comments.create(:content => "Both of you are crazy")
```

If you have noticed, we didn't need to specify **user_id**. This is because rails knows the association and did it for us.

###Step 4
We can now play with it a little. Try these and pay attention to the results
``` ruby
Post.count
Comments.count
Post.first.comments
Post.last.comments
Comment.find(1).post
Comment.find(2).post
Post.all
Comment.all
```

<a name="modelAssociations"></a>
##Types of Associations for Models
The following is a list of association which you will all use in the future.
 - belongs_to
 - has_one
 - has_one, through:
 - has_many
 - has_many, through:
 - has_and_belongs_to_many
 - Polymorphic
 - Self

The common ones we would be using is ```belongs_to``` and ```has_many```

<a name="migrationTypes"></a>
##Types of Data Types for Migration
 - String/Text
 - Interger(2,4,8 bit)(limit: 2,4,8)
 - Float
 - Boolean
 - Serialize(Hash/Array)

<a name="databaseMethods"></a>
##Methods for Using the Database
The following is a list of common methods for using the database for your reference.
``` ruby
#all
  User.all
#first/last
  User.first
#count
  User.count
#order
  User.order(created_at: :asc)
#limit
  User.order(created_at: :asc).limit(10)
#offset
  page = params['page'] * 10
  User.order(created_at: :asc).limit(10).offset(page)
#find(n)
  User.find(1)
#find_by
  User.find_by(first_name: ‘denis’, last_name: ‘cheung’)
#where
  User.where(age: 40)[0][:email]
#distinct/uniq
  User.distinct
#pluck
  User.pluck(:religion).uniq
#group
  User.group(:religion).count
#include(eager loading)
  Post.find(1).user
  User.includes(:posts).where('posts.name = ?', 'WDI').references(:posts)
  User.includes(:posts).where(:posts => {name: 'WDI'})
#joins(lazy loading)
  User.joins(:posts).where('posts.name = ?', 'WDI').references(:posts)
  User.joins(:posts).where(:posts => {name: 'WDI'})
```
<a name="linkingAll"></a>
##Linking It All Together
Lets try to show the data from database in view.

###Step 1
Everything starts with the route, we will now create an index page for posts.

Recall the advanced MVC framwork? This is where we are putting it to use.

In **routes.rb** add ```resources :posts```

What this will give you is
``` ruby
#     posts GET    /posts(.:format)          posts#index
#           POST   /posts(.:format)          posts#create
#  new_post GET    /posts/new(.:format)      posts#new
# edit_post GET    /posts/:id/edit(.:format) posts#edit
#      post GET    /posts/:id(.:format)      posts#show
#           PATCH  /posts/:id(.:format)      posts#update
#           PUT    /posts/:id(.:format)      posts#update
#           DELETE /posts/:id(.:format)      posts#destroy
```

Which command checks the routes?

###Step 2
In iTerm, create controller for **posts**

What command generates controller?

###Step 3
In the controller we just created, add the **method index**

What does the method syntax looks like?

###Step 4
Inside the index method, add
``` ruby
@posts = Post.all
```

###Step 5
Create a file **index.html.erb**

Where should this file be placed?

###Step 6
Edit the **index.html.erb** we created for posts.
``` html
<h1>List of all Comments</h1>

<% @posts.each do |post| %>
  <div>
    <%= post.id %>
    <%= post.title %>
    <%= post.content %>
    <%= post.created_at %>
  </div>
<% end %>
```

###Step 7
Start the server if you havn't already, then go to ```localhost:3000/posts```

Bravo! You have just used the whole MVC model!!!!

<a name="ajaxROR"></a>
##Using AJAX with ROR
Lets try to use AJAX to create a post!

###Step 1
As always, we start off with routes. However by using ```resources :post``` it have already generated the route for us.

**Challenge**
 - According to CRUD, which AJAX method should we use when creating a post?
 - With that specific AJAX method in mind, use the ```rake routes``` command to see which controller and what method you need to use/create.

###Step 2
**Challenge**
 - In the post **index.html.erb** create inputs for **title and content**
 - Create a button that sends the data through AJAX request

###Step 3
Now we need the controller to process the data we sent it and create a database entry with it.

When the controller recieves the data, it will automatically put it in the **params** variable

Given you sent these data in AJAX
``` js
var data = {
  title: "My Title",
  content: "My Content"
}
```

Will give you
``` ruby
params["title"] #returns "My Title"
params["content"] #returns "My Content"
```

**Challenge**
 - Given the format below, fill in xxxxx

``` ruby
post = Post.new(:title => xxxxx, :content => xxxxx)
if post.xxxx
  redirect_to posts_path
else
  flash[:message] = post.errors.messages
  redirect_to :back
end
```

###Step 4
Since we have already create the view, all you need to do now is to test to see if the whole thing works

<a name="devise"></a>
##Using Devise
Follow these instructions on install and using devise

**Challenge**
 - Create devise user
 - Add associations to Post and Comment
 - Create devise view for user
 - Cook the database
 - Restart your server
 - go to http://localhost:3000/users/sign_in
 - go to http://localhost:3000/users/sign_up
 - Create a sign in button
 - Create a sign up button
 - Create a sign out button

<a name="validations"></a>
##Model Validations




