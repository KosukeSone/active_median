# ActiveMedian

Median for ActiveRecord

Supports PostgreSQL 9.4+, MariaDB 10.3.3+, and SQLite, plus arrays and hashes

:fire: Uses native functions for blazing performance

[![Build Status](https://travis-ci.org/ankane/active_median.svg)](https://travis-ci.org/ankane/active_median)

## Usage

```ruby
Item.median(:price)
```

Works with grouping, too.

```ruby
Order.group(:store_id).median(:total)
```

## User Input

If passing user input as the column, be sure to sanitize it first like you must [with other aggregate methods like `sum`](https://rails-sqli.org/).

```ruby
column = params[:column]

# check against permitted columns
raise "Unpermitted column" unless ["column_a", "column_b"].include?(column)

User.median(column)
```

## Arrays and Hashes [master]

```ruby
[1, 2, 3].median
```

You can also pass a block.

```ruby
{a: 1, b: 2, c: 3}.median { |k, v| v }
```

## Installation

Add this line to your application’s Gemfile:

```ruby
gem 'active_median'
```

For SQLite, also follow the instructions below.

### SQLite

SQLite requires a [community extension](https://www.sqlite.org/contrib). Download [extension-functions.c](https://www.sqlite.org/contrib/download/extension-functions.c) and follow the [instructions for compiling loadable extensions](https://www.sqlite.org/loadext.html#compiling_a_loadable_extension) for your platform. On Mac, use:

```sh
gcc -g -fPIC -dynamiclib extension-functions.c -o extension-functions.dylib
```

To load it in Rails, create an initializer with:

```ruby
db = ActiveRecord::Base.connection.raw_connection
db.enable_load_extension(1)
db.load_extension("extension-functions.dylib")
db.enable_load_extension(0)
```

## Upgrading

### 0.2.0

A user-defined function is no longer needed. Create a migration with `ActiveMedian.drop_function` to remove it.

## Contributing

Everyone is encouraged to help improve this project. Here are a few ways you can help:

- [Report bugs](https://github.com/ankane/active_median/issues)
- Fix bugs and [submit pull requests](https://github.com/ankane/active_median/pulls)
- Write, clarify, or fix documentation
- Suggest or add new features
