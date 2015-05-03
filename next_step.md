<a name="ajaxROR"></a>
##Using AJAX with ROR
Lets try to use AJAX to create a post!

###Step 1
As always, we start off with routes. However by using ```resources :post``` it have already generated the route for us.

**Challenge**
 - According to CRUD, which AJAX method should we use when creating a post?
 - With that specific AJAX method in mind, use the ```rake routes``` command to see what method you need to use/create.

###Step 2
We have the route, but we need something to send the AJAX request

**Challenge**
 - In the post **index.html.erb** create inputs for **title and content**
 - Create a button that sends the data through AJAX request

###Step 3
Now we need the controller to process the data we sent it and create a database entry with it.

When the controller receives the data, it will automatically put it in the **params** variable

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
## Installing Devise
### Adding Devise
1. add gem ‘devise’ in Gemfile
2. bundle install

### Install necessary configs for devise
3. rails generate devise:install 

### Generate the recipe, or the migration file for users
4. rails g devise user

### Customize our login views
5. rails g devise:views

### Cooking the recipe / running the migration
6. rake db:migrate
7. restart your server
8. go to http://localhost:3000/users/sign_in
9. go to http://localhost:3000/users/sign_up

###Challenge
 - Add associations to Post and Comment
 - Create a sign out button
