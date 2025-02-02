# NewFangle ![linux](https://github.com/uperl/NewFangle/workflows/linux/badge.svg) ![macos](https://github.com/uperl/NewFangle/workflows/macos/badge.svg)

Unofficial Perl NewRelic SDK

# SYNOPSIS

```perl
use NewRelic;
my $app = NewRelic::App->new('MyApp', $license_key);
my $txn = $app->start_transaction('my transaction');
$txn->end;
```

# DESCRIPTION

This module provides bindings to the NewRelic C-SDK.  Since NewRelic doesn't provide
native Perl bindings for their product, and the older Agent SDK is not supported,
this is probably the best way to instrument your Perl application with NewRelic.

This distribution provides a light OO interface using [FFI::Platypus](https://metacpan.org/pod/FFI::Platypus) and will
optionally use [Alien::libnewrelic](https://metacpan.org/pod/Alien::libnewrelic) if the C-SDK can't be found in your library
path.  Unfortunately the naming convention used by NewRelic doesn't always have an
obvious mapping to the OO Perl interface, so I've added notation (example:
(csdk: newrelic\_version)) so that the C version of functions and methods can be
found easily.  The documentation has decent coverage of all methods, but it doesn't
always make sense to reproduce everything that is in the C-SDK documentation, so
it is recommended that you review it before getting started.

I've called this module [NewFangle](https://metacpan.org/pod/NewFangle) in the hopes that one day NewRelic will write
native Perl bindings and they can use the more obvious NewRelic namespace.

# FUNCTIONS

These may be imported on request using [Exporter](https://metacpan.org/pod/Exporter).

## newrelic\_configure\_log

```perl
my $bool = newrelic_configure_log($filename, $level);
```

Configure the C SDK's logging system.  `$level` should be one of:

- `error`
- `warning`
- `info`
- `debug`

(csdk: newrelic\_configure\_log)

## newrelic\_init

```perl
my $bool = newrelic_init($daemon_socket, $time_limit_ms);
```

Initialize the C SDK with non-default settings.

(csdk: newrelic\_init)

## newrelic\_version

```perl
my $version = newrelic_version();
```

(csdk: newrelic\_version)

Returns the version of the NewRelic C-SDK as a string.

## newrelic\_set\_hostname

```perl
my $bool = newrelic_set_hostname($hostname);
```

Sets the default hostname to be used in the NewRelic UI.  This is the result of
`gethostname` by default, but that might not be usefully meaningful when running in
a docker or similar container.

This requires a properly patched NewRelic C-SDK to work, since the base C-SDK doesn't
currently support setting the language or version.  If you installed with [Alien::libnewrelic](https://metacpan.org/pod/Alien::libnewrelic)
then it should have been properly patched for you.

Returns true if successful, false otherwise.  Normally a failure would only happen if
the NewRelic C-SDK hadn't been patched.

# ENVIRONMENT

- `NEWRELIC_APP_NAME`

    The default app name, if not specified in the configuration.

- `NEWRELIC_LICENSE_KEY`

    The NewRelic license key.

- `NEWRELIC_APP_HOSTNAME`

    The host display name that will be reported to NewRelic, if the `libnewrelic` has been properly
    patched (see `newrelic_set_hostname` above).

# CAVEATS

Unlike the older NewRelic Agent SDK, there is no interface to set the programming
language or version.  Since we are using the C-SDK the language shows up as `C`
instead of `Perl`.

# SEE ALSO

- [NewFangle::App](https://metacpan.org/pod/NewFangle::App)
- [NewFangle::Config](https://metacpan.org/pod/NewFangle::Config)
- [NewFangle::CustomEvent](https://metacpan.org/pod/NewFangle::CustomEvent)
- [NewFangle::Segment](https://metacpan.org/pod/NewFangle::Segment)
- [NewFangle::Transaction](https://metacpan.org/pod/NewFangle::Transaction)

# AUTHOR

Graham Ollis <plicease@cpan.org>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2020 by Graham Ollis.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
