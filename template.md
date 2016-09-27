# Pets CRUD App in Rails

{{ TOC }}

## Steps to reproduce

### Step 0 - Install Rails

```bash
rbenv versions  # check to see if you are on a recent version of Ruby
                # * 2.2.4 (set by /Users/drmikeh/.rbenv/version)

which gem       # check your path for the correct gem installer
                # /Users/drmikeh/.rbenv/shims/gem

gem install rails  # install the latest stable version of Rails

rails -v        # check the version of Rails just installed
                # Rails 5.0.0.1
```

---

### Step 1 - Generate The Project

1a. Use rails to generate a new project

```bash
rails new --database=postgresql --skip-test rails-pets
cd rails-pets
rake db:create            # This file creates the databases (dev and test)
```

1b. Save your work:

```bash
git init
git add -A
git commit -m "Created Rails project."
git tag step1
```

---

### Step 2 - Add Gems

2a. Edit Gemfile and add the following gems:

```ruby
group :development, :test do
  # Use pry with Rails console
  gem 'pry-rails'

  # Better Rails Error Pages
  gem 'better_errors'
end
```

2b. Run Bundler

```bash
bundle install
```

2c. Test it out

```bash
rails s
```

Point your browser to `localhost:3000` to see the Rails Welcome page.

2d. Save your work:

```bash
git add -A
git commit -m "Added gems."
git tag step2
```

---

### Step 3 - Create the `Owner` Model Class

3a. Use rails to generate an `Owner` model class:

We can use the `rails g` command to generate new files for our Rails project!
Let's create a new _model_ class using `rails g`:

```bash
rails g model Owner first_name:string last_name:string phone:string email:string
```

This command created 2 files for us:

* `app/models/owner.rb`                        - the Owner model class
* `db/migrate/20160925230742_create_owners.rb` - a migration script for updating our DB schema

After generating a new _model_ class, we need to update our DB with the new schema required for our new model:

```bash
rake db:migrate
```

Since this is the first time we have needed a migration, this command created the file: `db/schema.rb`.

3b. Edit the file `app/models/owner.rb` and set the content to the following:

```ruby
class Owner < ApplicationRecord
  validates :first_name, :last_name, :email, presence: true

  def full_name
    "#{first_name} #{last_name}"
  end

  def to_s
    full_name
  end
end
```

3c. Use `rails console` to create some Owners:

Rails has a `console` mode that is a REPL that we can use to test out our code. Try the following:

```bash
rails c
> Owner.create({ first_name: 'Mike', last_name: 'Hopper', email: 'drmikeh@gmail.com' })
> Owner.all
> mike = Owner.find(1)
> mike.first_name = 'Michael'
> mike.phone = '678-123-4567'
> mike.valid?
> mike.save!
> Owner.all
> quit
```

3d. Save your work:

```bash
git add -A
git commit -m "Created the Owner model."
git tag step3
```

---

### Step 4 - Create a RESTful Controller and the RESTful Routes for Owners

4a. Use rails to generate a controller for Owners

We can also create _controllers_ using the `rails g` command:

```bash
rails g controller Owners index show new edit
```

Whoa, that command created a _lot_ of files:

```
      create  app/controllers/owners_controller.rb     # our new controller class
       route  get 'owners/edit'                        # 4 routes added to routes.rb
       route  get 'owners/new'
       route  get 'owners/show'
       route  get 'owners/index'
      invoke  erb
      create    app/views/owners                       # 1 folder and 4 files for our views
      create    app/views/owners/index.html.erb
      create    app/views/owners/show.html.erb
      create    app/views/owners/new.html.erb
      create    app/views/owners/edit.html.erb
      invoke  helper                                   # a helper module for Owners
      create    app/helpers/owners_helper.rb
      invoke  assets                                   # client-side stubs for our Owner views
      invoke    coffee
      create      app/assets/javascripts/owners.coffee
      invoke    scss
      create      app/assets/stylesheets/owners.scss
```

4b. Edit the file `app/config/routes.rb` and replace the lines:

```ruby
get 'owners/index'
get 'owners/show'
get 'owners/new'
get 'owners/edit'
```

with

```ruby
resources :owners
```

That 1 line of code will define the standard 7 RESTful routes for our _owners_ resource!

4c. Try looking at the routes:

To see all of your routes, you can run `rake routes`:

```bash
rake routes
```

which produces the following output:

```
    Prefix Verb   URI Pattern                Controller#Action
    owners GET    /owners(.:format)          owners#index
           POST   /owners(.:format)          owners#create
 new_owner GET    /owners/new(.:format)      owners#new
edit_owner GET    /owners/:id/edit(.:format) owners#edit
     owner GET    /owners/:id(.:format)      owners#show
           PATCH  /owners/:id(.:format)      owners#update
           PUT    /owners/:id(.:format)      owners#update
           DELETE /owners/:id(.:format)      owners#destroy
```

> NOTE: you can also run your app and open your browser to `http://localhost:3000/rails/info/routes` to see all of the routes.


4d. Edit the file `app/controllers/owners_controller.rb` and set the content to the following:

```ruby
{{ rails-pets/app/controllers/owners_controller.rb }}
```

> Now let's add some content to the views:

4e. Edit `app/views/owners/index.html.erb` and set the content to the following:

```html
{{ rails-pets/app/views/owners/index.html.erb }}
```

4f. Edit the file `app/views/owners/show.html.erb` and set the content to the following:

```html
{{ rails-pets/app/views/owners/show.html.erb }}
```

4g. Create the file `app/views/owners/_form.html.erb` and set the content to the following:

```bash
touch app/views/owners/_form.html.erb
```

```html
{{ rails-pets/app/views/owners/_form.html.erb }}
```

This file is a _partial_ that will be used in 2 of our views, the `new` and the `edit` views.

> Partials are great for keeping our view code DRY!

4h. Edit `app/views/owners/new.html.erb` and set the content to the following:

```html
{{ rails-pets/app/views/owners/new.html.erb }}
```

4i. Edit `app/views/owners/edit.html.erb` and set the content to the following:

```html
{{ rails-pets/app/views/owners/edit.html.erb }}
```

4j. Test it all out.

```bash
rails s
```

Open your browser to `localhost:3000` and try out all of the RESTful endpoints and do all of the _CRUD_ operations.

4k. Save your work:

```bash
git add -A
git commit -m "Added MVC Crud for Owners."
git tag step4
```

---

> Now we want to create a model class, a controller class, the 7 RESTful routes, and the 4 views for a _Pet_. We also want each Pet to have an owner (a `belongs_to` relationship) and add the `has_many` relationship to the _Owner_ model class.

---

### Step 5 - Create the `Pet` Model Class

5a. Use rails to generate an `Pet` model class:

```bash
rails g model Pet name:string age:integer owner:references
```

This command created 2 files for us:

* `app/models/pet.rb`                          - the Pet model class
* `db/migrate/20160926000836_create_pets.rb`   - a migration script for updating our DB schema

After generating a new _model_ class, we need to update our DB with the new schema required for our new model:

```bash
rake db:migrate
```

Note that this command will _both_ update our DB schema and update the file: `db/schema.rb`.

Inspect the file `db/schema.rb` to see how your DB schema looks.

Also inspect the `app/models/pet.rb` file and notice that the generated model class contains the `belongs_to :owner` association. Pretty cool!!!


5b. Edit the file `app/models/pet.rb` and add the following:

```ruby
  def to_s
    "Pet #{name} is #{age} years old and belongs to #{owner.full_name}."
  end
```

5c. Edit the file `app/models/owner.rb` and add as `has_many` association to the Pet model:

```ruby
class Owner < ApplicationRecord
  has_many :pets
```

5d. Use `rails console` to create some Pets:

```bash
rails c
> mike = Owner.first
> miko = Pet.create!({ name: 'Miko', age: 4, owner: mike })
> meisha = Pet.create!({ name: 'Meisha', age: 3, owner: mike })
> Pet.all
> Owner.first.pets
> quit
```

5e. Save your work:

```bash
git add -A
git commit -m "Created the Pet model."
git tag step5
```

---

### Step 6 - Create a RESTful Controller and the RESTful Routes for Pets

6a. Use rails to generate a controller for Pets

```bash
rails g controller Pets index show new edit
```

6b. Edit the file `app/config/routes.rb` and replace the lines:

```ruby
get 'pets/index'
get 'pets/show'
get 'pets/new'
get 'pets/edit'
```

with

```ruby
resources :pets
```

or you can even combine the resources lines:

```ruby
resources :owners, :pets
```

6c. Inspect the routes:

```bash
rake routes
```

which should produce the following output:

```
    Prefix Verb   URI Pattern                Controller#Action
    owners GET    /owners(.:format)          owners#index
           POST   /owners(.:format)          owners#create
 new_owner GET    /owners/new(.:format)      owners#new
edit_owner GET    /owners/:id/edit(.:format) owners#edit
     owner GET    /owners/:id(.:format)      owners#show
           PATCH  /owners/:id(.:format)      owners#update
           PUT    /owners/:id(.:format)      owners#update
           DELETE /owners/:id(.:format)      owners#destroy
      pets GET    /pets(.:format)            pets#index
           POST   /pets(.:format)            pets#create
   new_pet GET    /pets/new(.:format)        pets#new
  edit_pet GET    /pets/:id/edit(.:format)   pets#edit
       pet GET    /pets/:id(.:format)        pets#show
           PATCH  /pets/:id(.:format)        pets#update
           PUT    /pets/:id(.:format)        pets#update
           DELETE /pets/:id(.:format)        pets#destroy
```

6d. Edit the file `app/controllers/pets_controller.rb` and set the content to the following:

```ruby
{{ rails-pets/app/controllers/pets_controller.rb }}
```

> Now let's add some content to the views:

6e. Edit `app/views/pets/index.html.erb` and set the content to the following:

```html
{{ rails-pets/app/views/pets/index.html.erb }}
```

6f. Edit the file `views/pets/show.html.erb` and set the content to the following:

```html
{{ rails-pets/app/views/pets/show.html.erb }}
```

6g. Create the file `app/views/pets/_form.html.erb` and set the content to the following:

```bash
touch app/views/pets/_form.html.erb
```

```html
{{ rails-pets/app/views/pets/_form.html.erb }}
```

6h. Edit `app/views/pets/new.html.erb` and set the content to the following:

```html
{{ rails-pets/app/views/pets/new.html.erb }}
```

6i. Edit `app/views/pets/edit.html.erb` and set the content to the following:

```html
{{ rails-pets/app/views/pets/edit.html.erb }}
```

6j. Test it all out.

```bash
rails s
```

Open your browser to `localhost:3000` and try out all of the RESTful endpoints and do all of the _CRUD_ operations.

6k. Save your work:

```bash
git add -A
git commit -m "Added MVC Crud for Pets."
git tag step6
```

### Step 7 - Add a NavBar and Some Style

7a. Add a NavBar

Edit the file `app/views/layouts/application.html.erb` and set the `body` to the following:

```html
  <body>
    <nav>
      <ul>
        <li><%= link_to 'Owners', owners_path %></li>
        <li><%= link_to 'Pets', pets_path %></li>
      </ul>
    </nav>
    <%= yield %>
  </body>
```

7b. Add some CSS

Edit the file `app/assets/stylesheets/application.css` and add the following at the end of the file:

```css
 body {
  background-color: #def;
  margin-left: 60px;
 }

nav ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

nav li {
  display: inline;
}

nav a {
  text-decoration: none;
  font-size: 1.4em;
}
```

7c. Save your work:

```bash
git add -A
git commit -m "Added NavBar and CSS"
git tag step7
```

## Summary

We have created a Rails app using the following commands:

* rails new
* rake db:create
* rails g model (rails generate model)
* rake db:migrate
* rails g controller (rails generate controller)
* rake routes
* rails server (rails s)
* rails console (rails c)

