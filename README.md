# <a name="top"></a> [ZEN](https://en.wikipedia.org/wiki/Zen) of Fortran

> an opinionated coding guidelines for Fortran poor people:

1. [standard](#standard) compliance is better than *custom or extended*;
1. [beautiful](#beautiful) is better than *ugly*;
    1. [readability](#readability) counts;
    1. [explicit](#explicit) is better than *impl.*;
    1. [simple](#simple) is better than *CoMpleX*;
    1. [CoMpleX](#simple) is better than *c0mp1|c@ted*;
    1. [flat](#flat) is better than *nested*;
    1. [s p a r s e](#sparse) is better than *dense*;
1. [fast](#fast) is better than *slow*:
    1. [vector](#vector) is better than *loop*;
    1. [matrix](#matrix) is better than *vector*;
    1. [strided](#strided) is better than *scattered*;
    1. [contiguous](#contiguous) is better than *strided*;
    1. [broadcasting](#broadcasting) is a great idea, use where possible;
1. [slow](#fast) is better than *unmaintainable*;
1. make it look like the [math](#math);
1. [special](#special) cases aren't special enough to break rules...
1. although [practicality](#special) beats *purity*;
1. [pure](#pure) procedure is better than *impure*...
1. although [practicality](#pure) beats *purity* again;
1. [private](#private) is better than *public*;
1. [errors](#errors) should never pass *silently*...
1. unless [errors](#errors) are explicitly *silenced*;
1. to be continued

### References

This list is inspired by many sources:

<a name="zen-of-python"></a>[1] [zen of python](https://github.com/ewjoachim/zen-of-python).

<a name="std-03"></a>[2] [Fortran 2003 standard](http://www.j3-fortran.org/doc/year/04/04-007.pdf).

<a name="modern-fortran-book"></a>[3] [Modern Fortran: Style and Usage](http://www.amazon.com/Modern-Fortran-Norman-S-Clerman/dp/052173052X), a book by *Norman S. Clerman and Walter Spector*.

<a name="fortran-styles"></a>[4] [The Fortran Company styles](http://www.fortran.com/Fortran_Style.pdf).

<a name="fortran-best-practices"></a>[5] [Fortran best practices](http://www.fortran90.org/src/best-practices.html).

<a name="eu-f90-standards"></a>[6] [European Standards For Writing and Documenting Exchangeable Fortran 90 Code](http://research.metoffice.gov.uk/research/nwp/numerical/fortran90/f90_standards.html).

<a name="cam-standard"></a>[7] [Fortran Coding Standard for the Community Atmospheric Model](http://www.cesm.ucar.edu/working_groups/Software/dev_guide/dev_guide/node7.html).

Go to [TOP](#top)

# Motivations

This Zen of Fortran is an arbitrary guidelines for Fortraners, mainly inspired by [[1]](#zen-of-python). The followings are the opinions on which we base our Zen.

## <a name="standard"></a> Standard compliance
*Extensions* to the standardized language are tempting: in general they provide many improvements (e.g. flexibility, better hardware support, special cases handling, etc...), but *everything comes at a price*. In particular

> **standard compliance** codes are, in general, **portable**, whereas portability is, in general, lost in non standard codes.

Portable is better than *specialized*, a portable code is:

+ free from a particular OS/hardware architecture:
    + ensure long-time code's life: the OS/hardware evolution has lower impact on portable code than on non standard one (the improvement/maintenance costs are strongly reduced);
    + allow easy code-enabling on new hardware: standard code can run on any architecture for which a standard compiler is provided;
+ free from a particular compiler Vendor:
    + standard codes are not chained to a particular Vendor, i.e. standard codes can be built by any compilers supporting the selected standard;
    + the possibility to use different compilers concurrently provide a much high level cross-checking/debugging than a non portable code that can be built by only one compiler.

> select your preferred standard (no matter if 77, 90-95, 2003-2008 or futures) and try to develop standard compliance codes as much as possible.

**Note**: compilers often provide special options to check the standard adherence of the sources compiled, e.g. Intel Fortran compiler provides `-stdXX` and GNU GCC gfortran provides `-std=fXX` (replace `XX` with standard year YY).

Go to [TOP](#top)

## <a name="beautiful"></a> Beautiful: Readability & Co.
There is a common wrong feeling about computer codes: *only the results count, no matter the form* of your code is. This is wrong from many points of view, but mostly remember that

> in addition to writing code for the computer, you are also writing code for humans, yourself included. The purpose of the calculations and the methods used to do so must be clear.

see [[3]](#modern-fortran-book). This means that results count, but also *readability* is a value: a readable and clear (and possible concise) code can be easily debugged, improved and *maintained* (e.g. ported on new architecture). As a consequence, the time you spend on beautify your code is not wasted: it will reduce future works, it will allow easy collaboration with new developers, it will make easy to evolve the code itself.

The followings are some guidelines to try to develop *beautiful* (in the sense of readable/clear) codes:

1. [readability](#readability);
1. [explicit](#explicit);
1. [simple](#simple);
1. [flat](#flat);
1. [s p a r s e](#sparse).

### <a name="readability"></a> Readability

> An advice: coding *styles* are somehow arbitrary, 100 programmers have 101 different styles, the styles *evolution* is a fact. Our Zen is consequently not steady, it will evolves during time.

To improve the readability, we agree on some guidelines:

##### Use free source form

From the standard 90, Fortran language allows (and promotes) *free source form*.

> We recommend that free source form always be used in new code.

Free source form offers a number of advantages with respect fixed source form, see [[3]](#modern-fortran-book) for a more comprehensive list. Essentially, *132 characters are better than 72*. Moreover, the availability of modern wide screen allows the exploit of the new free/long line: use all the spaces you need, do not limit yourself.

The most common counter-reason to prefer *short line*, e.g. 80 characters see [[4]](#fortran-styles), is based on the eye's (in)capability to follow long line: eyes prefer vertical column order rather than horizontal row one, e.g. newspapers are typically organized with multiple columns per page. We agree on this, but this does not constitute a *rule of thumb*: especially for mathematical formula, it could be preferable to avoid splitting on multiple lines as much as possible.

> Use your feeling: it is not mandatory to fill each 132 character of line, but it is not necessary to stay on 72.

For example, considering the following snippet:

```fortran
...
associate(Ni=>self%Ni, Nj=>self%Nj, ...)
  do k=1, Nk
    do j=1, Nj
      do i=1, Ni
        do v=1, Nv
          if (foo) then
            select case(trim(string(i, j, k, v)))
            case('good')
              x_abscissa(i, j, k, v) = good_transform(abscissa=x_abscissa(i, j, k, v))
            case('bad')
              x_abscissa(i, j, k, v) = bad_transform(abscissa= &
                                                     x_abscissa(i, j, k, v))
            case default
              ...
            endselect
          elseif (bar) then
            ...
          else
            ...
          endif
        enddo
      enddo
    enddo
  enddo
endassociate
...
```
Due to the nested loops and controls the `x_abscissa` computation of `good` case overcomes the 80th column, thus one is tempted to split this line as done in the `bad` branch. However, if we trim out the leading withe spaces (due to the indentation) this statement is *only* 72 characters, thus our eyes should be still able to follow it, no matter if it overcomes the 80th column.

##### Indentation

Old codes, say Fortran 66-77, had faced with many *styles* restrictions, mainly due to the fixed source form (originated from the *punched cards* hardwares). In particular, it was common to *collapse* indentation to save precious line characters. You are now free

> indent as much as possible, everywhere it makes sense.

In particular, directly from [[3]](#modern-fortran-book) and [[4]](#fortran-styles):

+ *use a consistent number of spaces when indenting code*, no matter if you prefer 2, 3, 4 or another number of leading spaces;
+ *increase the indentation of the source code every time the data scope changes*;
+ *indent the block of code statements within all control constructs*;
+ *indent all the code after a named construct so the name stands out*;
+ *break lines in logical places and indent continued lines double the number of spaces that each block is indented*;
+ *use blank lines to separate related parts of a program*.

For a example the following snippet shows good indentation.

```fortran
module indented_module
  implicit none
  private
  public :: foo

  type :: foo
    private
    logical                       :: is_good = .false.
    character(len=:), allocatable :: name
    contains
      private
      procedure, public, pass(self) :: init
  endtype foo

  contains
    pure subroutine init(self, name)
      class(foo),             intent(inout) :: self
      character(*), optional, intent(in)    :: name

      if (allocated(self%name)) deallocate(self%name)
      check_name: if (present(name)) then
                    if (len_trim(adjustl(name)) == 0) exit check_name
                    self%name = trim(adjustl(name))
                  else
                    self%name = 'J. Doe'
      endif check_name
      if (allocated(self%name)) self%is_good = .true.
      return
    endsubroutine init
endmodule indented_module
```

##### White spaces

Fortran language allows an arbitrary number of white spaces between keywords

> exploit white spaces to make readable/clear the code statements also for humans.

**Note** Please, avoid hard *tabs*.

For example the followings are considered best practices:

```fortran
! white spaces around operators
if (foo > bar) then
  x = (y + a / b) * z / (c + d * 0.5)
  y = x * z
endif
! align similar code statements
subroutine foo (name, address, age, contact)
  character(*),           intent(in)  :: name    ! Complete name, first+surname.
  character(*),           intent(in)  :: address ! Home address.
  integer,                intent(in)  :: age     ! Age in year, e.g. 23.
  type(personal_contact), intent(out) :: contact ! Complete contact data.
  ...
endsubroutine foo
! place a space after all commas
x(i, j, k) = foo(i, j, k:k+1)
```

Go to [TOP](#top)

Go to [Readability](#readability)

### <a name="explicit"></a> Explicit
Implicit typing can *boost* the development of code-snippets, but

> it is difficult to debug/read/understand **implicit** medium/large codes.

Always use *implicit none* statement, see [[5]](#fortran-best-practices), in:

##### Modules

Once for all, at the beginning of modules definition

```fortran
module explicit_module
...
implicit none
...
endmodule explicit_module
```

##### Programs

Once for all, at the beginning of (main) programs definition

```fortran
program explicit_program
...
implicit none
...
endprogram explicit_program
```

##### Procedures

For each procedure (function and subroutine) that is **not** *contained* into a module or program, at the beginning of its definition

```fortran
function explicit_function
...
implicit none
...
endfunction explicit_function
```

```fortran
subroutine explicit_subroutine
...
implicit none
...
endsubroutine explicit_subroutine
```

> all procedures should be contained into a module or program: old-fashioned *file-libraries* containing (eventually unrelated) procedures without interfaces should be avoided.

Go to [TOP](#top)

Go to [Explicit](#explicit)

### <a name="simple"></a> Simple
to be written

Go to [TOP](#top)

Go to [Simple](#simple)

### Copyrights [![License](http://i.creativecommons.org/l/by/4.0/80x15.png)]()

This work is licensed under a [Creative Commons Attribution 4.0 Unported License](http://creativecommons.org/licenses/by/4.0/deed.en_US).

Go to [TOP](#top)
