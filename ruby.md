---
title: Ruby
description: Lots of Ruby tricks
date: 2016-09-15
category: ruby
---

# Ruby
Over the years some tricks have been very useful while programming in Ruby, this is a collection of them.

## Conditional/Assignment merge
Other languages may not let you assign variables inside a conditional statement, such as ``if``, ``elsif``, ``while`` or ``until``.
But Ruby does let you do that, which actually can help you avoid asking twice for the same variable.
The following code blocks are equivalent, in the first case we test and only then modify the array, in the second case we do both at once.

<div class="split" markdown="1">

```ruby
a = [1, 2, 3]
# Multi-line version
until a.empty?
  puts a.shift
end
# Single line version
puts a.shift until a.empty?
```

</div>
<div class="split" markdown="1">

```ruby
a = [1, 2, 3]
# Multi-line version
until first = a.shift
  puts first
end
# Fail due to first being assigned after access
puts first until first = a.shift
```

</div>

## Side-effects
Sometimes parentheses can make all the difference in the evaluation of an expression.
The following examples show how you can obtain the last element of a list and a list without its last element, with or without side-effects.

<div class="split" markdown="1">

```ruby
a = [1,2,3]
b = a[-1]  #=> 3, a = [1,2,3]
b = a.last #=> 3, a = [1,2,3]
c = a.pop  #=> 3, a = [1,2]
```

</div>
<div class="split" markdown="1">

```ruby
a = [1,2,3]
b = a[0..-2]  #=> [1,2], a = [1,2,3]
b = a.drop(1) #=> [1,2], a = [1,2,3]
(c = a).pop   #=> 3, a = c = [1,2]
```

</div>

## Splat operator
Exploit arrays with the ``*`` splat operator, an asterisk prefix to expand content from an inner container to an outer container at the same position.

```ruby
[1, *[2, 3]] #=> [1, 2, 3]
[*1..3, 4]   #=> [1, 2, 3, 4]

def process(arg0, arg1, *other_args)
  # arg0 == ARGV[0]
  # arg1 == ARGV[1]
  # other_args == ARGV[2..-1]
end

process(*ARGV)

# Particularly useful to print Hashes
p *GC.stat

# Array coercion
a = *'str' #=> ['str']
```

## Custom-implicit Splat
Define ``to_ary`` to splat any instance.

```ruby
class Foo
  def initialize(x, y)
    @x = x
    @y = y
  end

  def to_ary
    [@x, @y]
  end
end

a, b = Foo.new(4,2)
a # => 4
b # => 2
```

## Splat iterator
Iterating nested ``Arrays`` can be cumbersome, but with parentheses we have parallel assignment, which acts just like the splat operator.

```ruby
array = [[[1, 2], [3, 4]]]
array.each {|a,b| p [a.first, a.last, b.first, b.last]} # Prints [1, 2, 3, 4]
array.each {|(a,b),(c,d)| p [a, b, c, d]}               # Prints [1, 2, 3, 4]

# Splat with index
array = [['a', 'b'], ['c', 'd']]
array.each_with_index {|(a,b),i| p [i, a, b]} # Prints [0, "a", "b"] [1, "c", "d"]
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
But that does not mean your program is going to terminate, sometimes you want to avoid a dirty exit and the computer must rescue you.
This is useful when combined with timers, so you know how much time has passed until your patience has run dry.
Multiple exceptions can be defined and their order is important.

```ruby
# Single line
unsafe(x) rescue puts 'Oops, something went wrong'

begin # or def bar
  t = Time.now.to_f
  f = open('file.txt')
  # ...
rescue Interrupt
  puts "Interrupted after #{Time.now.to_f - t}s"
rescue
  puts $!, $@
else
  puts 'No exceptions were raised!'
ensure
  f.close
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
Instead of thinking if both objects have the same class we can think if ``self`` is an instance of other object class, which is slightly faster.
Another option is to use duck typing with ``respond_to?``, which allows other class instances to be compared, while requiring a ``respond_to?`` call for each method or variable to verify its availability.

```ruby
class MyObject

  attr_reader :var

  def initialize(var)
    @var = var
  end

  def ==(other) # Class comparison
    instance_of?(other.class) and @var == other.var
  end

  def ==(other) # Duck typing comparison
    other.respond_to?(:var) and @var == other.var
  end
end
```

## Float division
Instead of forcing values to Float before division to output another Float, ``a.to_f / b``, remember that ``fdiv`` can do that for you, ``a.fdiv(b)``.
This is useful if values must conform to mathematics/physics formulas where integer division rules from programming are nonexistent.

## Range max or last element
Ranges may include ``(1..end)`` or exclude ``(1...end)`` their end, you can check with ``range.exclude_end?``.
To obtain the last element included in a range use ``range.max``, otherwise use ``range.last``.

<div class="split" markdown="1">

```ruby
a = 1..5
a.exclude_end?    #=> false
[a.first, a.last] #=> [1, 5]
[a.min,   a.max]  #=> [1, 5]
```

</div>
<div class="split" markdown="1">

```ruby
b = 1...5
b.exclude_end?    #=> true
[b.first, b.last] #=> [1, 5]
[b.min,   b.max]  #=> [1, 4]
```

</div>

## Everything returns a value
Every Ruby code block returns a value, from a class or method definition to a method call or if/case statement.
Which means you can do ``a.each {}.clear`` instead of ``a.each {}; a.clear``, or assign the return value of method calls from case statements, such as ``a = case b; when ...; end.method``.
One interesting construction is to select which array to append an element ``(use_a ? array_a : array_b) << element``.
Consider replacing ``element`` with a complex formula without this construction, one would repeat the formula for branch or use a variable before the condition is evaluated.
Repetition implies hard maintenance and variables imply reuse, use such constructions with caution to make your code intention more readable.

## Give me more
These are places to search for more Ruby snippets:
- [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide) to learn about consistency
- [Idiosyncratic Ruby](https://idiosyncratic-ruby.com/) to learn about inconsistencies
- [RuboCop](http://batsov.com/rubocop/) to enforce a style to your project
- [Fast Ruby](https://github.com/JuanitoFatas/fast-ruby) is a collection of fast Ruby idioms
- BigBinary's blog has a series of posts about Ruby [2.4](https://blog.bigbinary.com/categories/Ruby-2-4), [2.5](https://blog.bigbinary.com/categories/Ruby-2-5) and [2.6](https://blog.bigbinary.com/categories/Ruby-2-6)
- [Ruby performance tricks](https://www.greyblake.com/blog/2012-09-01-ruby-perfomance-tricks/) and [Unexpected Ruby behaviour](https://www.greyblake.com/blog/2012-08-10-unexpected-ruby-behaviour/) by Sergey Potapov