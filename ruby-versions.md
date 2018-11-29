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
[2.4](#24) | 2016-12-25 | TBA            | TBA
[2.5](#25) | 2017-12-25 | TBA            | TBA
[2.6](#26) | 2018-12-25 | TBA            | TBA

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
- UTF-8 as default encoding [#6679](https://bugs.ruby-lang.org/issues/6679)
- Convert to Hash with ``#to_h`` [#6276](https://bugs.ruby-lang.org/issues/6276)
- Onigmo replaces Oniguruma [#5820](https://bugs.ruby-lang.org/issues/5820)
- ``Kernel#__dir__`` [#3346](https://bugs.ruby-lang.org/issues/3346)
- ``String#lines/chars/codepoints/bytes`` returns Array instead of Enumerator [#6670](https://bugs.ruby-lang.org/issues/6670)

## 2.1
- VM (method cache)
- Bignum with GMP [#8796](https://bugs.ruby-lang.org/issues/8796)
- ``String#scrub`` [#8414](https://bugs.ruby-lang.org/issues/8414)
- ``Numeric#bit_length`` [#8700](https://bugs.ruby-lang.org/issues/8700)
- ``Module#prepend/include`` are public

## 2.2
- ``nil``, ``true`` and ``false`` objects are frozen [#8923](https://bugs.ruby-lang.org/issues/8923)
- Symbol GC [#9634](https://bugs.ruby-lang.org/issues/9634)

## 2.3
- Frozen string literals [#11473](https://bugs.ruby-lang.org/issues/11473)
- ``String#+@/-@`` to get mutable and frozen strings, respectively [#11782](https://bugs.ruby-lang.org/issues/11782)
- ``obj&.foo`` [safe navigation operator](https://en.wikipedia.org/wiki/Safe_navigation_operator) [#11537](https://bugs.ruby-lang.org/issues/11537)
- ``Array/Hash#dig`` [#11643](https://bugs.ruby-lang.org/issues/11643)
- Experimental ``RubyVM::InstructionSequence#to_binary`` and ``RubyVM::InstructionSequence.load_from_binary`` [#11788](https://bugs.ruby-lang.org/issues/11788)

## 2.4
- Unify Fixnum and Bignum into Integer [#12005](https://bugs.ruby-lang.org/issues/12005)
- ``Array#max/min`` do not create temporary arrays under certain conditions [#12172](https://bugs.ruby-lang.org/issues/12172)
- ``Regexp#match?`` does not change global variables [#8110](https://bugs.ruby-lang.org/issues/8110)
- ``Comparable#clamp`` [#10594](https://bugs.ruby-lang.org/issues/10594)

## 2.5
- ``Kernel#yield_self`` [#6721](https://bugs.ruby-lang.org/issues/6721)
- ``String#delete_prefix/delete_suffix`` [#12694](https://bugs.ruby-lang.org/issues/12694) [#13665](https://bugs.ruby-lang.org/issues/13665)
- ``Array#prepend/append`` as aliases of ``Array#unshift/push`` [#12746](https://bugs.ruby-lang.org/issues/12746)
- ``rescue/else/ensure`` in ``do/end`` blocks [#12906](https://bugs.ruby-lang.org/issues/12906)

## 2.6
[Ruby 2.6-preview1](https://www.ruby-lang.org/en/news/2018/02/24/ruby-2-6-0-preview1-released/):
- ``Random.bytes`` [#4938](https://bugs.ruby-lang.org/issues/4938)
- ``Binding#source_location`` [#14230](https://bugs.ruby-lang.org/issues/14230)
- ``Kernel#system`` raise with ``:exception`` option [#14386](https://bugs.ruby-lang.org/issues/14386)

[Ruby 2.6-preview2](https://www.ruby-lang.org/en/news/2018/05/31/ruby-2-6-0-preview2-released/):
- Endless Ranges ``(1..)`` [#12912](https://bugs.ruby-lang.org/issues/12912)
- ``Kernel.then`` as alias to ``Kernel.yield_self`` [#14594](https://bugs.ruby-lang.org/issues/14594)
- ``String#split`` yields to block if given [#4780](https://bugs.ruby-lang.org/issues/4780)

## Future
- [Ruby 3x3: Matz, Koichi, and Tenderlove on the future of Ruby Performance](https://blog.heroku.com/ruby-3-by-3)
- [MJIT: A Method Based Just-in-time Compiler for Ruby](https://blog.heroku.com/ruby-mjit)