---
title: Ruby Versions
description: Differences between Ruby versions
date: 2018-02-11
category: ruby
---

# Ruby Versions
This is a reminder of how Ruby evolves through time.

 Version   | Release    | End of support | End of maintenance
 :---:     | :---:      | :---:          | :---:
[1.8](#18) | 2003-08-04 | 2012-06-xx     | 2014-07-01
[1.9](#19) | 2007-12-25 | 2014-02-23     | 2015-02-23
[2.0](#20) | 2013-02-24 | 2015-02-24     | 2016-02-24
[2.1](#21) | 2013-12-25 | 2016-03-31     | 2017-03-31
[2.2](#22) | 2014-12-25 | 2017-03-28     | 2018-03-31
[2.3](#23) | 2015-12-25 | 2018-03-28     | 2019-03-31
[2.4](#24) | 2016-12-25 | 2019-04-01     | 2020-03-31
[2.5](#25) | 2017-12-25 | 2020-04-01     | 2021-04-05
[2.6](#26) | 2018-12-25 | 2021-04-01     | 2022-04-01
[2.7](#27) | 2019-12-25 | 2022-04-01     | 2023-03-31
[3.0](#30) | 2020-12-25 | 2023-04-01     | 2024-04-23
[3.1](#31) | 2021-12-25 | 2024-04-01     | 2025-03-26
[3.2](#32) | 2022-12-25 | 2025-04-01     | 2026-03-31
[3.3](#33) | 2023-12-25 | TBA            | 2027-03-31
[3.4](#34) | 2024-12-25 | TBA            | TBA
[4.0](#40) | 2025-12-25 | TBA            | TBA

{% assign issue = "https://bugs.ruby-lang.org/issues/" %}

## 1.8
- Consider this version our starting point

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
- UTF-8 as default encoding [#6679]({{ issue }}6679)
- Convert to Hash with ``#to_h`` [#6276]({{ issue }}6276)
- Onigmo replaces Oniguruma [#5820]({{ issue }}5820)
- ``Kernel#__dir__`` [#3346]({{ issue }}3346)
- ``String#lines/chars/codepoints/bytes`` returns Array instead of Enumerator [#6670]({{ issue }}6670)

## 2.1
- VM (method cache)
- Generational GC
- Bignum with GMP [#8796]({{ issue }}8796)
- ``String#scrub`` [#8414]({{ issue }}8414)
- ``Numeric#bit_length`` [#8700]({{ issue }}8700)
- ``Module#prepend/include`` are public [#8846]({{ issue }}8846)

## 2.2
- ``nil``, ``true`` and ``false`` objects are frozen [#8923]({{ issue }}8923)
- Symbol GC [#9634]({{ issue }}9634)

## 2.3
- Frozen string literals [#11473]({{ issue }}11473)
- ``String#+@/-@`` to get mutable and frozen strings, respectively [#11782]({{ issue }}11782)
- ``obj&.foo`` [safe navigation operator](https://en.wikipedia.org/wiki/Safe_navigation_operator) [#11537]({{ issue }}11537)
- ``Array/Hash/Struct#dig`` [#11643]({{ issue }}11643) [#11688]({{ issue }}11688)
- ``Array#bsearch_index`` [#10730]({{ issue }}10730)
- ``Hash#fetch_values`` [10017]({{ issue }}10017)
- Experimental ``RubyVM::InstructionSequence#to_binary`` and ``RubyVM::InstructionSequence.load_from_binary`` [#11788]({{ issue }}11788)

## 2.4
- Unify Fixnum and Bignum into Integer [#12005]({{ issue }}12005)
- ``Integer/Float#ceil/floor/truncate`` with optional digits [#12245]({{ issue }}12245)
- ``Integer#digits`` to extract columns of place-value notation [#12447]({{ issue }}12447)
- ``Integer#round`` with half option, default behavior is round-up [#12548]({{ issue }}12548) [#12958]({{ issue }}12958)
  - Half option can be: ``:even``, ``:up``, and ``:down`` [#12953]({{ issue }}12953)
- ``Array#max/min`` do not create temporary arrays under certain conditions [#12172]({{ issue }}12172)
- ``Regexp#match?`` does not change global variables [#8110]({{ issue }}8110)
- ``Comparable#clamp`` [#10594]({{ issue }}10594)
- ``IO#gets/readline/each_line/readlines/foreach``, ``String#each_line/lines``, ``StringIO#gets/readline/each_line/readlines`` with chomp option [#12553]({{ issue }}12553)
- ``String.new`` with ``:capacity`` to preallocate size [#12024]({{ issue }}12024)
- ``String#unpack1`` [#12752]({{ issue }}12752)

## 2.5
- ``Kernel#yield_self`` [#6721]({{ issue }}6721)
- ``String#delete_prefix/delete_suffix`` [#12694]({{ issue }}12694) [#13665]({{ issue }}13665)
- ``Array#prepend/append`` as aliases of ``Array#unshift/push`` [#12746]({{ issue }}12746)
- ``rescue/else/ensure`` in ``do/end`` blocks [#12906]({{ issue }}12906)
- ``Dir.children/each_child`` [#11302]({{ issue }}11302)
- ``Integer.pow(a, b)`` [#12508]({{ issue }}12508)

## 2.6
- Initial implementation of a JIT compiler [#14235]({{ issue }}14235)
  - [MJIT: A Method Based Just-in-time Compiler for Ruby](https://blog.heroku.com/ruby-mjit)
- Endless Ranges ``(1..)`` and ``(1...)`` [#12912]({{ issue }}12912)
- ``Array#union/difference`` [#14097]({{ issue }}14097)
- ``Dir#each_child/children`` [#13969]({{ issue }}13969)
- ``Random.bytes`` [#4938]({{ issue }}4938)
- ``Binding#source_location`` [#14230]({{ issue }}14230)
- ``Kernel#system`` raise with ``:exception`` option [#14386]({{ issue }}14386)
- ``Kernel.then`` as alias to ``Kernel.yield_self`` [#14594]({{ issue }}14594)
- ``String#split`` yields to block if given [#4780]({{ issue }}4780)
- ``File#read/binread/write/binwrite/foreach/readlines`` without pipes [#14245]({{ issue }}14245)

## 2.7
- Experimental numbered parameter [#4475]({{ issue }}4475)
- Experimental pattern matching [#14912]({{ issue }}14912)
- Beginless Ranges ``(..1)`` and ``(...1)`` [#14799]({{ issue }}14799)
- ``Enumerable#tally`` [#11076]({{ issue }}11076)
- ``Integer#[]`` now supports range operation [#8842]({{ issue }}8842)
- Syntactic sugar argument forwarding ``def foo(*) bar.foo(*) end`` [#16253]({{ issue }}16253)
- ``Enumerable#filter_map`` [#15323]({{ issue }}15323)
- ``Array#intersection`` [#16155]({{ issue }}16155)

## 3.0
- [Ruby 3x3: Matz, Koichi, and Tenderlove on the future of Ruby Performance](https://blog.heroku.com/ruby-3-by-3)
- Experimental Ractors
- Experimental endless method definition ``def square(x) = x * x`` [#16746]({{ issue }}16746)
- ``Hash#except`` [#15822]({{ issue }}15822)

## 3.1
- YJIT: New experimental in-process JIT compiler [#18229]({{ issue }}18229)
- ``Array#intersect?`` [#15198]({{ issue }}15198)
- Anonymous block argument ``def foo(&) bar(&) end`` [#11256]({{ issue }}11256)
- Omit values in Hash literals and keyword arguments [#14579]({{ issue }}14579)

## 3.2
- Anonymous rest and keyword rest arguments [#18351]({{ issue }}18351)
  - ``def foo(*) bar(*) end``
  - ``def baz(**) quux(**) end``
- ``Enumerator#product`` [#18685]({{ issue }}18685)
- Data class as immutable Struct [#16122]({{ issue }}16122)
- YJIT no longer experimental

## 3.3
- ``String#bytesplice`` [#19314]({{ issue }}19314)
- ``MatchData#named_captures`` [#19591]({{ issue }}19591)
- ``Array#pack`` and ``String#unpack``: raise ``ArgumentError`` for unknown directives [#19150]({{ issue }}19150)
- ``Range#reverse_each`` [#18515]({{ issue }}18515)
- ``Range#overlap?`` [#19839]({{ issue }}19839)

## 3.4
- ``it`` as anonymous block argument [#18980]({{ issue }}18980)

## 4.0
- ``Array#find/rfind`` [#21678]({{ issue }}21678)

## More Information
- [Ruby Changes](https://rubyreferences.github.io/rubychanges/)