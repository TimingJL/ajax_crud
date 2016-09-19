# Ajax Crud
![Ubuntu version](https://img.shields.io/badge/Ubuntu-16.04%20LTS-orange.svg)
![Rails version](https://img.shields.io/badge/Rails-v5.0.0-blue.svg)
![Ruby version](https://img.shields.io/badge/Ruby-v2.3.1p112-red.svg)

This is the little practice about how to build dynamic application using the built-in AJAX functionality of Rails.

![image](https://github.com/TimingJL/ajax_crud/blob/master/pic/index.jpeg)
![image](https://github.com/TimingJL/ajax_crud/blob/master/pic/new_post.jpeg)

# Create a App
```console
$ rails new rails_ajax
```
# Add therubyracer rubygems
In Gemfile
```console
gem 'therubyracer'
```
Note: 
Because there is no Javascript interpreter for Rails on Ubuntu Operation System, we have to install `Node.js` or `therubyracer` to get the Javascript interpreter.


# Using Scaffold
```console
$ rails g scaffold Post title:stirng description:text
$ rake db:migrate
```

# Setting some routes
In `config/routes.rb`
```ruby
Rails.application.routes.draw do
	resources :posts
	root "posts#index"
end
```

# Styling
Let's poste these links from the bootstrap.          
http://getbootstrap.com/getting-started/       

In `app/views/layouts/application.html.erb`
```html

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>RailsAjax</title>
	    <%= csrf_meta_tags %>

	    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
	    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>

		<!-- Latest compiled and minified CSS -->
		<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

		<!-- Optional theme -->
		<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" integrity="sha384-rHyoN1iRsVXV4nD0JutlnGaslCJuC7uwjduW9SVrLvRYooPp2bWYgmgJQIXwl/Sp" crossorigin="anonymous">

		<!-- Latest compiled and minified JavaScript -->
		<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
	  </head>

	  <body>
	    <%= yield %>
	  </body>
	</html>
```

Then we're going to have a button and a modal.         
http://getbootstrap.com/javascript/#modals         

So we create a partial in `app/views/posts` and name it `_post.html.erb`

In `app/views/posts/index.html.erb`
```html

	<div class="container">

	    <div class="row">
	      <div class="text-center">
	          <!-- Button trigger modal -->
	          <button type="button" class="btn btn-primary btn-lg" data-toggle="modal" data-target="#mynewpost">
	            New Post
	          </button>
	      </div>
	    </div>

	    <br>
	    <hr>

	    <div class="row" id="container_posts">
	        <%= render @posts %>
	    </div>


	    <!-- Modal  create action -->
	      <%= form_for(@post, remote: true) do |f| %>
	        <div class="modal fade" id="mynewpost" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
	        <div class="modal-dialog">
	          <div class="modal-content">

	            <div class="modal-header">
	              <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
	              <h4 class="modal-title" id="myModalLabel">Post</h4>
	            </div>
	            
	            <div class="modal-body">
	              <div class="field">
	                <%= f.label :title %><br>
	                <%= f.text_area :title, class: "form-control post_title" %>
	              </div>
	              <div class="field">
	                <%= f.label :description %><br>
	                <%= f.text_area :description, class: "form-control post_content" %>
	              </div>
	            </div>

	            <div class="modal-footer">
	              <button type="button" class="btn btn-default" data-dismiss="modal" id="mynewpostclose">Close</button>
	              <%= submit_tag "Create", class: "btn btn-primary" %>
	            </div>
	          </div>
	        </div>
	        </div>
	      <% end %>
	    <!-- Modal -->



	</div>
```


In `app/views/posts/_post.html.erb`
```html

	<div class="col-md-4 blog-main box text-center" id="post_<%= post.id %>">

	            <div class="panel panel-info"><!-- Panel Info -->

	                  <div class="panel-heading">
	                    <h3 class="panel-title"><%= post.title %></h3>
	                    <p><%= post.description %></p>
	                  </div>

	                  <div class="panel-body"><!-- Panel Body -->
	                    <div style="color: #3BB2D6;"><%= time_ago_in_words(post.created_at) %></div>
	                    <br>

	                    <div class="btn-group" role="group" aria-label="...">
	                        <button type="button" class="btn btn-warning" data-toggle="modal" data-target="#myupdatepost_<%= post.id %>">
	                          Edit
	                        </button>
	                        <%= link_to 'Destroy', post, method: :delete, class: "btn btn-danger", remote: true %>
	                    </div>
	                  </div><!-- Panel Body -->

	            </div><!-- Panel Info -->


	            <!-- Modal - update posts -->
	                  <%= form_for(post, :method => :put, remote: true) do |f| %>
	                  <div class="modal fade" id="myupdatepost_<%= post.id %>" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
	                  <div class="modal-dialog">
	                    <div class="modal-content">
	                      <div class="modal-header">
	                        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
	                        <h4 class="modal-title" id="myModalLabel">Edit</h4>
	                      </div>
	                      <div class="modal-body">

	                      <div class="field">
	                        <%= f.label :title %><br>
	                        <%= f.text_area :title, class: "form-control" %>
	                      </div>
	                      <div class="field">
	                        <%= f.label :description %><br>
	                        <%= f.text_area :description, class: "form-control" %>
	                      </div>

	                      </div>
	                      <div class="modal-footer">
	                        <button type="button" id="myupdatebutton_<%= post.id %>" class="btn btn-default" data-dismiss="modal">Close</button>
	                        <%= submit_tag "Update", class: "btn btn-primary" %>
	                      </div>
	                    </div>
	                  </div>
	                  </div>
	                  <% end %>
	            <!-- Modal -->


	</div>
```


And in `app/controllers/posts_controller.rb`
```ruby
def index
	@posts = Post.all
	@post = Post.new
end
```

![image](https://github.com/TimingJL/ajax_crud/blob/master/pic/index.jpeg)
![image](https://github.com/TimingJL/ajax_crud/blob/master/pic/new_post.jpeg)
![image](https://github.com/TimingJL/ajax_crud/blob/master/pic/show.jpeg)

# Using Ajax to create post
First, we need to check whether `remote: true` is in `<%= form_for(@post, remote: true) do |f| %>` in `app/views/posts/index.html.erb`.

Then, we need to tell the controller that format has js.        

In `app/controllers/posts_controller.rb`, we add `format.js`
```ruby
  def create
    @post = Post.new(post_params)

    respond_to do |format|
      if @post.save
        format.html { redirect_to @post, notice: 'Post was successfully created.' }
        format.json { render :show, status: :created, location: @post }
        format.js
      else
        format.html { render :new }
        format.json { render json: @post.errors, status: :unprocessable_entity }
        format.js
      end
    end
  end
```

Next, we create a new file with the same name of the create action `create.js.erb` in `app/views/posts`. And in this file, we're gonna write some javascript to actually tell the browser what to do with a page.

In `app/views/posts/create.js.erb`
```js
$("#mynewpost").modal('hide');
$(".post_title").val('');
$(".post_content").val('');

$("#container_posts").prepend('<%= j render @post %>');
$("#post_<%= @post.id %>").hide().fadeIn(1000);
```
![image](https://github.com/TimingJL/ajax_crud/blob/master/pic/ajax_post.jpeg)

# Reference
#### Codeplace | How to use the built-in AJAX functionality of Ruby on Rails
https://www.youtube.com/watch?v=2Il7PPhen3o         

#### Source code
https://github.com/stukio/ajax_crud