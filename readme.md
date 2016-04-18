# Pets CRUD App in Rails

## Steps to reproduce

### Step 1 - Generate The Project

1a. Use rails to generate a new project

```bash
rails new --database=postgresql --skip-test-unit rails-pets
cd rails-pets
rake db:create
```

1b. Save your work:

```bash
git init
git add -A
git commit -m "Created Rails project."
git tag step1
```

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

2b. Save your work:

```bash
bundle install
git add -A
git commit -m "Added gems."
git tag step2
```

### Step 3 - Create the `Owner` Model Class

3a. Use rails to generate an Owner model class:

```bash
rails g model Owner first_name:string last_name:string phone:string email:string
rake db:migrate
```

3b. Edit the file `app/models/owner.rb` and set the content to the following:

```ruby
class Owner < ActiveRecord::Base
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

```bash
rails c
> Owner.create({ first_name: 'Mike', last_name: 'Hopper', email: 'drmikeh@gmail.com' })
> Owner.all
> mike = Owner.find(1)
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

### Step 4 - Create RESTful Controller and Routes for Owners

4a. Use rails to generate a controller for Owners

```bash
rails generate controller Owners index show new edit
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

4c. Edit the file `app/controllers/owners_controller.rb` and set the content to the following:

```ruby
class OwnersController < ApplicationController
  before_action :set_owner, only: [:show, :edit, :update, :destroy]

  def index
    @owners = Owner.all
  end

  def show
  end

  def new
    @owner = Owner.new
  end

  def edit
  end

  def create
    @owner = Owner.new(owner_params)
    respond_to do |format|
      if @owner.save
        format.html { redirect_to @owner, notice: 'Owner was successfully created.' }
        format.json { render :show, status: :created, location: @owner }
      else
        format.html { render :new }
        format.json { render json: @owner.errors, status: :unprocessable_entity }
      end
    end
  end

  def update
    respond_to do |format|
      if @owner.update(owner_params)
        format.html { redirect_to @owner, notice: 'Owner was successfully updated.' }
        format.json { render :show, status: :ok, location: @owner }
      else
        format.html { render :edit }
        format.json { render json: @owner.errors, status: :unprocessable_entity }
      end
    end
  end

  def destroy
    @owner.destroy
    respond_to do |format|
      format.html { redirect_to owners_url, notice: 'Owner was successfully destroyed.' }
      format.json { head :no_content }
    end
  end

  private

  # Use callbacks to share common setup or constraints.
  def set_owner
    @owner = Owner.find(params[:id])
  end

  # Never trust parameters from the scary internet, only allow the white list through.
  def owner_params
    params.require(:owner).permit(:first_name, :last_name, :phone, :email)
  end
end
```

4d. Try looking at the routes:

* Run `rake routes`
* Alternatively, you can open your browser to `http://localhost:3000/rails/info/routes`

> Now let's add some content to the views:

4e. Edit `views/owners/index.html.erb` and set the content to the following:

```html
<p id="notice"><%= notice %></p>

<h1>Listing Owners</h1>

<table>
  <thead>
    <tr>
      <th>First name</th>
      <th>Last name</th>
      <th>Phone</th>
      <th>Email</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @owners.each do |owner| %>
      <tr>
        <td><%= owner.first_name %></td>
        <td><%= owner.last_name %></td>
        <td><%= owner.phone %></td>
        <td><%= owner.email %></td>
        <td><%= link_to 'Show', owner %></td>
        <td><%= link_to 'Edit', edit_owner_path(owner) %></td>
        <td><%= link_to 'Destroy', owner, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Owner', new_owner_path %>
```

4f. Edit the file `views/owners/show.html.erb` and set the content to the following:

```html
<p id="notice"><%= notice %></p>

<p>
  <strong>First name:</strong>
  <%= @owner.first_name %>
</p>

<p>
  <strong>Last name:</strong>
  <%= @owner.last_name %>
</p>

<p>
  <strong>Phone:</strong>
  <%= @owner.phone %>
</p>

<p>
  <strong>Email:</strong>
  <%= @owner.email %>
</p>

<%= link_to 'Edit', edit_owner_path(@owner) %> |
<%= link_to 'Back', owners_path %>
```

4g. Create the file `views/owners/_form.html.erb` and set the content to the following:

```html
<%= form_for(@owner) do |f| %>
  <% if @owner.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(@owner.errors.count, "error") %> prohibited this owner from being saved:</h2>

      <ul>
      <% @owner.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= f.label :first_name %><br>
    <%= f.text_field :first_name %>
  </div>
  <div class="field">
    <%= f.label :last_name %><br>
    <%= f.text_field :last_name %>
  </div>
  <div class="field">
    <%= f.label :phone %><br>
    <%= f.text_field :phone %>
  </div>
  <div class="field">
    <%= f.label :email %><br>
    <%= f.text_field :email %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %>
```

This file is a _partial_ that will be used in 2 of our views, the `new` and the `edit` views.

4h. Edit `views/owners/new.html.erb` and set the content to the following:

```html
<h1>New Owner</h1>

<%= render 'form' %>

<%= link_to 'Back', owners_path %>
```

4i. Edit `views/owners/edit.html.erb` and set the content to the following:

```html
<h1>Editing Owner</h1>

<%= render 'form' %>

<%= link_to 'Show', @owner %> |
<%= link_to 'Back', owners_path %>
```

4j. Test it all out.


4k. Save your work:

```bash
git add -A
git commit -m "Added MVC Crud for Owners."
git tag step4
```

### Step 5 - Repeat Step 4 but for Pets instead of Owners
