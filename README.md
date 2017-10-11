## Setup

Ensure you have the following gems in your Rails `Gemfile`

```ruby
# Gemfile
gem 'bootstrap-sass'
gem 'font-awesome-sass'
gem 'simple_form'
gem 'autoprefixer-rails'
gem 'jquery-rails' # Add this line if you use Rails 5.1
```

In your terminal, generate SimpleForm Bootstrap config.

```bash
$ bundle install
$ rails generate simple_form:install --bootstrap
```

Then replace Rails' stylesheets by Le Wagon's stylesheets:

```
$ rm -rf app/assets/stylesheets
$ curl -L https://github.com/lewagon/stylesheets/archive/master.zip > stylesheets.zip
$ unzip stylesheets.zip -d app/assets && rm stylesheets.zip && mv app/assets/rails-stylesheets-master app/assets/stylesheets
```

Don't forget the sprockets directives in `assets/javascripts/application.js`

```javascript
// app/assets/javascripts/application.js

//= require jquery
//= require jquery_ujs
//= require bootstrap-sprockets
//= require_tree .
```

And the viewport in the layout

```html
<!-- app/views/layouts/application.html.erb -->
<head>
  ...
  <!-- Add these lines for detecting device width -->
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
</head>
```

## Adding new `.scss` files

Look at your main `application.scss` file to see how SCSS files are imported. There should **not** be a `*= require_tree .` line in the file.

```scss
// app/assets/stylesheets/application.scss

// Graphical variables
@import "config/fonts";
@import "config/colors";
@import "config/bootstrap_variables";

// External libraries
@import "bootstrap-sprockets";
@import "bootstrap";
@import "font-awesome-sprockets";
@import "font-awesome";

// Your CSS partials
@import "layouts/index";
@import "components/index";
@import "pages/index";
```

For every folder (**`components`**, **`layouts`**, **`pages`**), there is one `_index.scss` partial which is responsible for importing all the other partials of its folder.

**Example 1**: Let's say you add a new `_contact.scss` file in **`pages`** then modify `pages/_index.scss` as:

```scss
// pages/_index.scss
@import "home";
@import "contact";
```

**Example 2**: Let's say you add a new `_sidebar.scss` file in **`layouts`** then modify `layouts/_index.scss` as:

```scss
// layouts/_index.scss
@import "sidebar";
```

## Navbar template

Our `layouts/_navbar.scss` code works well with our home-made ERB template which you can find here:

- [version without login](https://github.com/lewagon/awesome-navbars/blob/master/templates/_navbar_wagon_without_login.html.erb).
- [version with login](https://github.com/lewagon/awesome-navbars/blob/master/templates/_navbar_wagon.html.erb).

Don't forget that `*.html.erb` files go in the `app/views` folder, and `*.scss` files go in the `app/assets/stylesheets` folder. Also, our navbar have a link to the `root_path`, so make sure that you have a `root to: "controller#action"` route in your `config/routes.rb` file.
