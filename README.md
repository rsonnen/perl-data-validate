# Data::Validate - common data validation methods

## SYNOPSIS
      use Data::Validate wq(:math);
  
      if(defined(is_integer($suspect))){
            print "Looks like an integer\n";
      }
  
      my $name = is_alphanumeric($suspect);
      if(defined($name)){
            print "$name is alphanumeric, and has been untainted\n";
      } else {
            print "$suspect was not alphanumeric"
      }
  
      # or as an object
      my $v = Data::Validate->new();
  
      die "'foo' is not an integer" unless defined($v->is_integer('foo'));

## DESCRIPTION
    This module collects common validation routines to make input
    validation, and untainting easier and more readable. Most of the
    functions are not much shorter than their direct perl equivalent (and
    are much longer in some cases), but their names make it clear what
    you're trying to test for.

    Almost all functions return an untainted value if the test passes, and
    undef if it fails. This means that you should always check for a defined
    status explicitly. Don't assume the return will be true. (e.g.
    is_integer(0))

    The value to test is always the first (and often only) argument.

## FUNCTIONS
    new - constructor for OO usage
          new();

        *Description*
            Returns a Data::Validator object. This lets you access all the
            validator function calls as methods without importing them into
            your namespace or using the clumsy
            Data::Validate::function_name() format.

        *Arguments*
            None

        *Returns*
            Returns a Data::Validate object

    is_integer - is the value an integer?
          is_integer($value);

        *Description*
            Returns the untainted number if the test value is an integer, or
            can be cast to one without a loss of precision. (i.e. 1.0 is
            considered an integer, but 1.0001 is not.)

        *Arguments*

            $value
                The potential integer to test.

        *Returns*
            Returns the untainted integer on success, undef on failure. Note
            that the return can be 0, so always check with defined()

        *Notes, Exceptions, & Bugs*
            Number translation is done by POSIX casting tools (strtol).

    is_numeric - is the value numeric?
          is_numeric($value);

        *Description*
            Returns the untainted number if the test value is numeric, or
            can be cast to a number without a any bits being left over.
            (i.e. 1.0 is considered a number, 1.0hello is not.)

        *Arguments*

            $value
                The potential number to test.

        *Returns*
            Returns the untainted number on success, undef on failure. Note
            that the return can be 0, so always check with defined()

        *Notes, Exceptions, & Bugs*
            Number translation is done by POSIX casting tools (strtol).

    is_hex - is the value a hex number?
          is_hex($value);

        *Description*
            Returns the untainted number if the test value is a hex number.

        *Arguments*

            $value
                The potential number to test.

        *Returns*
            Returns the untainted number on success, undef on failure. Note
            that the return can be 0, so always check with defined()

        *Notes, Exceptions, & Bugs*
            None

    is_oct - is the value an octal number?
          is_oct($value);

        *Description*
            Returns the untainted number if the test value is a octal
            number.

        *Arguments*

            $value
                The potential number to test.

        *Returns*
            Returns the untainted number on success, undef on failure. Note
            that the return can be 0, so always check with defined()

        *Notes, Exceptions, & Bugs*
            None

    is_between - is the value between to numbers?
          is_between($value, $min, $max);

        *Description*
            Returns the untainted number if the test value is numeric, and
            falls between $min and $max inclusive. Note that either $min or
            $max can be undef, which means 'unlimited'. i.e.
            is_between($val, 0, undef) would pass for any number zero or
            larger.

        *Arguments*

            $value
                The potential number to test.

            $min
                The minimum valid value. Unlimited if set to undef

            $max
                The maximum valid value. Unlimited if set to undef

        *Returns*
            Returns the untainted number on success, undef on failure. Note
            that the return can be 0, so always check with defined()

    is_greater_than - is the value greater than a threshold?
          is_greater_than($value, $threshold);

        *Description*
            Returns the untainted number if the test value is numeric, and
            is greater than $threshold. (not inclusive)

        *Arguments*

            $value
                The potential number to test.

            $threshold
                The minimum value (non-inclusive)

        *Returns*
            Returns the untainted number on success, undef on failure. Note
            that the return can be 0, so always check with defined()

    is_less_than - is the value less than a threshold?
          is_less_than($value, $threshold);

        *Description*
            Returns the untainted number if the test value is numeric, and
            is less than $threshold. (not inclusive)

        *Arguments*

            $value
                The potential number to test.

            $threshold
                The maximum value (non-inclusive)

        *Returns*
            Returns the untainted number on success, undef on failure. Note
            that the return can be 0, so always check with defined()

    is_equal_to - do a string/number neutral ==
          is_equal_to($value, $target);

        *Description*
            Returns the target if $value is equal to it. Does a math
            comparison if both $value and $target are numeric, or a string
            comparison otherwise. Both the $value and $target must be
            defined to get a true return. (i.e. undef != undef)

        *Arguments*

            $value
                The value to test.

            $target
                The value to test against

        *Returns*
            Unlike most validator routines, this one does not necessarily
            untaint its return value, it just returns $target. This has the
            effect of untainting if the target is a constant or other clean
            value. (i.e. is_equal_to($bar, 'foo')). Note that the return can
            be 0, so always check with defined()

    is_even - is a number even?
          is_even($value);

        *Description*
            Returns the untainted $value if it's numeric, an integer, and
            even.

        *Arguments*

            $value
                The value to test.

        *Returns*
            Returns $value (untainted). Note that the return can be 0, so
            always check with defined().

    is_odd - is a number odd?
          is_odd($value);

        *Description*
            Returns the untainted $value if it's numeric, an integer, and
            odd.

        *Arguments*

            $value
                The value to test.

        *Returns*
            Returns $value (untainted). Note that the return can be 0, so
            always check with defined().

    is_alphanumeric - does it only contain letters and numbers?
          is_alphanumeric($value);

        *Description*
            Returns the untainted $value if it is defined and only contains
            letters (upper or lower case) and numbers. Also allows an empty
            string - ''.

        *Arguments*

            $value
                The value to test.

        *Returns*
            Returns $value (untainted). Note that the return can be 0, so
            always check with defined().

    is_printable - does it only contain printable characters?
          is_alphanumeric($value);

        *Description*
            Returns the untainted $value if it is defined and only contains
            printable characters as defined by the composite POSIX character
            class [[:print:][:space:]]. Also allows an empty string - ''.

        *Arguments*

            $value
                The value to test.

        *Returns*
            Returns $value (untainted). Note that the return can be 0, so
            always check with defined().

    length_is_between - is the string length between two limits?
          length_is_between($value, $min, $max);

        *Description*
            Returns $value if it is defined and its length is between $min
            and $max inclusive. Note that this function does not untaint the
            value.

            If either $min or $max are undefined they are treated as
            no-limit.

        *Arguments*

            $value
                The value to test.

            $min
                The minimum length of the string (inclusive).

            $max
                The maximum length of the string (inclusive).

        *Returns*
            Returns $value. Note that the return can be 0, so always check
            with defined(). The value is not automatically untainted.

## AUTHOR
    Richard Sonnen <sonnen@richardsonnen.com>.

## COPYRIGHT
    Copyright (c) 2004 Richard Sonnen. All rights reserved.

    This program is free software; you can redistribute it and/or modify it
    under the same terms as Perl itself.

