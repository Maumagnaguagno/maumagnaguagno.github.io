---
layout: default
title: Ruby
description: My Ruby tricks
---

# Ruby

## Conditional/Assignment merge
Other languages may not let you assign variables inside a conditional statement, such as ``if``, ``elsif``, ``while`` or ``until``.
But Ruby does let you do that, which actually can help you avoid asking twice for the same variable.
The following code blocks are equivalent, in the first case we test and only then modify the array, in the second case we do both at once.

```ruby
my_array = [1, 2, ...]
until my_array.empty?
  puts my_array.shift
end
```

```ruby
my_array = [1, 2, ...]
until first = my_array.shift
  puts first
end
```

## Splat operator
Exploit easy to use arrays with the ``*`` splat operator, an asterisk that prefix variables to remove the array container.

```ruby
[1, *[2, 3]] == [1, 2, 3] # => true

def process(par1, par2, *par3)
  # par1 == ARGV[0]
  # par2 == ARGV[1]
  # par3 == ARGV[2..-1]
end

process(*ARGV)
```

## Pointer unification
Instead of creating your own ``FreeVariable`` class, consider ``String`` for fast value replacement.
Strings can be used as pointers, so that changing the value of one string object changes the value of all variables that point to that one.
Other complex objects use the same approach, with variables only holding such pointer, but ``true``, ``false``, ``nil`` and numeric objects are stored in-place for speed.

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
unification(a) { puts a } # Prints 1\n2\n3\n
```

## Rescue
Sometimes your program is interrupted by an error, other times you just did something wrong and decided to finish it, causing an interruption ``CTRL+C``
Useful when combined with timers, so you know how much time has passed until your patience has run dry.
Remember that you can also use ``rescue`` blocks in method definitions.

```ruby
begin # or def bar
  # ...
rescue Interrupt
  puts 'Interrupted'
rescue
  puts $!, $@
end
```

## ARGV parsing
Parse arguments using a ``while`` loop that consumes values from ``ARGV``.
To make everything easier you can define the parser in a class method, such as ``self.setup``, that returns a new instance.
Required terms are processed first while others are shifted from the option list as required.
Put ``HELP`` description at the top to provide help for both developer and user.
Conditional/Assignment merge and splat are used in this example.

```ruby
class Foo

  HELP = "Foo #{VERSION = '1.0.0'}
  Usage:
    Foo arg1 [options]\n
  Options:
    -d       - specify debug mode
    -v level - verbose level: 0 for silent, 1 for warnings or 2 for complete"

  def initialize(ar1, debug, verbose)
    #...
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
  if ARGV.size < 1 or ARGV.first == '-h'
    puts Foo::HELP
  else Foo.setup(*ARGV)
  end
end
```