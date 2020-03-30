---
title: 'My first exposure to AST instrumentation in Go'
date: 2020-03-29
permalink: /blog/2020-03-29-AST-GO/
tags:
  - AST
  - Instrumentation
  - Golang
  - Runtime Verification
---

# Intro to Go Concurrency Verification
Due to its simplicity and functionality, Go is receiving attention among developers. However, there are not that many tools to focus on the correctness of Go concurrency. Two recent Go [static verification](https://en.wikipedia.org/wiki/Software_verification) papers ([1](https://dl.acm.org/doi/10.1145/3009837.3009847), [2](http://mrg.doc.ic.ac.uk/publications/a-static-verification-framework-for-message-passing-in-go-using-behavioural-types/draft.pdf)) extract a system of type equations from the Go source code SSA IR [^1] equivalent. A calculus mirrors channel-based behaviors of Go programs (goroutines, functions, channel create/communicate/close, etc.)  in the form of mutually recursive definitions. Table1 explains the syntax of MiGo.  The behavioral typing system derives from MiGo can be checked for liveness and safety in three steps:

1. Syntactic restrictions on types (*Fenced* types)
2. Symbolic semantics definitions for types (LTS)
3. Decidability proof for liveness and channel safety (Symbolic execution)

Static verification, however, has its own pitfalls. For larger programs, it would suffer from *state explosion* and *un-scalability*. That is why I am interested in studying **dynamic** behavior of Go. I am gradually literature reviewing [current instrumenting/tracing tool](https://docs.google.com/spreadsheets/d/14h-ej1wNa-ZFDNTAt9QZoPn3vrI_jjBfCmi7X-2VDac/edit?usp=sharing) (industrial and academic). My goal is to find the best way to instrument Go applications to trace events that imply:

- Channel creation/communication/termination
- Goroutine (i.e., user-level thread) spawning
- *Select* non-deterministic selection

to build a knowledge base so that we can bind the typing system described in work by *Lange et al.* to captured events and check for liveness and safety conflicts.

Thanks to my advisor, we found a recent [paper](https://dl.acm.org/doi/10.1145/3236950.3236959) by Marting Sulzmann that:
- Instruments the source using AST and Parser package in Go
- Trace channel, goroutine and select events
- Assign vector clocks to events to infer Happens-Before relation among them
- Replay the collected event to dynamically verify (check for liveness and safety) Go concurrency
- Study the alternating potential schedules

Well, they have implemented the exact idea that I had in mind. Here is the [repo](https://github.com/KaiSta/gopherlyzer-GoScout) of their tool. It looks nicely implemented using ideas that I found well explained in [building Go tool video](https://www.youtube.com/watch?v=oxc8B2fjDvY) and [AST-based instrumentation blog](https://developers.mattermost.com/blog/instrumenting-go-code-via-ast/).


[^1]: Single Static Assignment (SSA) is a form property of Intermediate Representation (IR), which requires that each variable assigned precisely once. Go has a package that generates SSA IR from source.

# Tools to build Go tools
Tracing tools like [Jaeger](https://github.com/jaegertracing/jaeger-client-go), [Prometheus](https://prometheus.io/), [Zipkin](https://github.com/openzipkin/zipkin-go) and [Datadog](https://www.datadoghq.com/dg/apm/go-application-performance/) provides front-ends on top of general-purpose back-end distributed tracing engines like [OpenTracing](https://opentracing.io/) and [Dapper](https://research.google/pubs/pub36356/). The provided APIs are rich enough to gather evidence and metrics for application performance and latency measurement. But I did not find any of these APIs useful to gather precise information regarding channels, goroutines and non-determinism of select (which are requirements for formal verification of Go applications).

Now that I did not find any of these (mostly commercial) tools helpful for my purpose (which is automatic instrumentation of Go for runtime verification), I found that Go provides packages that allow developers/researchers make their own customized tools:


- **Go/token:** Defines constants representing the lexical tokens of the Go and their basic operations.
- **Go/scanner:** Implements a scanner for source text. It takes a []byte as source which can be tokenized through repeated calls to the scan method.
- **Go/ast:** Declares the types used to represent syntax trees for Go packages.
- **Go/parser:** Implements a parser for Go source files. Input may be provided in a variety of forms; the output is an abstract syntax tree (AST) representing the Go source. In other words, parser uses scanner to iterate over tokens of a source code (e.g., line of code) and generates its equivalent AST.
- **Go/printer:** Prints the equivalent Go source of an AST.

The basic idea for automatic instrumentation of Go source code is to:
- take a Go source and pass it to the **parser** package
- generate its **AST**
- insert whatever instrumentation is needed to the AST
- print the new source
------
