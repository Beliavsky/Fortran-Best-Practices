# Fortran Best Practices

#### Use free source form rather than fixed source form

#### Use the .f90 suffix for free source form code
The .f90 suffix indicates free source form, not that the code conforms to the Fortran 90 standard. Don't use suffixes such as .f95, .f03, .f08, or .f18, which not all compilers and build systems recognize as free source form Fortran files.

#### Write code in lower case
Code in lower case is more readable than code in all upper case. Comments should be capitalized as normal written language.

#### Use format strings instead of ```format``` statements
It is easier for the reader to understand a ```write``` statement if the format string is on the same line, rather than in a separate numbered format statement. If the same format string is used in multiple ```write``` statements, define it as a ```parameter```.

#### Put functions and subroutines in modules
This enables type-checking of arguments at compile time. If this is not possible, provide an explicit interface for the procedure.

#### Put ```implicit none``` at the beginning of the main program or module
If this is done, it is unnecessary to write ```implicit none``` in each procedure.

#### Use ```intent``` for all procedure arguments
Any argument that is strictly an input should be declared ```intent(in)```.

#### All function arguments should be ```intent(in)```
A function with an ```intent(out)``` argument should be rewritten as a subroutine.

#### Declare functions and subroutines ```pure``` when possible
This tells the reader that they do not have side effects.

#### Do not have more than three or four required arguments for a procedure
Use ```optional``` arguments and derived types (which can store multiple entities) to reduce the number of required arguments.

#### The order of subroutine arguments should be required ```intent(in)``` arguments, followed by required ```intent(out)``` or ```intent(in out)``` arguments, followed by ```optional``` arguments
This will make it easier to call such a subroutine without naming the arguments. Argument declarations should be in the same order as the argument list.

#### Declare a function ```result``` on a separate line
It should appear after the declarations of function arguments but before the declarations of local variables.

#### Avoid using saved variables when possible. When they are necessary use the ```save``` atribute.
In a procedure, ```integer :: i = 0``` has a different meaning in Fortran than the analogous statement in other languages, and one should write ```integer, save :: i = 0``` instead to alert the reader.

#### A procedure call with many arguments should use named arguments
For example, ```call reg(lhs=a,rhs=b,coeff=x,stderr=c,sse=d)``` is clearer than ```call reg(a,b,x,c,d)```

#### In subroutine calls, document the ```intent(out)``` and ```intent(in out)``` arguments with a comment
For example, ```call sub(x1,x2,x3,x4) ! out: x3,x4``` clarifies what the outputs of ```sub``` are.

#### Avoid ```intent(in out)``` arguments when possible
It was common in Fortran 77 libraries to overwrite an input matrix with output to conserve memory, but this is rarely necessary now. Instead use separate ```intent(in)``` and ```intent(out)``` array arguments.

#### Do not write a function with a ```pointer``` ```result```
This is prone to memory leaks when the function is used in an expression.

#### Use ```allocatable``` arrays rather than ```pointer``` arrays when possible
Pointer arrays must be explicitly deallocated to avoid a memory leak.

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

#### If a derived type has ```allocatable``` components that have related shapes, write a subroutine to allocate them consistently
Otherwise the code will need to be updated in several places if a new allocatable component is added to the definition of the derived type. Also write subroutines to read and write the derived type.

#### When opening a file, specify the ```action``` as ```read``` or ```write```

#### Do not write to standard output in a subroutine, except for error handling
It is more flexible to pass a unit number to the subroutine and to write to that unit. The unit number argument can be ```optional```, and the local unit number can be set to standard output if the argument is not ```present```.

#### Use local variables with a _ suffix to shadow ```optional``` arguments
Set ```x_``` to ```optional``` argument ```x``` if it is ```present``` and to a default value otherwise. Consider using the ```optval``` function of stdlib to set such shadow variables.

#### Make a variable whose value is fixed a ```parameter```
This clarifies to the reader and the compiler that the value is fixed and prevents inadvertent changes.

#### Parametrize file units with meaningful names
For example, set a ```parameter``` such as ```data_unit``` and ```output_unit``` and use it in ```read``` and ```write``` statements. If you use integer literals to specify the unit in ```read``` or ```write``` statements, it's easy to mix them up.

#### Explicitly leave spaces between numbers in formatted output
Otherwise the numbers may be "mushed" together if they are larger than expected. Instead of 

```write (*,"(100f12.6)") x(1:n)```

write

```write (*,"(100(1x,f12.6))") x(1:n)```

#### Use list-directed ```read``` to read data.
A formatted ```read``` depends on the data file matching the format exactly and is error-prone.

#### Indent loops, ```if``` blocks, and ```select case``` blocks

#### Label deeply nested loops so that it is clear what the ```end do``` corresponds to

#### Use one-line-if statements instead of if blocks when possible, to be concise

#### Short variable names should follow implicit typing rules, even though ```implicit none``` should be used
The reader of a Fortran program expects variables such as ```i``` or ```i1``` to be integers and ```x``` or ```x1``` to be real.

#### If a program or procedure uses a published algorithm or is derived from another code, cite the publication or code

#### Use compiler options that detect unused variables or parameters, and delete them

#### Avoid ```goto```, except possibly for error handling at the end of a main program or procedure. Avoid numbered lines except to delineate a block of code for error handling.

#### Avoid the ```dimension``` keyword in declarations, except to declare multiple arrays of the same shape
```real :: vec(n1),mat(n1,n2)``` is more concise than </br></br>
```real, dimension(n1) :: vec```</br>
```real, dimension(n1,n2) :: mat``` </br></br>However,</br>
```real, dimension(n1,n2) :: a,b,c``` is a good use of ```dimension```

#### Declare multiple variables of the same type on the same line, for concision
An exception is when you want an inline comment for each variable after its declaration.

#### Use informative loop variable names similar to the name of the upper bound
For example, ```do iday=1,ndays``` or ```do iatom=1,natoms```

#### Don't use array sections when the whole array is referenced
Refer to ```real :: x(n)``` with ```x``` instead of ```x(:)``` or ```x(1:n)```. Use array sections for subsets of an array.

#### Use whole arrays in ```read``` and ```write``` statements instead of implied do loops when possible
Use ```write (*,*) x``` instead of ```write (*,*) (x(i),i=1,n)``` if they have the same effect.

#### If you are passing multiple arrays to a procedure that must have the same dimensions, consider putting them in a derived type
For example, if a subroutine accepts arguments ```days(:)```, ```months(:)```, ```years(:)``` that have a common length, probably a derived type should be defined containing (day,month,year). Then an array of that type can be passed.

#### Use explicit type conversions
For ```real(kind=wp) :: x``` and ```integer :: i``` write ```x = real(i,kind=wp)``` instead of ```x = i``` to make it clear that the conversion from integer to real is intentional. Do not use ```x = dble(i)``` which is less flexible.

#### Separate procedures in a module with a blank or comment line
This aids legibility.

#### Use ```end subroutine foo``` or ```end function foo``` or ```end program foo``` instead of just ```end```
It helps the reader see what program unit is being terminated.

#### Separate terms in an arithmetic expression and the LHS and RHS with spaces
For example write ```z = c*x + d*y```

#### Use integer exponentiation when possible
Write ```x**2``` not ```x**2.0``` . The compiler may produce more efficient code for the former, which makes clear that the variable is raised to an exact integer power.

#### Beware of integer overflow
Write ```(1.0/i)**n``` instead of ```1.0/i**n``` since the denominator may overflow in the latter expression.

#### Collect physical and mathematical constants in a single module that is used where needed
Avoid defining a constant such as pi in multiple places.

#### Use [] instead of (//) for array constructors
This has been standard since Fortran 2003 and is supported by currently maintained compilers. The syntax is shorter and resembles that of other languages.

#### Do not rely on list-directed output having the same appearance across compilers
Use formatted output instead when this is desired.

#### Use the * unlimited repeat count edit descriptor of Fortran 2008 instead of a large integer repeat count
The danger of ```write (*,"(a,1000(1x,i0))") "abc",ivec(:)``` is that ```size(ivec) > 1000``` will cause an error. Instead use the format string </br>```"(a,*(1x,i0))"```

#### Do not rely on integer or real variables being initialized to zero
The Fortran standard does not require this. Set such variables to zero explicitly if the code depends on this. Use a compiler option that sets variables to NaN or Inf so that the use of unitialized variables is detected.

#### Echo the values of input parameters read from a file
A common error is for the input file not to match the ```read``` statements of a program.

#### Print the names of output files created
If many output files are created in a directory, at least print the directory name.

#### For production programs, print the results of the ```compiler_version``` and ```compiler_options``` intrinsics and the executable name by calling ```get_command_argument```
This will make it easier to reproduce results.

#### Do not rely on short-circuiting in compound logical expressions or the ```merge``` function
It is not safe, for example, to write ```if (present(a) .and. a > 0) ainv = 1/a``` . A nested ```if``` where the condition ```present(a)``` is tested first should be used instead. 

#### Do not use the ```random_number``` intrinsic if you want exactly reproducible results across compilers
Compilers implement ```random_number``` differently. This is beneficial if you want to verify that simulation results are robust to the RNG algorithm used.

#### If you want reproducible results with ```random_number``` for a single compiler, set the seed explicitly
Some but not all compilers produce the same stream of variates on each run when ```random_number``` is called without a prior call to ```random_seed```.

#### Compile with a standard conformance option and fix code that is not conformant. Consider fixing code that uses obsolescent features.
Use for example ```gfortran -std=f2018``` or ```ifort -stand:f18```

#### Compile with bounds-checking and other debugging options, until speed is needed for a production version.

#### Regularly compile and run your code with multiple compilers
Gfortran and Intel Fortran are both free and mature and should both be used, at the minimum. If you have a Fortran 95 code, consider testing it with g95, a mature but unmaintained Fortran 95 compiler.

#### If you compile on Windows, also compile on Windows Subsystem for Linux to verify portability
Two sources of non-portability are Linux have case-sensitive file and directory names and Linux using a different directory separator.
