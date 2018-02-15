---
title: Ruby Versions
description: Differences between Ruby versions
date: 2018-02-11
category: ruby
hidden: true
---

# Ruby Versions
As time goes on some features are added, others removed, some changed.
This is a reminder of how Ruby evolved through time.

Version | Release date | End of support | End of maintenance
 ---    | ---          | ---            | ---
[1.8]   |              |                |
[1.9]   |              |                |
[2.0]   |              |                |
[2.1]   |              |                |
[2.2]   |              |                |
[2.3]   |              |                |
[2.4]   |              |                |
[2.5]   |              |                |

## 1.8
- We will consider this version our starting point.

## 1.9
- From MRI to YARV
- Splat any args
- Enumerators
- ``Kernel#require_relative``
- ``String#prepend``
- ``IO#write/binwrite/binread``
- ``String#clear``
- ``Numeric#step``

## 2.0
- ``Kernel#__dir__``

## 2.1
- ``Numeric#bit_length``

## 2.2
- Symbol GC

## 2.3
- Frozen string literals
- Safe navigation ``&``
- ``Array/Hash#dig``

## 2.4
- Unify Fixnum and Bignum into Integer

## 2.5
- ``Kernel#yield_self``
- ``String#delete_prefix/delete_suffix``
- ``Array#prepend/append`` as aliases of ``Array#unshift/push``

## Future
- [Ruby 3x3](https://blog.heroku.com/ruby-3-by-3 "Ruby 3x3: Matz, Koichi, and Tenderlove on the future of Ruby Performance")