---
layout: post
title:      "Adding a simple search; wasn't so simple for this noob."
date:       2020-09-06 22:10:16 +0000
permalink:  adding_a_simple_search_wasnt_so_simple_for_this_noob
---


I have a "pizza building app". You can login, build a pizza, edit it, delete it, or save it. The entire app is built around the currnet user.

I was challenged to add a simple search function. I just want to search by pizza size to prove that the code works. This has proven to be more challenging than I thought. 

The idea is a logged in customer can enter an attribute of a pizza (in this case the size) into a search bar on his own view page and return only the pizzas from that customer's history  matching the search params. This is how things progressed.

This is the pizzas table:
```
create_table "pizzas", force: :cascade do |t|
    t.string "size"
    t.string "crust"
    t.string "cheese"
    t.integer "customer_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end	
```
It has attributes of "size", "crust", and "cheese". Toppings is a seperate table.
The first thing I did was to add the search bar to my `views/customer show.html.erb` file.
```
<%= form_tag customer_pizzas_path(current_customer.id), method: :get do %>
    Search your past creations by size, cheese, or crust: <br> 
    <%= text_field_tag :search, params[:search]%>
    <%= submit_tag "Search", name: nil %>
<% end %><br>
```

Let's disect this line by line:
```
<%= form_tag customer_pizzas_path(current_customer.id), method: :get do %>
```
 The fist part, "form_tag", is our helper method provided by Rails. It "starts a form tag that points the action to a url...". The url is `customer_pizzas_path` or `/customers/:id/pizzas` which is our pizza index. And `(current_customer.id)` is the argument saying which list of pizzas to look for - the current customer's. By default the `form_tag` is a `post_method`. Because we are asking to see data rather than adding data, we want to specify `method: :get` then enter the `do-block`.
```
Search your past creations by size, cheese, or crust: <br> 
```
Is a simple instructional message. `<br>` is a line break for aesthetics.
```
<%= text_field_tag :search, params[:search]%>
```
`text_field_tag` will "...generate just a single <input> element. The first parameter...is always the name of the input (`:search`). When the form is submitted, the name will be passed along with the form data, and will make its way to the params in the controller with the value entered by the user for that field.
```
<%= submit_tag "Search", name: nil %>
```
And this creates the actual button you click. We add `name: nil` clean up the url. Without it we get something like `/customers/1/pizzas?search=small&commit=Search` everything after the word `small` is unsightly and unneccessary. Adding `name: nil` gets rid of that.

And that's it for rendering the actual input field on the user's show page.

Next, the `:search` part of our code.

Because we are searching the database of pizzas we need to create a class method on the `pizza.rb` model.

```
    def self.search(search)
        where(" (size) LIKE :search OR (crust) LIKE :search OR 
        (cheese) LIKE :search ", search: "%#{search}%")
    end
```
Here we define the class method `def self.search(search)`. Our method takes in an argument of `search` *(here `search` is just an arbitrary placeholder. you can call it anything here. in practice this will be our size - `small, medium, or large.) 

```
 where(" (size) LIKE :search OR (crust) LIKE :search OR 
        (cheese) LIKE :search ", search: "%#{search}%")
```
Here we perform a SQL query. We are asking the database to return all the records `where` the pizza's attribute (size, crust, or chesse (let's search by a couple params)) is `LIKE`  what is passed into `:search`. That data is stored in a hash `%#{search}%` and passed to the `PizzaController`'s `index` action.

Then of course we close the method with `end`.

Now that we have a form that takes in an input value, and a method that takes that value and looks through the database for records that match, we need to actually put it all to use in the `PizzaController`.

Let's define the `/pizza/index` action like this:
```
    def index        
        @pizzas = current_customer.pizzas.search(params[:search])
    end
```


Here `@pizzas` is being set to a minimum of `current_customer.pizzas`. All of a customer's past pizzas. If `.search(params[:search]` doesn't return anything or nothing is entered in the search field, because there is(are) no specific record(s) to return, `where` in the `:search` method defaults to returning all records.

So, as an example:
If a customer navigates to `customers/:id` and enters `small` in the search field, the `self.search(search)` method accepts the passed in param, querries the database for records matching the search param, if records are found they are carried to the customer's pizza index at `/customers/:id/pizzas`. If no records are found then all records are delivered to `/customers/:id/pizzas` and will be displayed to the user at `customers/:id/pizzas?search=large` through the `views/pizza index.html.erb` file. 

I hope this was helpful. And as always,

Code you imagination to life.

reference materials:
https://apidock.com/rails/ActionView/Helpers/FormTagHelper/form_tag;
https://guides.rubyonrails.org/form_helpers.html;
https://medium.com/@zylberberg.jonathan/creating-a-search-form-in-rails-5-77fdef6be74d




