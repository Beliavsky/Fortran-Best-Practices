# Fortran Best Practices

#### Use free source form rather than fixed source form

#### Use the .f90 suffix for free source form code
The .f90 suffix indicates free source form, not that the code conforms to the Fortran 90 standard. Don't use suffixes such as .f95, .f03, .f08, .f18, which not all compilers and build systems recognize as free source form Fortran files.

#### Write code in lower case
Code in lower case is more readable than code in all upper case. Comments should be capitalized as normal English.

#### Use format strings instead of format statements
It is easier for the reader to understand a ```write``` statement if the format string is on the same line, rather than in a separate numbered format statement. If the same format string is used in multiple ```write``` statements, define it as a ```parameter```.

#### Put functions and subroutines in modules
This enables type-checking of arguments at compile time.

#### Put implicit none at the beginning of the main program or module
If this is done, it is unnecessary to write ```implicit none``` in each procedure.

#### Use ```intent``` for all procedure arguments
Any argument that is strictly an input should be declared ```intent(in)```.

#### Declare functions and subroutines pure when possible
This tells the reader that they do not have side effects.

#### At the beginning of a function or subroutine, have at least one comment line explaining what it does

#### Have a comment explaining the meaning of each procedure argument, either separately or on the same line as the argument declaration

#### Explicitly leave spaces between numbers in formatted output
Otherwise the numbers may be "mushed" together if they are larger than expected. Instead of 

```write (*,"('coeff:',100f12.6)") x(1:n)```

write

```write (*,"('coeff:',100(1x,f12.6))") x(1:n)```
