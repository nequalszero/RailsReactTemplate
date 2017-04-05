# React/Redux/Rails Template
## Summary
This project was setup using the project setup directions found below with some extra gems and packages included.  It handles basic user authentication and persists sessions in local storage.

## Directions for Running
1. Run `bundle install` to install the gems.
2. Run `rake db:setup` to setup the database.
3. Run `npm install` to install node modules.
4. Run `webpack` to generate bundle file.
5. If cloning repo for new project, remember to change the database names in `config/database.yml`.
6. Run `webpack --w` and `rails s` to start server and listen for changes.
7. Open application at `localhost:3000`

## Notes for testing
Run tests with `bundle exec rspec`. There is currently some error that occurs when running all the tests together, that seems be caused by the `DatabaseCleaner.clean` command in each spec file.  Running the test suites individually allows them all to pass.  This may be due to an incorrect setup on my part...

## Project Setup Checklist
Here's a Rails/React/Redux setup checklist.

* [ ] `rails new`
  * Add `--database=postgresql` if using Postgres.
  * Add `--skip-turbolinks` to skip the turbolinks gem.
* [ ] Update your `Gemfile`.
  * `better_errors`
  * `binding_of_caller`
  * `pry-rails`
  * `annotate`
* [ ] `bundle install`
* [ ] `git init`
  * Update your `.gitignore`.
    * `node_modules/`
    * `bundle.js`
    * `bundle.js.map`
* [ ] `git remote add` the proper remote.
  * `git push -u` the remote.
* [ ] `npm init --yes` to create a package.json file with the default setup.
* [ ] Create a frontend folder at the root of your project with an entry file inside of it.
* [ ] `npm install --save`
  * `webpack`
  * `react`
  * `react-dom`
  * `react-router` <-- might not be needed if using history and react-router-dom
  * `redux`
  * `react-redux`
  * `babel-core`
  * `babel-loader`
  * `babel-preset-react`
  * `babel-preset-es2015`
  * `babel-preset-stage-1`
  * `react-modal`
  * `classnames`
  * `react-loading-animation`
  * `react-router-dom`
  * `redux-thunk`
  * `history`
  * `lodash`
  * `redux-form` <-- optional
* [ ] Create a `webpack.config.js` file.
  * The entry point should be in frontend, e.g. `entry: 'frontend/index.jsx'`.
  * The output path should be `'app/assets/javascripts'`.
  * Configure your `module.loaders` to use Babel transpilation for:
    * JSX
    * ES6
  * Include `devtool: 'source-map'`.
* [ ] `git commit` again (Verify that your `.gitignore` works).
  * `git push` your skeleton.


## Rails Setup

There are several things you **must** do when beginning every Rails
app for the near future.

First off, check your rails version. In this course, we will be
using Rails `~>4.2.0` (greater than or equal to 4.2.0 but less than
4.3). Check to see if you have a proper version by running:

```
rails -v
```

If you don't have a new enough version of Rails, run:

```
gem install rails -v '~> 4.2.0'
```

Now that you have Rails installed and ready, start your project
with the following command:

```
rails new MyProjectName --database=postgresql --skip-turbolinks
```

If you have Rails 5 installed, you can start a Rails 4 project
using the following command:

```
rails _4.2.7_ new MyProjectName --database=postgresql --skip-turbolinks
```

### Use Common Debugging Gems

You want to use `better_errors` and `binding_of_caller`, which will
make it much easier to see what is going on in your Rails app. You
also want to use `pry-rails`, which will provide a nicer console than
IRB when you run `rails console`. Lastly, you can use the
`quiet_assets` gem, which will reduce excessive logging of requests
for CSS/JavaScript files, making it easier for you to read your Rails
logs:

```ruby
# Add to Gemfile
group :development do
  gem 'better_errors'
  gem 'binding_of_caller'
  gem 'pry-rails'
  gem 'quiet_assets'
end
```

**Note:** You should always place these gems in the `development` group. Rails
supports three environments by default: development, test, and production.
Development is what it runs in on your localhost. Test is what it runs in when
you run RSpec tests. Production is what it will run in on Heroku or other hosts
that make your app available to the Internet. You *never* want gems like
`better_errors` or `binding_of_caller` running in production. In addition to
loading unnecessary code, these gems will constitute a huge security hole if
their features are exposed to the Internet. So whenever you install a new gem,
think carefully about which environment(s) it is applicable to.

### Setting up rspec

1. Add the following gems to the `Gemfile`

  ```ruby
  group :development, :test do
    # Call 'byebug' anywhere in the code to stop execution and get a debugger console
    gem 'byebug'
    gem 'rspec-rails'
  end

  group :test do
    gem 'factory_girl_rails'
    gem 'faker'
    gem 'guard-rspec'
    gem 'launchy'
    gem 'shoulda-matchers', '~> 3.0'
    gem 'database_cleaner'
  end
  ```
2. Run `rails g rspec:install`
3. Add the following to your `config/application.rb` to use `rspec` and `factory_girl` for testing
  ```ruby
  config.generators do |g|
    g.test_framework :rspec,
      :fixtures => false,
      :view_specs => false,
      :helper_specs => false,
      :routing_specs => false,
      :controller_specs => true,
      :request_specs => false
    g.fixture_replacement :factory_girl, :dir => "spec/factories"
  end
  ```

4. Uncomment this line in `spec/rails_helper.rb`
  ```ruby
  Dir[Rails.root.join('spec/support/**/*.rb')].each { |f| require f }
  ```

5. To use the `database_cleaner` gem, add the following lines to the top of `spec/rails_helper.rb`
  ```ruby
  require 'database_cleaner'

  DatabaseCleaner.strategy = :truncation
  ```

6. Add the following to `spec/rails_helper.rb` inside the `Rails.configure` block to use `shoulda-matchers` in your tests:
  ```ruby
  Shoulda::Matchers.configure do |config|
    config.integrate do |with|
      with.test_framework :rspec
      with.library :rails
    end
  end
  ```

7. Add the following to the end of `spec/rails_helper.rb` inside the `Rails.configure` block to use `shoulda-matchers` in your tests:
  ```ruby
  RSpec.configure do |config|
    config.include FactoryGirl::Syntax::Methods
  end
  ```

8. Check the options being provided in `.rspec`. You may want to add `--format documentation`, it likely will already have `--color` and `--require spec_helper`

  ```ruby
  --color
  --require spec_helper
  --format documentation
  ```
