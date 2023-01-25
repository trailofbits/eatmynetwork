eatmynetwork
============

[![Lint](https://github.com/trailofbits/eatmynetwork/actions/workflows/lint.yml/badge.svg)](https://github.com/trailofbits/eatmynetwork/actions/workflows/lint.yml)

`eatmynetwork` is a small script for running programs with (minimal) network
sandboxing.

```console
$ eatmynetwork ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8): 56 data bytes
ping: sendto: Operation not permitted
```

## Why/what/how?

Network connectivity sneaks its way into programs &mdash; many things that
*shouldn't* need network access do, and fail in surprising ways when a network
connection isn't available.

`eatmynetwork` makes it easy to quickly determine how a program behaves when it
doesn't have network access, without actually having to turn off your host's
network adapter(s). This makes it useful for:

* Resiliency testing: ensuring that a program that *shouldn't* require network
  access handles the lack of access gracefully

* Unit testing: ensuring that network access does not creep into idempotent and
  offline-only unit tests and, separately, ensuring that online unit tests
  are correctly marked and filtered when testing offline

Under the hood, all `eatmynetwork` does is wrap a handful of platform-specific
network isolation techniques: on macOS it uses `sandbox-exec`, and on Linux
it tries a handful of different network namespace management tools.

Importantly, `eatmynetwork` **does not provide a security boundary**: it does
not guarantee that a motivated or malicious process **can't** access the
network, only that ordinary ("honest") programs will think that they're offline.
If you need strong isolation, you need another tool.

## Installation

`eatmynetwork` is a POSIX `sh` script, and should work correctly on
modern Linux and macOS installations.

To install it, just copy it wherever you need it:

```console
$ git clone https://github.com/trailofbits/eatmynetwork && cd eatmynetwork
$ cp ./eatmynetwork /some/bin/dir/
```

There's also a [manual page](./eatmynetwork.1) that you can copy.

## Usage

Run a command:

```console
$ eatmynetwork ls
```

Run a command, with arguments:

```console
$ eatmynetwork curl https://example.com
```

Do some debug logging while running:

```console
$ DEBUG=1 eatmynetwork ssh example.com
```

In general, prefixing any command with `eatmynetwork` should run the command
exactly the same, just without network access. There are no behavior-modifying
flags or options.

## License

`eatmynetwork` is licensed under the terms of the
[Apache-2.0 License](./LICENSE).
