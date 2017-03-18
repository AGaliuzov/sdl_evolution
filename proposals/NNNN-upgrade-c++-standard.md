# Move to the new C++11 standrd 

* Proposal: [SDL-NNNN](NNNN-upgrade-c++-standard.md)
* Author: [Alexandr Galiuzov](https://github.com/AGaliuzov)
* Status: **Awaiting review**
* Impacted Platforms: [Core]

## Introduction

C++ was designed to be a systems programming language and has been used for embedded systems programming and other resource-constrained types of programming since the earliest days.
Nowadays we are beginning to see bits and pieces of what the so-called connected car will look like — a fully digitized vehicle with Wi-Fi; advanced infotainment systems and apps; vehicle-to-vehicle communications that let cars on the road “talk” to each other, exchanging basic safety data such as speed and position; real-time location services and routing based on traffic conditions; and networked Web links that facilitate vehicle diagnostics and repairs. So embedded systems become much more complex and in the same time requirements for the performance and stability constantly increase.

## Motivation
* Reinventing the wheel:
  * own implementations for synchronization primitives, thread handling, atomics, smart pointers and various utilities
* Code bloating:
  * poor support of principle "write once - use many times"
  * usage of multiple functors and helper classes
  * lack of algorithms in old version of standard library force to write own implementation
* Performance constraints:
  * wasting more time and system resources for object copying
* Risk of resource leaks:
  * unhandy initialization of containers
  * absence of shared pointers support and presence of dangerous auto pointers
* Safety risks:
  * no strictly defined null pointer
  * using of ifdefs and various macroses
  * no strongly typed enums
* Portability issues:
  * no standardized multithreading support
* Complexity growth:
  * absence of variadic templates support, template aliases and other related features forces to write more complex intricated code

## Proposed solution
In order to be able to develop stable, reliable and high-performance solutions automotive industry needs to use appropriate environment and try to follow modern standards in software development area and in particular updated `C++11` standard. New `C++11` standard is aimed to resolve or minimize multiple problems we have currently.

## Potential downsides
* Not all the compilers are fully supports the `C++11` standard. `GCC` partially supports it starting from version 4.4.
* There is no backward compatibility with currently used `c++98`
* Some OEM are still using old software development environment i.e. QNX SDK 6.5 which is quite obsolete and uses `GCC` 4.4. With new standard they have to upgrade own environment

## Impact on existing code
All the SDL is the impact area. However it should not be changed at once. The upgrading could be done step-by-step without any kind of regression.
* Change utiilty classes such as hand written [`SharedPtr`](https://github.com/smartdevicelink/sdl_core/blob/master/src/components/include/utils/shared_ptr.h)
* Use lambda function instead of heavy functors
* Use `auto` variables and `default` constructions etc.

In general this movement could be done transperently for SDL users.

## Alternatives considered
The only alternative is to continue to use obsolete `c++98`
