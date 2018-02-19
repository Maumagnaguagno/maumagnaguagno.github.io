---
title: Ruby Versions
description: Differences between Ruby versions
date: 2018-02-11
category: ruby
hidden: true
---

# Ruby Versions
As time goes on some features are added, others removed or changed.  
This is a reminder of how Ruby evolved through time.

 Version   | Release    | End of support | End of maintenance
 :---:     | :---:      | :---:          | :---:
[1.8](#18) | 2003-08-04 | 2012-06-xx     | 2014-07-01
[1.9](#19) | 2007-12-25 | 2014-02-23     | 2015-02-23
[2.0](#20) | 2013-02-24 | 2015-02-24     | 2016-02-24
[2.1](#21) | 2013-12-25 | 2016-03-30     | 2017-03-31
[2.2](#22) | 2014-12-25 | 2017-03-28     | 2018-03-31
[2.3](#23) | 2015-12-25 | TBA            | TBA
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
- Hashes use insertion order
- One character strings are not ASCII values ``?c == 'c'``
- ``Kernel#require_relative``
- ``String#prepend``
- ``String#clear``
- ``IO#write/binwrite/binread``
- ``Numeric#step``

## 2.0
- Keyword arguments
- UTF-8 as default encoding
- Convert to Hash with ``#to_h``
- Onigmo replaces Oniguruma
- ``Kernel#__dir__``

## 2.1
- ``Numeric#bit_length``

## 2.2
- Symbol GC [#9634](https://bugs.ruby-lang.org/issues/9634)

## 2.3
- Frozen string literals
- Safe navigation ``obj&.foo`` [#11537](https://bugs.ruby-lang.org/issues/11537)
- ``Array/Hash#dig``
- Experimental ``RubyVM::InstructionSequence#to_binary/load_from_binary`` [#11788](https://bugs.ruby-lang.org/issues/11788)

## 2.4
- Unify Fixnum and Bignum into Integer [#12005](https://bugs.ruby-lang.org/issues/12005)
- ``Array#max/min`` do not create temporary arrays under certain conditions [#12172](https://bugs.ruby-lang.org/issues/12172)
- ``Regexp#match?`` does not change global variables [#8110](https://bugs.ruby-lang.org/issues/8110)

## 2.5
- ``Kernel#yield_self``
- ``String#delete_prefix/delete_suffix``
- ``Array#prepend/append`` as aliases of ``Array#unshift/push``
- ``rescue/else/ensure`` in ``do/end`` blocks

## 2.6
- **TODO: list possible features**

## Future
- [Ruby 3x3](https://blog.heroku.com/ruby-3-by-3 "Ruby 3x3: Matz, Koichi, and Tenderlove on the future of Ruby Performance")