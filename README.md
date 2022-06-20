First of all make sure you've created a rails app

```bash
rails new APP_NAME
```

## Setup

Ensure you have Bootstrap:

```ruby
# Gemfile
gem "bootstrap"
```

```bash
bundle install
```

Ensure you have the following gems in your Rails `Gemfile`:

```ruby
# Uncomment this gem already present in your Gemfile
gem "sassc-rails"

# Add those ones
gem "autoprefixer-rails"
gem "font-awesome-sass", "~> 6.1"
gem "simple_form", github: "heartcombo/simple_form"
```

⚠ To this day (March, 9th, 2022), Simple Form support of Bootstrap 5 has been merged in `main` but has not been released yet. To use a version of Simple Form which supports Bootstrap 5, we need to install the gem from GitHub and we've added the specific `components/_form_legend_clear.scss` partial in our stylesheets.

In your terminal, generate Simple Form Bootstrap config:

```bash
bundle install
rails generate simple_form:install --bootstrap
```

Then replace Rails' stylesheets by Le Wagon's stylesheets:

```bash
rm -rf app/assets/stylesheets
curl -L https://github.com/lewagon/stylesheets/archive/master.zip > stylesheets.zip
unzip stylesheets.zip -d app/assets && rm stylesheets.zip && mv app/assets/rails-stylesheets-master app/assets/stylesheets
```

**On Ubuntu/Windows**: if the `unzip` command returns an error, please install it first by running `sudo apt install unzip`.

Note that when you update the colors in `config/colors`, the (text) color of your buttons might change from white to black. This is done automatically by Bootstrap using the [WCAG 2.0 algorithm](https://getbootstrap.com/docs/5.1/customize/sass/#color-contrast) which makes sure that the contrast between the text and the background color meets accessibility standards.

## Bootstrap JS

Add Bootstrap in importmaps:

```ruby
# config/importmap.rb
pin "jquery", to: "https://ga.jspm.io/npm:jquery@3.6.0/dist/jquery.js"
pin "bootstrap", to: "https://ga.jspm.io/npm:bootstrap@5.1.3/dist/js/bootstrap.esm.js"
pin "@popperjs/core", to: "https://ga.jspm.io/npm:@popperjs/core@2.11.5/lib/index.js"
```

```js
// app/javascript/packs/application.js
import "bootstrap"
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
@import "bootstrap";
@import "font-awesome";

// Your CSS partials
@import "components/index";
@import "pages/index";
```

For every folder (**`components`**, **`pages`**), there is one `_index.scss` partial which is responsible for importing all the other partials of its folder.

**Example 1**: Let's say you add a new `_contact.scss` file in **`pages`** then modify `pages/_index.scss` as:

```scss
// pages/_index.scss
@import "home";
@import "contact";
```

**Example 2**: Let's say you add a new `_card.scss` file in **`components`** then modify `components/_index.scss` as:

```scss
// components/_index.scss
@import "card";
```

## Navbar template

Our `layouts/_navbar.scss` code works well with our home-made ERB template which you can find here:

- [version without login](https://github.com/lewagon/awesome-navbars/blob/master/templates/_navbar_wagon_without_login.html.erb)
- [version with login](https://github.com/lewagon/awesome-navbars/blob/master/templates/_navbar_wagon.html.erb)

Don't forget that `*.html.erb` files go in the `app/views` folder, and `*.scss` files go in the `app/assets/stylesheets` folder. Also, our navbars have a link to the `root_path`, so make sure that you have a `root to: "controller#action"` route in your `config/routes.rb` file.
