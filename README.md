# MiniMime

Minimal mime type implementation for use with the mail and rest-client gem.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'mini_mime'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install mini_mime

## Usage

```
require 'mini_mime'

MiniMime.lookup_by_filename("a.txt").content_type
# => "text/plain"

MiniMime.lookup_by_content_type("text/plain").extension
# => "txt"

MiniMime.lookup_by_content_type("text/plain").binary?
# => false

```

## Performance

MiniMime is optimised to minimize memory usage. It keeps a cache of 100 mime type lookups (and 100 misses). There are benchmarks in the [bench directory](https://github.com/discourse/mini_mime/blob/master/bench/bench.rb)

```
Memory stats for requiring mime/types/columnar
Total allocated: 8686910 bytes (102917 objects)
Total retained:  3156016 bytes (33593 objects)

Memory stats for requiring mini_mime
Total allocated: 41064 bytes (362 objects)
Total retained:  7156 bytes (60 objects)
Warming up --------------------------------------
cached content_type lookup MiniMime
                        72.481k i/100ms
content_type lookup MIME::Types
                        13.284k i/100ms
Calculating -------------------------------------
cached content_type lookup MiniMime
                        914.838k (± 1.3%) i/s -      4.639M in   5.071456s
content_type lookup MIME::Types
                        140.215k (± 3.4%) i/s -    704.052k in   5.026273s
Warming up --------------------------------------
uncached content_type lookup MiniMime
                         1.329k i/100ms
content_type lookup MIME::Types
                        13.225k i/100ms
Calculating -------------------------------------
uncached content_type lookup MiniMime
                         13.338k (± 1.7%) i/s -     67.779k in   5.083373s
content_type lookup MIME::Types
                        139.626k (± 4.2%) i/s -    700.925k in   5.027074s
```

As a general guideline, cached lookups are 6x faster than MIME::Types equivalent. Uncached lookups are 10x slower.

Note: It was run on macOS 10.14.2, and versions of Ruby and gems are below.

- Ruby 2.6.0
- mini_mime (1.0.1)
- mime-types (3.2.2)
- mime-types-data (3.2018.0812)

## Development

MiniMime uses the officially maintained list of mime types at [mime-types-data](https://github.com/mime-types/mime-types-data) repo to build the internal database.

To update the database run:

```ruby
bundle exec rake rebuild_db
```

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/discourse/mini_mime. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
