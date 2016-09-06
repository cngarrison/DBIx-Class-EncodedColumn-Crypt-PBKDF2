# NAME

DBIx::Class::EncodedColumn::Crypt::PBKDF2 - PBKDF2 support for DBIx::Class::EncodedColumn

# VERSION

version 0.000001

# SYNOPSIS

    __PACKAGE__->add_columns(
        'password' => {
            data_type           => 'text',
            encode_column       => 1,
            encode_class        => 'Crypt::PBKDF2',
            encode_args         => {
                hash_class => 'HMACSHA1',
                iterations => 1000
            },
            encode_check_method => 'check_password',
        }
    )

# DESCRIPTION

# NAME

DBIx::Class::EncodedColumn::Crypt::PBKDF2 - PBKDF2 support for DBIx::Class::EncodedColumn

# NAME

DBIx::Class::EncodedColumn::Crypt::PBKDF2

# ARGUMENTS for `encode_args` - same as args for `Crypt::PBKDF2-`new>

## hash\_class

**Type:** String, **Default:** HMACSHA1

The name of the default class that will provide PBKDF2's Pseudo-Random
Function (the backend hash). If the value starts with a `+`, the `+` will
be removed and the remainder will be taken as a fully-qualified package
name. Otherwise, the value will be appended to `Crypt::PBKDF2::Hash::`.

## hash\_args

**Type:** HashRef, **Default:** {}

Arguments to be passed to the `hash_class` constructor.

## hasher

**Type:** Object (must fulfill role [Crypt::PBKDF2::Hash](https://metacpan.org/pod/Crypt::PBKDF2::Hash)), **Default:** None.

It is also possible to provide a hash object directly; in this case the
`hash_class` and `hash_args` are ignored.

## iterations

**Type:** Integer, **Default:** 1000.

The default number of iterations of the hashing function to use for the
`generate` and `PBKDF2` methods.

## output\_len

**Type:** Integer.

The default size (in bytes, not bits) of the output hash. If a value isn't
provided, the output size depends on the `hash_class` / `hasher`
selected, and will equal the output size of the backend hash (e.g. 20 bytes
for HMACSHA1).

## salt\_len

**Type:** Integer, **Default:** 4

The default salt length (in bytes) for the `generate` method.

## encoding

**Type:** String (either "crypt" or "ldap"), **Default:** "ldap"

The hash format to generate. The "ldap" format is intended to be compatible
with RFC2307, and looks like:

    {X-PBKDF2}HMACSHA1:AAAD6A:8ODUPA==:1HSdSVVwlWSZhbPGO7GIZ4iUbrk=

While the "crypt" format is similar to the format used by the `crypt()`
function, except with more structured information in the second (salt) field.
It looks like:

    $PBKDF2$HMACSHA1:1000:4q9OTg==$9Pb6bCRgnct/dga+4v4Lyv8x31s=

Versions of this module up to 0.110461 generated the "crypt" format, so set
that if you want it. Current versions of this module will read either format,
but the "ldap" format is preferred.

## length\_limit

**Type:** Integer

The maximum password length to allow, for generate and verify functions.
Allowing passwords of unlimited length can allow a denial-of-service attack
in which an attacker asks the server to validate very large passwords.

For compatibility this attribute is unset by default, but it is recommended
to set it to a reasonably small value like 100 -- large enough that users
aren't discouraged from having secure passwords, but small enough to limit
the computation needed to validate any one password.

# METHODS

## make\_encode\_sub($column\_name, \\%encode\_args)

Returns a coderef that accepts a plaintext value and returns an
encoded value.

## make\_check\_sub($column\_name, \\%encode\_args)

Returns a coderef that when given the row object and a plaintext value
will return a boolean if the plaintext matches the encoded value. This
is typically used for password authentication.

# SEE ALSO

- [DBIx::Class::EncodedColumn](https://metacpan.org/pod/DBIx::Class::EncodedColumn)
- [Crypt::PBKDF2](https://metacpan.org/pod/Crypt::PBKDF2)

# AUTHOR

Charlie Garrison <garrison@zeta.org.au>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2016 by Charlie Garrison.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
