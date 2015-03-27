# fake\_stripe, a Stripe fake

This library is a way to test [Stripe](http://www.stripe.com/) code without hitting Stripe's
servers. It uses
[Capybara::Server](https://github.com/jnicklas/capybara/blob/master/lib/capybara/server.rb)
and [Webmock](https://github.com/bblimke/webmock) to intercept all of the calls from Stripe's
Ruby library and returns JSON that the Stripe library can parse.

## Installation

### Gemfile

Add the `fake_stripe` Gem to the `:test` group in your Gemfile:

```ruby
# Gemfile
group :test do
  gem 'fake_stripe'
end
```

Remember to run `bundle install`.

### Stripe settings

```ruby
# config/initilialzers/stripe.rb
Stripe.api_key = ENV['STRIPE_API_KEY']
```

Include the Stripe JavaScript in your application template:

```rhtml
# app/views/layouts/application.html.erb
<%= javascript_include_tag "https://js.stripe.com/v1/" %>
```

When the test suite runs `fake_stripe` will intercept requests to the Stripe
address and serve up a local version of [Stripe.js](https://stripe.com/docs/stripe.js).

### In Tests

Require the library in your spec support:

```ruby
# spec/support/fake_stripe.rb
require 'fake_stripe'

RSpec.configure do |config|
  config.before(:each) do
    FakeStripe.stub_stripe
  end
end
```

## Contributing

Please see [CONTRIBUTING.md][1] for more details.

[1]: https://github.com/thoughtbot/fake_stripe/blob/master/CONTRIBUTING.md
