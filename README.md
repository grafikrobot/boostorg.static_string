# Boost.FixedString

Branch          | Travis | Appveyor | Azure Pipelines | codecov.io | Docs | Matrix |
:-------------: | ------ | -------- | --------------- | ---------- | ---- | ------ |
[`master`](https://github.com/vinniefalco/fixed_string/tree/master) | [![Build Status](https://travis-ci.org/vinniefalco/fixed_string.svg?branch=master)](https://travis-ci.org/vinniefalco/fixed_string) | [![Build status](https://ci.appveyor.com/api/projects/status/github/vinniefalco/fixed_string?branch=master&svg=true)](https://ci.appveyor.com/project/vinniefalco/fixed-string/branch/master) | [![Build Status](https://dev.azure.com/vinniefalco/fixed-string/_apis/build/status/pipeline?branchName=master)](https://dev.azure.com/vinniefalco/fixed-string/_build/latest?definitionId=6&branchName=master) | [![codecov](https://codecov.io/gh/vinniefalco/fixed_string/branch/master/graph/badge.svg)](https://codecov.io/gh/vinniefalco/fixed_string/branch/master) | [![Documentation](https://img.shields.io/badge/docs-master-brightgreen.svg)](http://www.boost.org/doc/libs/master/doc/html/fixed_string.html) | [![Matrix](https://img.shields.io/badge/matrix-master-brightgreen.svg)](http://www.boost.org/development/tests/master/developer/fixed_string.html)
[`develop`](https://github.com/vinniefalco/fixed_string/tree/develop) | [![Build Status](https://travis-ci.org/vinniefalco/fixed_string.svg?branch=develop)](https://travis-ci.org/vinniefalco/fixed_string) | [![Build status](https://ci.appveyor.com/api/projects/status/github/vinniefalco/fixed_string?branch=develop&svg=true)](https://ci.appveyor.com/project/vinniefalco/fixed-string/branch/develop) | [![Build Status](https://dev.azure.com/vinniefalco/fixed-string/_apis/build/status/pipeline?branchName=develop)](https://dev.azure.com/vinniefalco/fixed-string/_build/latest?definitionId=6&branchName=master) | [![codecov](https://codecov.io/gh/vinniefalco/fixed_string/branch/develop/graph/badge.svg)](https://codecov.io/gh/vinniefalco/fixed_string/branch/develop) | [![Documentation](https://img.shields.io/badge/docs-develop-brightgreen.svg)](http://www.boost.org/doc/libs/develop/doc/html/fixed_string.html) | [![Matrix](https://img.shields.io/badge/matrix-develop-brightgreen.svg)](http://www.boost.org/development/tests/develop/developer/fixed_string.html)

## This is currently **NOT** an official Boost library.

## Introduction

This library provides a dynamically resizable string of characters with
compile-time fixed capacity and contiguous embedded storage in which the
characters are placed within the string object itself. Its API closely
resembles that of `std::string`

## Motivation

A fixed capacity string is useful when:

* Memory allocation is not possible, e.g., embedded environments without a free
  store, where only a stack and the static memory segment are available.
* Memory allocation imposes an unacceptable performance penalty.
  e.g., with respect to latency.
* Allocation of objects with complex lifetimes in the static-memory
  segment is required.
* A dynamically-resizable string is required within `constexpr` functions.
* The storage location of the static_vector elements is required to be
  within the string object itself (e.g. to support `memcpy` for serialization
  purposes).

## Design

The over-arching design goal is to resemble the interface and behavior of
`std::string` as much as possible. When any operation would exceed the
maximum allowed size of the string, `std::length_error` is thrown. All
algorithms which throw exceptions provide the strong exception safety
guarantee. This is intended to be a drop in replacement for `std::string`.
All the operations for `fixed_string` work when the source is within the string itself. 

The API of `fixed_string` only diverges from `std::string` in few places,
being `substr` for which this implementation returns a string view instead of `fixed_string,
and certain functions that will never throw are marked as `noexcept`, which diverges from
those of `std::string`. Every function that is in the C++20 specification of `std::string` is
present in this implementation, with the only difference being the lack of `constexpr`
for the time being. The avaliable overloads for `fixed_string` are identical to those
of `std::string`, except for `operator+` which is explicitly deleted as no reasonable implementation
would be possible, due to the difficulty in determining the size of the resulting `fixed_string`.

## Iterators

The iterator invalidation rules are different than those for `std::string`,
since:

* Moving a string invalidates all iterators
* Swapping two strings invalidates all iterators


## License

Distributed under the Boost Software License, Version 1.0.
(See accompanying file [LICENSE_1_0.txt](LICENSE_1_0.txt) or copy at
https://www.boost.org/LICENSE_1_0.txt)
