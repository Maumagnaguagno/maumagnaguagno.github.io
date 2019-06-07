---
title: Ruby Versions
description: Differences between Ruby versions
date: 2018-02-11
category: ruby
---

# Ruby Versions
This is a reminder of how Ruby evolved through time.

 Version   | Release    | End of support | End of maintenance
 :---:     | :---:      | :---:          | :---:
[1.8](#18) | 2003-08-04 | 2012-06-xx     | 2014-07-01
[1.9](#19) | 2007-12-25 | 2014-02-23     | 2015-02-23
[2.0](#20) | 2013-02-24 | 2015-02-24     | 2016-02-24
[2.1](#21) | 2013-12-25 | 2016-03-30     | 2017-03-31
[2.2](#22) | 2014-12-25 | 2017-03-28     | 2018-03-31
[2.3](#23) | 2015-12-25 | 2018-03-28     | 2019-03-31
[2.4](#24) | 2016-12-25 | 2019-04-01     | 2020-04-01
[2.5](#25) | 2017-12-25 | TBA            | TBA
[2.6](#26) | 2018-12-25 | TBA            | TBA
[2.7](#27) | 2019-12-25 | TBA            | TBA

## 1.8
- We will consider this version our starting point

## 1.9
- From MRI to YARV
- Splat any args
- Enumerators
- New Hash syntax for Symbol keys ``{key: 'value'}``
- Hashes preserve insertion order
- Single character strings are not ASCII values ``?c != 'c'.ord``
- ``Kernel#require_relative``
- ``String#prepend``
- ``String#clear``
- ``IO.write/binwrite/binread``
- ``Numeric#step``

## 2.0
- Keyword arguments
- Extend class with ``Module#prepend``
- UTF-8 as default encoding {% include ruby_issue.html i=6679 %}
- Convert to Hash with ``#to_h`` {% include ruby_issue.html i=6276 %}
- Onigmo replaces Oniguruma {% include ruby_issue.html i=5820 %}
- ``Kernel#__dir__`` {% include ruby_issue.html i=3346 %}
- ``String#lines/chars/codepoints/bytes`` returns Array instead of Enumerator {% include ruby_issue.html i=6670 %}

## 2.1
- VM (method cache)
- Bignum with GMP {% include ruby_issue.html i=8796 %}
- ``String#scrub`` {% include ruby_issue.html i=8414 %}
- ``Numeric#bit_length`` {% include ruby_issue.html i=8700 %}
- ``Module#prepend/include`` are public {% include ruby_issue.html i=8846 %}

## 2.2
- ``nil``, ``true`` and ``false`` objects are frozen {% include ruby_issue.html i=8923 %}
- Symbol GC {% include ruby_issue.html i=9634 %}

## 2.3
- Frozen string literals {% include ruby_issue.html i=11473 %}
- ``String#+@/-@`` to get mutable and frozen strings, respectively {% include ruby_issue.html i=11782 %}
- ``obj&.foo`` [safe navigation operator](https://en.wikipedia.org/wiki/Safe_navigation_operator) {% include ruby_issue.html i=11537 %}
- ``Array/Hash#dig`` {% include ruby_issue.html i=11643 %}
- Experimental ``RubyVM::InstructionSequence#to_binary`` and ``RubyVM::InstructionSequence.load_from_binary`` {% include ruby_issue.html i=11788 %}

## 2.4
- Unify Fixnum and Bignum into Integer {% include ruby_issue.html i=12005 %}
- ``Array#max/min`` do not create temporary arrays under certain conditions {% include ruby_issue.html i=12172 %}
- ``Regexp#match?`` does not change global variables {% include ruby_issue.html i=8110 %}
- ``Comparable#clamp`` {% include ruby_issue.html i=10594 %}

## 2.5
- ``Kernel#yield_self`` {% include ruby_issue.html i=6721 %}
- ``String#delete_prefix/delete_suffix`` {% include ruby_issue.html i=12694 %} {% include ruby_issue.html i=13665 %}
- ``Array#prepend/append`` as aliases of ``Array#unshift/push`` {% include ruby_issue.html i=12746 %}
- ``rescue/else/ensure`` in ``do/end`` blocks {% include ruby_issue.html i=12906 %}

## 2.6
- Initial implementation of a JIT compiler {% include ruby_issue.html i=14235 %}
- Flip-flop syntax is deprecated {% include ruby_issue.html i=5400 %}
- Endless Ranges ``(1..)`` and ``(1...)`` {% include ruby_issue.html i=12912 %}
- ``Array#union/difference`` {% include ruby_issue.html i=14097 %}
- ``Dir#each_child/children`` {% include ruby_issue.html i=13969 %}
- ``Random.bytes`` {% include ruby_issue.html i=4938 %}
- ``Binding#source_location`` {% include ruby_issue.html i=14230 %}
- ``Kernel#system`` raise with ``:exception`` option {% include ruby_issue.html i=14386 %}
- ``Kernel.then`` as alias to ``Kernel.yield_self`` {% include ruby_issue.html i=14594 %}
- ``String#split`` yields to block if given {% include ruby_issue.html i=4780 %}

## 2.7
The first preview for 2.7 has been made public, featuring:
- Experimental numbered parameter {% include ruby_issue.html i=4475 %}
- Experimental pattern matching {% include ruby_issue.html i=14912 %}
- Method reference operator ``.:`` {% include ruby_issue.html i=12125 %}
- Beginless Ranges ``(..1)`` and ``(...1)`` {% include ruby_issue.html i=14799 %}
- ``Enumerable#tally`` {% include ruby_issue.html i=11076 %}
- ``Integer#[]`` now supports range operation {% include ruby_issue.html i=8842 %}

## Future
- [Ruby 3x3: Matz, Koichi, and Tenderlove on the future of Ruby Performance](https://blog.heroku.com/ruby-3-by-3)
- [MJIT: A Method Based Just-in-time Compiler for Ruby](https://blog.heroku.com/ruby-mjit)