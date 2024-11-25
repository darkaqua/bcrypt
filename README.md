# BCrypt

It has zero third-party dependencies.

Running in sync requires no permissions. Running in async functionality requires
--allow-net

## Import

If you don't want to specify a specific version and are happy to work with
breaking changes, you can import this module like so:

```ts
import * as bcrypt from "jsr:@da/bcrypt";
```

To ensure that you've got a specific version, it's recommend to import this
module specifying a
[specific release](https://github.com/JamesBroadberry/deno-bcrypt/releases) like
so:

```ts
import * as bcrypt from "jsr:@da/bcrypt";
```

## Usage

### Async

The Async implementation requires WebWorkers which require --allow-net to import
Deno standard modules from inside the Worker.

```ts
const hash = await bcrypt.hash("test");
```

To check a password:

```ts
const result = await bcrypt.compare("test", hash);
```

To hash a password with a manually generated salt:

```ts
const salt = await bcrypt.genSalt(8);
const hash = await bcrypt.hash("test", salt);
```

### Sync/Blocking

It is not recommended to use this unless you're running a quick script since the
BCrypt algorithm is computationally quite expensive. For example, if you're
running a server and make multiple calls to it, other requests will be blocked.
Using the Async methods are recommended.

To hash a password (with auto-generated salt):

```ts
const hash = bcrypt.hashSync("test");
```

To check a password:

```ts
const result = bcrypt.compareSync("test", hash);
```

To hash a password with a manually generated salt:

```ts
const salt = bcrypt.genSaltSync(8);
const hash = bcrypt.hashSync("test", salt);
```

### Older versions of Deno and/or BCrypt

Older versions of Deno will require an older version of this library
(specifically < 0.3.0) because of the introduction of `TextEncoder`. However,
older version of this library require the `--unstable` flag when running.

## Warnings

BCrypt v0.3.0 and below should NOT be used with Deno 1.23.0 or later since there's been a [breaking change in how Deno interprets the code](https://github.com/denoland/deno/issues/14900) and completely bypasses the cipher. Ensure that if you're running Deno 1.23.0 or later that you're using the latest version of BCrypt.
