# Fortran Best Practices

#### Use free source form rather than fixed source form

#### Use the .f90 suffix for free source form code
The .f90 suffix indicates free source form, not that the code conforms to the Fortran 90 standard. Don't use suffixes such as .f95, .f03, .f08, .f18, which not all compilers and build systems recognize as free source form Fortran files.

#### Write code in lower case
Code in lower case is more readable than code in all upper case. Comments should be capitalized as normal English.

#### Use format strings instead of ```format``` statements
It is easier for the reader to understand a ```write``` statement if the format string is on the same line, rather than in a separate numbered format statement. If the same format string is used in multiple ```write``` statements, define it as a ```parameter```.

#### Put functions and subroutines in modules
This enables type-checking of arguments at compile time.

#### Put ```implicit none``` at the beginning of the main program or module
If this is done, it is unnecessary to write ```implicit none``` in each procedure.

#### Use ```intent``` for all procedure arguments
Any argument that is strictly an input should be declared ```intent(in)```.

#### All function arguments should be ```intent(in)```.
A function with an ```intent(out)``` argument should be rewritten as a subroutine.

#### Declare functions and subroutines ```pure``` when possible
This tells the reader that they do not have side effects.

#### Do not have more than three or four required arguments for a procedure
Use ```optional``` arguments and derived types to reduce the number of required arguments.

#### At the beginning of a function or subroutine, have at least one comment line explaining what it does

#### Have a comment explaining the meaning of each procedure argument, either separately or on the same line as the argument declaration

#### When importing entities from a module, add the ```only``` qualifier
For example, ```use m, only: func,sub``` instead of ```use m```. This clarifies for the reader what is being imported from each module.

#### Put a ```private``` statement at the beginning of a module. Only make ```public``` what is necessary.
This reduces namespace clashes and tells the reader what entities of a module are exported to other program units.

#### Do not put a ```stop``` statement before the end of the main program or a ```return``` statement before the end of a procedure.
Such statements are unnecessary. Use ```stop``` only to end a program early or ```return``` only to exit a procedure early.

#### Do not ```deallocate``` ```allocatable``` arrays just before the end of a program or procedure
This is unnecessary since such arrays are deallocated when they go out of scope.

#### When opening a file, specify the ```action``` as ```read``` or ```write```

#### Make a variable whose value is fixed a ```parameter```
This clarifies to the reader and the compiler that the value is fixed and prevents inadvertent changes.

#### Parametrize file units with meaningful names
For example, set parameter such as ```data_unit``` and ```output_unit``` and use them in ```read``` and ```write``` statements. If you use integer literals to specify the unit in ```read``` or ```write``` statements, it's easy to mix them up.

#### Explicitly leave spaces between numbers in formatted output
Otherwise the numbers may be "mushed" together if they are larger than expected. Instead of 

```write (*,"('coeff:',100f12.6)") x(1:n)```

write

```write (*,"('coeff:',100(1x,f12.6))") x(1:n)```

#### Use list-directed ```read``` to read data.
A formatted ```read``` depends on the data file matching the format exactly and is error-prone.

#### Indent loops, ```if``` blocks, and ```select case``` blocks

#### Label deeply nested loops so that it is clear what the ```end do``` corresponds to

#### Short variable names should follow implicit typing rules, even though ```implicit none``` should be used
The reader of a Fortran program expects variables such as ```i``` or ```i1``` to be integers and ```x``` or ```x1``` to be real.

#### If a program or procedure uses a published algorithm or is derived from another code, cite the publication or code

#### Use compiler options that detect unused variables or parameters, and delete them

#### Compile with a standard conformance option and fix code that is not conformant
Use for example ```gfortran -std=f2018``` or ```ifort -stand:f18```

#### Regularly compile and run your code with multiple compilers
Gfortran and Intel Fortran are both free and mature and should both be used, at the minimum.

#### Avoid ```goto```, except possibly for error handling at the end of a main program or procedure
