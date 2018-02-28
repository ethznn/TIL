## An example of typical use

### Very good and detailed example and reasons to use is considered in [railscast](http://railscasts.com/episodes/324-passing-data-to-javascript) by Ryan Bates

When you need to send some start data from your controller to your js you might be doing something like this:

1. Write this data in controller(presenter/model) to some variable
2. In view for this action you put this variable to some objects by data attributes, or write js right in view
3. Then there can be two ways in js:

- if you previously wrote data in data attributes - you should parse this attributes and write data to some js variable.
- if you wrote js right in view (many frontenders would shame you for that) - you just use data from this js - OK.

1. You can use your data in your js

And every time when you need to send some data from action to js you do this.

With gon you configure it firstly - just put in layout one tag, and add gem line to your Gemfile and do the following:

1. Write variables by

```ruby
 gon.variable_name = variable_value

 # or new syntax
 gon.push({
   :user_id => 1,
   :user_role => "admin"
 })

 gon.push(any_object) # any_object with respond_to? :each_pair
```

1. In your js you get this by

```javascript
 gon.variable_name
```

1. profit?

With the `gon.watch` feature you can easily renew data in gon variables! Simply call `gon.watch` from your js file. It's super useful in modern web applications!

## Usage

### More details about configuration and usage you can find in [gon wiki](https://github.com/gazay/gon/wiki)

`app/views/layouts/application.html.erb`

```html
<head>
  <title>some title</title>
  <%= Gon::Base.render_data %>
  <!-- include your action js code -->
  ...
```

You can pass some [options](https://github.com/gazay/gon/wiki/Options) to `render_data` method.

You put something like this in the action of your controller:

```ruby
@your_int = 123
@your_array = [1,2]
@your_hash = {'a' => 1, 'b' => 2}
gon.your_int = @your_int
gon.your_other_int = 345 + gon.your_int
gon.your_array = @your_array
gon.your_array << gon.your_int
gon.your_hash = @your_hash

gon.all_variables # > {:your_int => 123, :your_other_int => 468, :your_array => [1, 2, 123], :your_hash => {'a' => 1, 'b' => 2}}
gon.your_array # > [1, 2, 123]

# gon.clear # gon.all_variables now is {}
```

Access the variables from your JavaScript file:

```javascript
alert(gon.your_int)
alert(gon.your_other_int)
alert(gon.your_array)
alert(gon.your_hash)
```

