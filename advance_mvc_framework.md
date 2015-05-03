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
We will now create a simple models(**defines the association**) and migration(**defines the columns of table**) files.

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
Before trying to use the controller to add entry to database, lets use the irb and learn how to **input** and **retrieve** data.

###Step 1
In iTerm, type ```rails c```

###Step 2
Try these commands in sequence one by one and pay attention to the results like the format. Is it an array? is it a hash? or something else?

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

Both is fine but generally speaking, **new** is easier to use when creating an API

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

The things we did are very basic but very powerful. We will now proceed to using these skills in an actual **controller** to show information in the **view*

<a name="linkingAllIndex"></a>
##Linking It All Together - Index
Lets try to get data in controller and show it in view.

###Step 1
Everything starts with the route, we will now create an index page for posts.

Recall the advanced MVC framwork? This is where we are putting it to use.

In **routes.rb** add ```resources :posts```

This is very useful as this will help you create a RESTful api

This will give you is
``` ruby
#    Prefix Verb   URI Pattern               Controller#Action
#     posts GET    /posts(.:format)          posts#index
#           POST   /posts(.:format)          posts#create
#  new_post GET    /posts/new(.:format)      posts#new
# edit_post GET    /posts/:id/edit(.:format) posts#edit
#      post GET    /posts/:id(.:format)      posts#show
#           PATCH  /posts/:id(.:format)      posts#update
#           PUT    /posts/:id(.:format)      posts#update
#           DELETE /posts/:id(.:format)      posts#destroy
```
Does these look confusing? For now, lets ignore the Prefix column and focus on the rest. Previously when we did ```root "static_pages#index"```, we basically tell the route to redirectect any "http://localhost:3000/" to the **static_pages controller index action**. Now given the type of http request (get, post, put, delete), it will redirect us to the appropirate controller and action.

Another way, if you prefer, is typing all these out one by one like this
``` ruby
get '/posts', to: 'posts#index'
post '/posts', to: 'posts#create'
#and so on
````

###Step 2
In iTerm, create controller for **posts** ```rails g controller Posts```

###Step 3
In the controller we just created, add the **index action**
``` ruby
def index

end
```

###Step 4
Inside the **index action**, add
``` ruby
@posts = Post.all
```

Here we are trying to create a **Global** variable that can be access by the view. Remember, any variable with "@" infront of it can be access by the view. Also we are asking the action to retrieve **all the Post data**.

###Step 5
Create a file **index.html.erb** for **posts controller** in ```app > views > posts```

Remember, almost every action should have a **action.html.erb**

###Step 6
Edit the **index.html.erb** we created for **posts controller**
``` HTML
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

Here you can see something very similar to what we did in Basic of Ruby. Basically the code with the = sign will try to print out the variable specified while the code without the = sign will simply run the logic.

###Step 7
Start the server if you havn't already, then go to ```localhost:3000/posts```

Bravo! You have just used the whole MVC model!!!!

<a name="linkingAllShow"></a>
##Linking It All Together - Show

<a name="linkingAllNewCreate"></a>
##Linking It All Together - New&Create
Just now we have completed the index action, but there are a lot more we need to do. We will do the new&create.

###Step 1
Since we already used ```resources :posts``` we don't need to add stuff to the route file. However if you are interested here is the code
``` ruby
get '/posts/new', to: 'posts#new'
post '/posts', to: 'posts#create'
```

In rails, **new action** will should give you a template form to create a new post. Then the form will be submitted to **create action**

###Step 2
In the posts controller, add emtpy **new action**

In **new action** add ```@post = Post.new()```

Recall the difference between .new() and .create()? Basically what we want is to **save a template of an entry to @post and use it to create a form in view```

###Step 3
Create a view file call **new.html.erb** into the ```app > views > posts``` folder.

In the **new.html.erb** add
``` HTML
<%= form_for @person do |f| %>
  <%= f.label :title %>:
  <%= f.text_field :title %><br />

  <%= f.label :content %>:
  <%= f.text_field :content %><br />

  <%= f.submit %>
<% end %>

<!-- basically, the code above would get rendered and become the code below -->

<form action="/posts" class="new_post" id="new_post" method="post">
  <input name="authenticity_token" type="hidden" value="NrOp5bsjoLRuK8IW5+dQEYjKGUJDe7TQoZVvq95Wteg=" />
  <label for="post_title">Title</label>:
  <input id="post_title" name="post[title]" type="text" /><br />

  <label for="post_content">Content</label>:
  <input id="post_content" name="post[content]" type="text" /><br />

  <input name="commit" type="submit" value="Create Post" />
</form>
```

###Step 4
Now that we have the **new action** covered, we need the **create action**. 
In the posts controller, add empty **create action**

In **create action** add
``` Ruby
  post = Post.new(params[:post])
  if post.save
    #if saved
    redirect_to posts
  else
    #if failed
    redirect_to posts
  end
```

Recall this?
``` ruby
#    Prefix Verb   URI Pattern               Controller#Action
#     posts GET    /posts(.:format)          posts#index
#           POST   /posts(.:format)          posts#create
#  new_post GET    /posts/new(.:format)      posts#new
# edit_post GET    /posts/:id/edit(.:format) posts#edit
#      post GET    /posts/:id(.:format)      posts#show
#           PATCH  /posts/:id(.:format)      posts#update
#           PUT    /posts/:id(.:format)      posts#update
#           DELETE /posts/:id(.:format)      posts#destroy
```

Remember we ignore the prefix column? It is actually very useful because we can simply refer to posts, we can be **redireted back to '/posts'**

###Step 5
Lets try it 

<a name="linkingAllEdit"></a>
##Linking It All Together - Edit
