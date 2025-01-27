# Terracotta: Simple project template

[Developer docs]:   https://github.com/danwilliams/terracotta/blob/master/docs/developer.md
[GitHub repo]:      https://github.com/danwilliams/terracotta/
[Integration docs]: https://github.com/danwilliams/terracotta/blob/master/docs/integration.md
[Rustdocs]:         https://docs.rs/terracotta/
[Terracotta]:       https://crates.io/crates/terracotta

This repository provides a boilerplate webserver application, using [Terracotta][],
to act as a foundation and springboard for building web applications and APIs.
The intention is for it to be used as a template for new projects.

This template is for a **simple, single-crate project**.

It is intended to be easy to use and understand, easy to set up and extend, and
easy to deploy.

[Terracotta][] was created in response to the lack of full examples of how to
use Axum, and the fact that many tutorials are out-of-date, lacking important
elements, or just plain wrong. You may not need everything provided — and you
also may well not agree with how some parts are implemented — but if you are
wanting a leg-up to save some time, it's not a bad place to start!

The main sections in this README are:

  - [Features](#features)
  - [Usage](#usage)
  - [Setup](#setup)
  - [Deployment](#deployment)
  - [Attributions](#attributions)

Additional documentation of note includes:

  - [Terracotta README][GitHub repo]
  - [Terracotta Rustdocs][Rustdocs]
  - [Developer documentation][Developer docs]
  - [API Integration documentation][Integration docs]


## Features

[Axum]:         https://crates.io/crates/axum
[Bulma]:        https://bulma.io/
[Figment]:      https://crates.io/crates/figment
[Font Awesome]: https://fontawesome.com/
[Tera]:         https://crates.io/crates/tera
[Tracing]:      https://crates.io/crates/tracing
[Hyper]:        https://crates.io/crates/hyper

The main high-level points of note are:

  - Simple codebase layout
  - Full yet minimal web application working out of the box
  - Easy to extend and build upon
  - High-performance asynchronous HTTP server using [Tokio Hyper][Hyper]
  - Based on the robust and ergonomic web framework [Axum][]
  - Configuration from config file and env vars using [Figment][]
  - Logging of HTTP requests and events using [Tokio Tracing][Tracing]
  - Templates implemented using the [Tera][] template engine
  - Static file handling
  - Ability to serve static files as protected or public
  - Ability to supplement and override the static assets using local files in
    addition to a pre-compiled binary (configurable)
  - Streaming of large static files for memory efficiency
  - Single-file deployment — all assets baked in (optional and configurable)
  - CSS foundation using the [Bulma][] CSS framework
  - Icons using [Font Awesome][]
  - Simple authentication using sessions and config-based user list
  - Login page, public and protected routes, logout ability
  - Health check API endpoints
  - Comprehensive application statistics gathering and API endpoints for
    reporting
  - Graceful handling of 404 and 500 HTTP errors
  - Graceful handling of runtime application errors
  - Full OpenAPI documentation

### Authentication

Terracotta features a custom-rolled authentication system, to demonstrate how to
implement a basic session-based setup. Although storing a user list in plain
text on a server is okay for small, limited projects and experiments, it is
highly recommended to store the credentials securely in a database. That is
currently outside the scope of this project, for a number of reasons.

In a real system you will probably also want to store the sessions in a database
instead of in memory.

It is also worth noting that the pattern implemented is the best and most ideal
for an application that serves HTML to a browser. If you are creating an API
then you will want to change some of the behaviour to return HTTP status codes
to tell the client that the request is unauthorised, rather than sending a login
page. Equally, you will likely want to implement JWT or similar. That is also
outside of scope at present, partly because there are various options to choose
from. However, there is a [basic API example](https://github.com/danwilliams/terracotta/blob/master/examples/api/)
available in the [Terracotta GitHub repository][GitHub repo].

The authentication system is set up to make it easy to configure routes as
either public or protected, and is fully-implemented including a login page,
logout action, and handling of every part of the authentication journey and the
possible situations.

### Statistics

Terracotta gathers a wide range of statistics about the application, and
provides a number of API endpoints to access them. These are intended to be used
by monitoring systems, and also to provide a simple way to see what is going on
with the application.

The approach is highly performant, despite gathering a comprehensive set of
measurements, and is designed to be as efficient as possible. It is also
designed to be easy to extend, to add new statistics to the system. There is a
central statistics queue plus broadcast system, with circular buffers for
interval history, and a tick clock to keep everything up-to-date.

The statistics data is available in summary form, per-measurement history form,
and as a real-time WebSocket event stream.

### Error-handling

Terracotta has an opinionated approach to handling errors, including both HTTP
errors and "true" (Rust) errors. This serves as a baseline to build on or to
change as required.

### Databases

Terracotta very purposefully does not include any kind of database integration.
There are so many, and such a plethora of crates to choose from, that this is
best left to the application developer to decide. Database interaction is very
straightforward and so this is a simple addition to make.

### Templates

The choice of Tera is unlikely to upset anyone, but if there is a preferred
option then it is easy to change or remove. Tera has been implemented in a
slightly opinionated manner, but it should be clear what has been changed from
the defaults and how.


## Usage

This repository is designed so that it can be used as a template for new
projects, and then customised and extended. You will naturally rename the
project and tailor it to your needs, and as you implement your own features it
will get harder and harder to merge in any upstream changes. It is therefore
likely best to consider this a starting point only, and an upgrade reference,
rather than an on-going contributing source.

Note that Terracotta itself is a library, as available on [crates.io][Terracotta].
This means that most of the functionality is abstracted, and not in this
template repository.

For information on developing a project based on Terracotta, please refer to the
[Developer documentation][Developer docs].


## Setup

[Rustup]: https://rustup.rs/

The steps to set up a Terracotta project are simple and standard. You need a
reasonably-recent Rust environment, on a Linux or Mac machine. There are
currently no special requirements beyond what is needed to build a standard Rust
project.

Note that these instructions are for building the application yourself, which
will usually be in context of having used this repository as a starting point.
In this case these steps will apply for your project too. See the [Developer
documentation][Developer docs] for more information on creating your project
using this repository as a template.

### Environment

There are some key points to note about the environment you choose:

  - Debian and Ubuntu are the Linux distros of choice, although other distros
    should also work just fine, as there are no special requirements.
  - Running natively on MacOS works fine.
  - Running natively on Windows is not targeted or tested, and there are no
    plans to support it, so although it may work, it also may not. Running on
    WSL does work fine, and is the recommended way to run on Windows.

Typically, you will set up Rust using [`rustup`][Rustup], which is the
recommended way to install Rust. The `stable` toolchain is targeted, as the
focus is on stability and correctness, rather than bleeding-edge features.

Once you have Rust installed, you can build the project using `cargo build`.
This will download and compile all dependencies, and build the project. You can
then run the project using `cargo run`.

### Configuration

This project template is configured using a TOML file. The default configuration
file is `Config.toml`, which should be placed in the repository root. The
configuration settings (and file) are optional, and if not provided, default
values will be used for all configuration options.

It is also possible to pass configuration parameters from the command line, as
environment variables. The environment variables take precedence over the
configuration file options.

#### General options

The following options should be specified without any heading:

  - `host`   - The host to listen on. Defaults to `127.0.0.1`.
  - `port`   - The port to listen on. Defaults to `8000`.
  - `logdir` - The directory to store log files in. Defaults to `log`.
  - `title`  - The title of the application. Defaults to `Terracotta`.

As shown here:

```toml
host   = "127.0.0.1"
port   = 8000
logdir = "log"
title  = "Terracotta"
```

#### Local loading options

By default, all resources are baked into the binary, and served from there. This
is the most efficient way to run the application, but it is also possible to
load resources from the local filesystem, which can be useful for development
and testing, and when there are large content files.

It is possible to supplement or override HTML templates and static assets.
Static assets are subdivided into protected and public.

The following type headings are available:

  - `html_templates`   - HTML templates.
  - `assets.protected` - Protected static assets.
  - `assets.public`    - Public static assets.

The following options should be specified under the individual type headings:

  - `behavior`   - The loading behaviour.
  - `local_path` - The path to the files on the local filesystem.

The `behavior` option can be one of the following values:

  - `Deny`       - Deny loading from the local filesystem. This is the default
                   for all the options.
  - `Supplement` - Load from the local filesystem if the baked-in resources are
                   not present.
  - `Override`   - Load from the local filesystem if present, and otherwise load
                   from the baked-in resources.

For those types configured to allow loading from the local filesystem, the
following options can be specified under the individual type headings:

  - `local_path` - The path to the files.

As shown here:

```toml
[html_templates]
behavior   = "Deny"
local_path = "html"

[assets.protected]
behavior   = "Override"   # default is "Deny"
local_path = "content"

[assets.public]
behavior   = "Override"   # default is "Deny"
local_path = "static"
```

An example is provided, `rustacean-flat-happy.png`, which is available through
http://localhost:8000/rustacean-flat-happy.png if using the settings in the
example configuration file. This is a protected asset, and so will only be
served to logged-in users.

#### Static file options

When static files are requested, the method by which they are served depends
upon their source and size. All files baked into the binary are served directly
from memory, and so these options do not apply to them. Files loaded from the
local filesystem are loaded into memory and served all once if they are small
enough, but past a certain (configurable) size they are streamed to the client.

The sizes of the stream buffer and read buffer are hugely important to
performance, with smaller buffers greatly impacting download speeds. The default
values have been carefully chosen based on extensive testing, and should not
generally need to be changed. However, on a system with lots of users and very
few large files it *may* be worth decreasing the buffer sizes to reduce memory
usage when those files are requested, and on a system with very few users and
lots of large files it *may* be worth increasing the buffer sizes to improve
throughput. However, the chosen values are already within 5-10% of the very best
possible speeds, so any increase should be made with caution. It is more likely
that they would need to be decreased a little on a very busy system with a lot
of large files, where the memory usage could become a problem and the raw speed
of each download becomes a secondary concern.

The following options should be specified under an `[assets.static_files]`
heading:

  - `stream_threshold` - The size of the file, in KB, above which it will be
                         streamed to the client. Defaults to `1000` (1MiB).
  - `stream_buffer`    - The size of the stream buffer to use when streaming
                         files, in KB. Defaults to `256` (256KB).
  - `read_buffer`      - The size of the read buffer to use when streaming
                         files, in KB. Defaults to `128` (128KB).

Each of these options accepts an integer value.

As shown here:

```toml
[assets.static_files]
stream_threshold = 1000 # 1MiB — files above this size will be streamed
stream_buffer    = 256  # 256KB
read_buffer      = 128  # 128KB
```

#### User list

A list of user credentials can be specified under a `[users]` heading:

  - `username: password` - The username as the key, and the password as the
                           value.

As shown here:

```toml
[users]
joe = "1a2b3c"
```

This is a simple list of username/password pairs, where the username is the key
and the password is the value. The password is stored in plain text, so be aware
of the security implications of this (ideally you would implement an integration
with your preferred database instead). The username and password are both
case-sensitive.

### Running

A Terracotta-based project can typically be run using the `cargo run` command,
or by running the compiled binary directly. The server will listen on port 8000
by default, and will serve content from the `static` directory, plus any request
handlers that you define. The `static` directory contains the static files to be
served.

### Testing

You can run the test suite using `cargo test`. This will run all unit and
integration tests.

**Note that, at present, there are very few tests written specifically for this
project, as it is mostly a combination of other crates from the Rust ecosystem.
Those tests that do exist are intended to be examples of approach, and are not
exhaustive. Additional tests might be added when the project is more mature, and
sensible things to test have been clearly identified.**

### Documentation

You can build the developer documentation using `cargo doc`. This will generate
HTML files and place them into `target/doc`. You can then open the documentation
in your browser by opening `target/doc/terracotta-simple/index.html`.

Building the documentation for local development use will also provide you with
links to the source code.


## Deployment

[Alpine]: https://alpinelinux.org/
[Docker]: https://www.docker.com/
[UPX]:    https://upx.github.io/

### Building

You can build the project in release mode by using `cargo build --release`.
Everything required for deployment will be contained in the single binary file
produced. It is recommended to run [`upx`][UPX] on the executable before
deployment, to reduce the file size.

You can optionally supplement the compiled system with additional files from the
local filesystem, as described in the [Local loading options](#local-loading-options)
section above.

The resulting binary file can then be copied to the deployment environment, and
run directly. This will often be in a [Docker][] or Kubernetes container.

### Examples

A typical build script might look like this:

```sh
cargo build --release
upx --best target/release/terracotta
scp target/release/terracotta you@yourserver:/path/to/deployment/directory
```

### Docker

A common deployment scenario is to use [Docker][]. The Terracotta repository
includes a `Dockerfile`, which can be used to build a Docker image. This image
is based on [Alpine][], and so is very small. It is also built using
multi-stage builds, so the final image is even smaller.

It is worth noting that the Alpine build uses the `musl` C library, which is
not compatible with the `glibc` C library used by most other Linux distributions
and Docker images. The advantage of using Alpine is that the resulting image is
very small, and everything is compiled statically. If you have any compatibility
problems then you may want to use the `distroless` build instead, which is based
on `glibc`.

The Docker image can be built using the following command:

```sh
docker build -t terracotta .
```

By default, this will build a release image, and compress the binary using
[`upx`][UPX]. The setup is optimised for executable speed, build speed, and
image size.

#### Profiles

You can specific the dev profile by passing the `--build-arg profile=dev` option
to the `docker build` command. This will build an image that is not compressed,
and is optimised for build speed but not image size.

#### Build arguments

Additionally, there are two other build arguments that can be passed in:

  - `upx`        - Whether to compress the binary using [`upx`][UPX]. Defaults
                   to `1`. Specify `0` to disable compression.
  - `cargo-opts` - Additional options to pass to `cargo build`, for instance
                   `--build-arg cargo_opts="--config opt-level=z"`.

#### Running

It's worth noting that the host IP to serve on needs to be set to `0.0.0.0` to
allow outside traffic to connect. In other words, the `host` entry in the
`Config.toml` file should be set to `"0.0.0.0"` for a Docker setup:

```toml
host   = "0.0.0.0"
port   = 8000
```

By default, Terracotta will run on port `8000`, and this is expected by the
`Dockerfile`. It is therefore advisable to keep this configured as such in the
`Config.toml` file (or omitted), and instead use port mapping to map the
container port to a host port. This can be achieved by specifying the `-p`
option when calling the `docker run` command, for instance:

```sh
docker run -p 8000:8000 terracotta
```

This will make the Terracotta server available on port `8000` on the host
machine, so that, on that machine, you will be able to visit it at
http://localhost:8000 or http://127.0.0.1:8000 in your browser.

If you run Terracotta on a different port, you will need to specify that port in
the `Dockerfile`.

#### Volumes

It is possible to mount volumes into the Docker container, to provide access to
local files. This can be useful for development, and also for providing
additional content and static assets. The following volumes are available:

  - `/usr/src/content` - Protected static assets.
  - `/usr/src/static`  - Public static assets.

These paths, and the options controlling them, can be overridden using the
[local loading options](#local-loading-options) described above.

To mount a volume, use the `-v` option when calling the `docker run` command,
for instance:

```sh
docker run -v /path/to/content:/usr/src/content:ro terracotta
```

It is advisable to specify the `ro` (read-only) option, as shown above, as there
is no reason for Terracotta to need to write to the content files.

#### Examples

Default build, generating a compressed release image:

```sh
docker build -t terracotta .
```

Default build, generating an uncompressed release image:

```sh
docker build -t terracotta --build-arg upx=0 .
```

Dev build, generating an uncompressed dev image:

```sh
docker build -t terracotta --build-arg profile=dev .
```

Adjusting the `opt-level` for the release build:

```sh
docker build -t terracotta --build-arg cargo_opts="--config opt-level=z" .
```

Running the image:

```sh
docker run terracotta
```

Running the image and exposing the default port:

```sh
docker run -p 8000:8000 terracotta
```

Mounting volumes:

```sh
docker run \
  -v /path/to/content:/usr/src/content:ro \
  -v /path/to/assets:/usr/src/static:ro \
  terracotta
```


## Attributions

[Bulma license]:        https://github.com/jgthms/bulma/blob/master/LICENSE
[CC-BY license]:        https://creativecommons.org/licenses/by/4.0/
[Font Awesome license]: https://fontawesome.com/license/free
[MIT license]:          http://opensource.org/licenses/MIT
[Public Domain]:        https://creativecommons.org/publicdomain/zero/1.0/
[Rust logo use]:        https://github.com/rust-lang/rust/issues/11562#issuecomment-50833809
[Rust logo]:            https://github.com/rust-lang/rust/issues/11562#issuecomment-32700278
[Rustacean]:            https://rustacean.net/
[SIL OFL license]:      https://scripts.sil.org/OFL

This project uses the [Rust logo][] as a default, due to being written in Rust.
The logo is [freely usable][Rust logo use] under the [CC-BY (Creative Commons
Attribution) license][CC-BY license].

An image of Ferris the crab (the Rust mascot) is used as an example of protected
content. This image is sourced from [rustacean.net][Rustacean] and is in the
[Public Domain][], so can be freely used.

This project uses the [Bulma CSS framework][Bulma], which is [published][Bulma license]
under the [MIT license][] and free to use without restriction.

The [Font Awesome][] icons are [published][Font Awesome license] under the
[CC-BY (Creative Commons Attribution) license][CC-BY license], and the webfonts
under the [SIL OFL (Open Font License)][SIL OFL license]. They are freely
usable, along with the CSS code used to display them, which is released under
the [MIT license][].


