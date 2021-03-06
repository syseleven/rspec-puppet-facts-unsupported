# RspecPuppetFactsUnsupported

[![Build Status](https://travis-ci.org/coi-gov-pl/rspec-puppet-facts-unsupported.svg?branch=develop)](https://travis-ci.org/coi-gov-pl/rspec-puppet-facts-unsupported) [![Gem](https://img.shields.io/gem/v/rspec-puppet-facts-unsupported.svg)](https://rubygems.org/gems/rspec-puppet-facts-unsupported)

Helpers to generate unsupported OS facts to test for proper fail.

Using new `on_unsupported_os` method you can get a number of random provided OS's with their facts. Those facts can be used in rspec-puppet tests just like shown below:

```ruby
on_unsupported_os.each do |os, facts|
  context "on unsupported OS #{os}" do
    let(:facts) { facts }
    it { is_expected.to compile.and_raise_error(/Unsupported operating system/) }
  end
end
```

## Installation

Add this to your puppet module's Gemfile:

```ruby
group :system_tests do
  # [..]
  gem 'rspec-puppet-facts-unsupported'
end
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rspec-puppet-facts-unsupported

## Usage

### Simple example

randomized, limited to 2 records, unsupported os - facts pair

```ruby
# in rspec-puppet test
require 'spec_helper'
require 'rspec-puppet-facts-unsupported'

describe '::vagrant', type: :class do
  include RspecPuppetFactsUnsupported

  # Generates by default 2 records from shuffled library of unsupported OS's
  on_unsupported_os.each do |os, facts|
    context "on unsupported OS #{os}" do
      let(:facts) { facts }
      it { is_expected.to compile.and_raise_error(/Unsupported operating system/) }
    end
  end
end
```

### Examples with options

```ruby
# in rspec-puppet test
require 'spec_helper'
require 'rspec-puppet-facts-unsupported'

describe '::vagrant', type: :class do
  include RspecPuppetFactsUnsupported

  # Generates by up to 6 records from library in original order filtering to return only RedHat systems
  on_unsupported_os(limit: 6, order: :original, filters: { osfamily: 'RedHat' }).each do |os, facts|
    context "on unsupported OS #{os}" do
      let(:facts) { facts }
      it { is_expected.to compile.and_raise_error(/Unsupported operating system: RedHat/) }
    end
  end
end
```

## Development

After checking out the repo, run `bundle` to install dependencies. Then, run `bundle exec rake spec` to run the tests. You can also run `bundle exec rake console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, use GitFlow convention - remember to update the version number in `version.rb`. Push branches and new tag. When finished checkout master and run `bundle exec rake build` and then run `gem push pkg/*.gem`, which will push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/coi-gov-pl/rspec-puppet-facts-unsupported. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [Apache 2.0](https://opensource.org/licenses/Apache-2.0).

## Code of Conduct

Everyone interacting in the Puppet::Examples::Helper project’s codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/coi-gov-pl/rspec-puppet-facts-unsupported/blob/master/CODE_OF_CONDUCT.md).
