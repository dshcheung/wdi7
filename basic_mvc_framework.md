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

add an **index method**
``` ruby
def index

end
```

###Step 5
Because we created an **index method** inside the controller, we need to created an html with the name **index.html.erb** under the folder with the same name as the controller
```
app > views > controller-name > method.html.erb
```

If we created **show method**, we need to created **show.html.erb**

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
