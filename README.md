<h1 align="center">
  <br>
  <a href="https://www.sourced.tech"><img src="docs/sourced.png" alt="source{d}" height="60px"></a>
  <br>
  <br>
  source{d} Lookout-sdk
  <br>
</h1>

<h3 align="center">
  Toolkit for writing new analyzers for <b><a href="https://github.com/src-d/lookout">lookout</a></b>.
</h3>

<p align="center">
  <a href="https://github.com/src-d/lookout-sdk/releases">
    <img src="https://badge.fury.io/gh/src-d%2Flookout-sdk.svg"
         alt="GitHub version">
  </a>
  <a href="https://pypi.org/project/lookout-sdk/">
    <img src="https://badge.fury.io/py/lookout-sdk.svg"
         alt="PyPI version">
  </a>
  <a href="https://travis-ci.org/src-d/lookout-sdk">
    <img src="https://travis-ci.org/src-d/lookout-sdk.svg?branch=master"
         alt="Build Status">
  </a>
  <a href="https://godoc.org/gopkg.in/src-d/lookout-sdk.v0/pb">
    <img src="https://godoc.org/gopkg.in/src-d/lookout-sdk.v0?status.svg"
         alt="GoDoc">
  </a>
  <a href="https://docs.google.com/document/d/1pqz-_SHO5BsJE-aa8o_bAY3r5vR67amnWN8-qZc2UgY">
    <img src="https://img.shields.io/badge/source%7Bd%7D-design%20document-blue.svg"
         alt="source{d} design document">
  </a>
</p>

<p align="center"><b>
    <a href="https://www.sourced.tech">Website</a> •
    <a href="https://docs.sourced.tech">Documentation</a> •
    <a href="https://blog.sourced.tech">Blog</a> •
    <a href="http://bit.ly/src-d-community">Slack</a> •
    <a href="https://twitter.com/sourcedtech">Twitter</a>
</b></p>

# Table of Contents

<!--ts-->
   * [Table of Contents](#table-of-contents)
   * [What does the SDK provide?](#what-does-the-sdk-provide)
   * [Caveats](#caveats)
         * [DataService](#dataservice)
   * [Contributing](#contributing)
      * [Community](#community)
   * [Code of Conduct](#code-of-conduct)
   * [License](#license)

<!-- Added by: david, at: 2018-12-04T20:18+01:00 -->

<!--te-->


# What does the SDK provide?

_For the complete documentation of **source{d} Lookout**, please take a look into [http://docs.sourced.tech/lookout](http://docs.sourced.tech/lookout)._

_For detailed information about the different parts of Lookout, and how they interact you can go to the [lookout architecture guide](https://github.com/src-d/lookout/blob/master/docs/architecture.md)._

**lookout-sdk** provides:

- **proto [definitions](./proto)**,
- pre-generated libraries code for [Golang](./pb) and [Python](./python):
  - an easy **access to the DataService API though a gRPC** service to lookout analyzers, so they won't need to deal with actual Git repositories, UAST extraction, programming language detection, etc,
  - low-level **helpers to workaround some protobuf/gRPC caveats**,
- quickstart [examples](./examples) of an Analyzer that detects language and number of functions (written in Go and in Python).


# Caveats

When the gRPC client and servers are being used, it is needed:
- to set [max gRPC message size](https://github.com/grpc/grpc/issues/7927); using lookout-sdk helpers it can be done:
  - go: using `pb.NewServer` and `pb.DialContext`,
  - python: using `lookout.sdk.grpc.create_channel`,
- to support [RFC 3986 URI scheme](https://github.com/grpc/grpc-go/issues/1911); using lookout-sdk helpers it can be done:
  - go: using `pb.ToGoGrpcAddress` and `pb.Listen`,
  - python: using `lookout.sdk.grpc.to_grpc_address`.

### DataService

When DataService is being dialed, you should:

- disable secure connection:
  - go: using `grpc.WithInsecure()` ([example](https://github.com/src-d/lookout-gometalint-analyzer/blob/7b4b37fb3109299516fbb43017934d131784f49f/cmd/gometalint-analyzer/main.go#L65)),
  - python: using `server.add_insecure_port(address)` ([example](https://github.com/src-d/lookout-sdk/blob/master/examples/language-analyzer.py#L63)),
- turn off [gRPC fail-fast](https://github.com/grpc/grpc/blob/master/doc/wait-for-ready.md) mode if your analyzer creates a connection to DataServer before it was actually started. This way the RPCs are queued until the chanel is ready:
  - go: using `grpc.FailFast(false)`
([example](https://github.com/src-d/lookout-gometalint-analyzer/blob/7b4b37fb3109299516fbb43017934d131784f49f/cmd/gometalint-analyzer/main.go#L66)).


# Contributing

Contributions are **welcome and very much appreciated** 🙌

Please refer [to our Contribution Guide](docs/CONTRIBUTING.md) for more details.


## Community

source{d} has an amazing community of developers and contributors who are interested in Code As Data and/or Machine Learning on Code. Please join us! 👋

- [Slack](http://bit.ly/src-d-community)
- [Twitter](https://twitter.com/sourcedtech)
- [Email](mailto:hello@sourced.tech)


# Code of Conduct

All activities under source{d} projects are governed by the [source{d} code of conduct](.github/CODE_OF_CONDUCT.md).


# License

Apache License Version 2.0, see [LICENSE](LICENSE.md)
