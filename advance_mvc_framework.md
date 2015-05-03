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

This time lets think of a data agency

 1. **User** enters "www.something.com/" in the browser. (Send a request to agency asking for specific data)
 2. **Routes** check the link and redirect to the appropriate controller. (Front-dest recieves the request and forward request to the appropriate department)
 3. **Controller** will ask the view to generate the HTML page. (The processing Department recieves the request and ask the data department for data)
 4. **Model** check if the request is valid. (The data department recieve request from processing department and validates if they have clearance)
 5. **Database** process the request and respond to model with data. (Data department will go through their database for requested data)
 6. **Model** responds to controller with data. (Whether or not data department was able to find the requested data, they will send a response to processing department)
 7. **Controller** might further process data, and then ask the View to generate the HTML/JSON. (The processing department recieves the data and package it into PDF...or whatever...use your imagination... then send it out)
 8. **View** will send the HTML/JSON to browser, and the browser will display the HTML/JSON (Recieves the data in a neat format)

<a name="simpleModel"></a>
##Simple Model Creation
###Understanding Associations
Before we start. Imagine we have two set of table we want to **associate**. Posts and Comments.

Now the question is how. Without going through actual coding, lets **literally express it in human understandable language aka. not code**. 

When creating associations you have to always keep in mind the type of association you want for the table. In our example of Posts and Comments, how many comments can there be about a post? how many posts can there be for a comment? For this we can say "1 Post **has many** Comments" and "1 Comment **belongs to** 1 Post"

We call this a **one-to-many** association. There are more associations which we will learn later.

###Creating Model and Associations
We will now create a simple models(defines the association) and migration(defines the columns of table) files.

Using our previous example of Post and Comments, we can express this in a basic excel format.

![Table](https://raw.githubusercontent.com/dshcheung/wdi7/master/images/Simple%20Model%20Creation.png)

####Step 1
In iTerm, type
```
rails g model post
rails g model comment
```

This should automatically generate both the model and migration file.

####Step 2
Open the model files
```
app > models > post.rb
app > models > comment.rb
```

In **post.rb** add ```has_many :comments```

In **comment.rb** add ```belongs_to :post```

What we are doing here is adding associations so that rails help us connect the tables. Please note the singular and pural as this is very important.

####Step 3
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

####Step 4
Cooking the recipe / running the migration.

We have now created the recipe, imagine models are the instructions and migration is the ingredients. All we need to do is to actually cook the recipe.

In iTerm type ```rake db:migrate```

An important concept in cooking is that if you add a new raw ingredient, you need to cook it again. This is the same for migration, if you have a new table or new column, you need to cook it again.

There! You have just successfully created two tables and associated them. However before we can test it out, we have to learn how to **input** and **retrieve** data.

<a name="usingDatabases"></a>
##Using Databases
Before trying to use the controller to add entry to database, lets use the irb and learn now to **input** and **retrieve** data.

###Step 1
In iTerm, type ```rails c```

###Step 2
Try these few commands in sequence one by one and pay attention to the results like the format. Is it an array? is it a hash? or something else?

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
Post.find(3)
```

Notice the difference between **new** vs **create**?
**New** will make a new entry but **DOES NOT SAVE** to the database, while **create** will make a new entry and **SAVE** at the same time.

Both is fine but generally speaking, **new** is easier to customize when creating an API

###Step 3
Lets create something on the Comment table!
``` ruby
Comments.create(:post_id => 1, :content => "Reddit does not suck")
Comments.create(:post_id => 1, :content => "You are an idxxt")
```

Since we have associated both tables we can also do this!

But Please be careful of singular and pural. Remember "1 Post has many Comments" and "1 Comment belongs to 1 Post? Watch the first 2 command carfully!
``` ruby
Post.find(1).comments
Comment.find(1).post
#Look at the following two command, they are very different yet same results
Post.find(1).comments.create(:content => "You are the crazy one")
Comments.create(:post_id => 1, :content => "Both of you are crazy")
```

If you have noticed, we didn't need to specify **post_id**. This is because rails knows the association and did it for us.

###Step 4
We can now play with the database. Try these and pay attention to the results and once again pay attention to for format. Is it an array? Is it a hash? or something else?
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