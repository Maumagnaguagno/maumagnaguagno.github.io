---
title: Ruby
description: My Ruby tricks
date: 2016-09-15
category: ruby
---

# Ruby

## Conditional/Assignment merge
Other languages may not let you assign variables inside a conditional statement, such as ``if``, ``elsif``, ``while`` or ``until``.
But Ruby does let you do that, which actually can help you avoid asking twice for the same variable.
The following code blocks are equivalent, in the first case we test and only then modify the array, in the second case we do both at once.

```ruby
my_array = [1, 2, 3]
# Multi-line version
until my_array.empty?
  puts my_array.shift
end
# Single line version
puts my_array.shift until my_array.empty?
```

```ruby
my_array = [1, 2, 3]
# Multi-line version
until first = my_array.shift
  puts first
end
# Single line version will not work due to first being assigned after access
puts first until first = my_array.shift
```

## Splat operator
Exploit arrays with the ``*`` splat operator, an asterisk prefix that remove variable content from the container.

```ruby
[1, *[2, 3]] == [1, 2, 3] # => true

def process(arg0, arg1, *other_args)
  # arg0 == ARGV[0]
  # arg1 == ARGV[1]
  # other_args == ARGV[2..-1]
end

process(*ARGV)

# Particularly useful to print Hashes
p *GC.stat
```

## Pointer unification
Instead of creating your own ``Pointer`` or ``FreeVariable`` class, consider ``String`` or ``Array`` for fast value replacement.
Strings can be used as pointers, so that changing the value of one string object changes the value of all variables that point to that string.
Other complex objects use the same approach, with variables only holding pointers, while ``true``, ``false``, ``nil`` and numeric objects are stored in-place for speed.
Note that you can replace only objects of the same class, such as ``array1.replace(array2)`` and not ``array1.replace(str2)``.

```ruby
def unification(var)
  var.replace('1')
  yield
  var.replace('2')
  yield
  var.replace('3')
  yield
end

a = ''
unification(a) { puts a } # Prints 1 2 3
```

## Rescue
Sometimes your program is interrupted by an error, other times you just noticed something wrong and decided to hit <kbd>CTRL+C</kbd>, causing an interruption.
But that does not mean your program is going to terminate, sometimes you want a warning asking to save your changes before a clean exit and the computer must rescue you.
This is also useful when combined with timers, so you know how much time has passed until your patience has run dry.

```ruby
begin # or def bar
  # ...
rescue Interrupt
  puts 'Interrupted'
rescue
  puts $!, $@
end

# You can also use rescue blocks in method definitions, without begin
def my_method
  # ...
rescue
  # Your rescue block
end
```

## ARGV parsing
Parse arguments using a ``while`` loop that consumes values from ``ARGV``.
To make everything easier you can define the parser in a class method, such as ``self.setup``, that returns a new instance.
Required terms are processed first while others are shifted from ``options`` as required.
Put ``HELP`` description at the top to provide help for both developers and users.
Conditional/Assignment merge and Splat operator, previously described, are used in this example.

```ruby
class Foo

  HELP = "Foo #{VERSION = '1.0.0'}
  Usage:
    Foo arg1 [options]\n
  Options:
    -d       - specify debug mode
    -v level - verbose level: 0 for silent, 1 for warnings or 2 for complete"

  def initialize(arg1, debug, verbose)
    # ...
  end

  def self.setup(arg1, *options)
    raise "arg1 not found: #{arg1.inspect}" unless File.exist?(arg1)
    debug = false
    verbose = 2
    # Parse each option
    while opt = options.shift
      case opt
      when '-d' then debug = true
      when '-v' then verbose = options.shift.to_i
      else raise "Unknown option: #{opt}"
      end
    end
    new(arg1, debug, verbose)
  end
end

if $0 == __FILE__
  # Use ARGV.size < N for N required parameters or empty? for N == 1
  if ARGV.empty? or ARGV.first == '-h'
    puts Foo::HELP
  else Foo.setup(*ARGV)
  end
end
```

## Hash behavior
Hash may behave differently according to object initialization if default value or block behavior is provided.

```ruby
# Basic Hash
h = Hash.new
h[2]      #=> nil
h         #=> {}
h[2] += 1 #=> NoMethodError: undefined method `+' for nil:NilClass

# Default value
h = Hash.new(0)
h[2]      #=> 0
h         #=> {}
h[2] += 1 #=> {2=>1}

# Default behavior
h = Hash.new {|h,k| h[k] = 0}
h[2]      #=> 0
h         #=> {2=>0}
h[2] += 1 #=> {2=>1}
```

If the default value is not a boolean or numeric object, the behavior is probably not what one would expect, as the same instance is always returned.

```ruby
h = Hash.new('a')
h[2]         #=> 'a'
h << 'b'     #=> 'ab'
h[2]         #=> 'ab'
h.default    #=> 'ab'
h[3] <<= 'c' #=> 'abc'
h[4] += 'c'  #=> 'abcc'
h.default    #=> 'abc'
h            #=> {3=>'abc', 4=>'abcc'}
```

## Equality operator
From time to time your new object class must be compared, and comparing only instance variables, ``@var == other.var``, will result in error if the ``other`` object does not respond to ``var``.
Most users will compare the class first, ``self.class == other.class``, which is good but not optimal.
Instead of thinking if both objects have the same class we can think if object is an instance of other object class, which is slightly faster.
There is probably a variation of this equality operator which can consider inheritance using ``is_a?``.

```ruby
class MyObject

  attr_reader :var

  def initialize(var)
    @var = var
  end

  def ==(other)
    instance_of?(other.class) and @var == other.var
  end
end
```

## Float division
Instead of forcing values to Float before division to output another Float, ``a.to_f / b``, remember that ``fdiv`` can do that for you, ``a.fdiv(b)``.
This is useful if values must conform to mathematics/physics formulas where integer division rules from programming are nonexistent.

## Give me more
There are more places in the internet to search for Ruby snippets:
- [Ruby Style Guide](bbatsov/ruby-style-guide) to learn about consistency
- [RuboCop](http://batsov.com/rubocop/) to enforce a style guide to your project
- [Fast Ruby](https://github.com/JuanitoFatas/fast-ruby) is a collection of fast common Ruby idioms
- [BigBinary blog](http://blog.bigbinary.com/categories/Ruby-2-4) have a Ruby 2.4 series of posts