# <a name="top"></a> ZEN of Fortran

> an opinionated coding guidelines for Fortran poor people:

1. [standard](#standard) compliance is better than *custom or extended*; 
1. [beautiful](#beautiful) is better than *ugly*;
1. [explicit](#explicit) is better than *impl.*;
1. [simple](#simple) is better than *CoMpleX*;
1. [CoMpleX](#simple) is better than *c0mp1|c@ted*;
1. [flat](#flat) is better than *nested*;
1. [s p a r s e](#sparse) is better than *dense*;
1. [readability](#readability) counts;
1. [special](#special) cases aren't special enough to break rules...
1. although [practicality](#special) beats *purity*;
1. [private](#private) is better than *public*;
1. [errors](#errors) should never pass *silently*...
1. unless [errors](#errors) are explicitly *silenced*;
1. to be continued

### References

This list is inspired by many sources

+ [zen of python](https://github.com/ewjoachim/zen-of-python)
+ [The Fortran Company styles](http://www.fortran.com/Fortran_Style.pdf)
+ to be continued

# Motivations

## <a name="standard"></a> Standard
to be written

Go to [TOP](#top)

## <a name="beautiful"></a> Beautiful
to be written

Go to [TOP](#top)

## <a name="explicit"></a> Explicit
Implicit typing can *boost* the development of code-snippets, but

> it makes difficult to debug/read/understand medium/large codes.

Always use *implicit none* statement in:

##### Modules

Once for all at the begining of modules definition

```fortran
module explicit_module
...
implicit none
...
endmodule explicit_module
```

##### Programs

Once for all at the begining of (main) programs definition

```fortran
program explicit_program
...
implicit none
...
endprogram explicit_program
```

##### Procedures

For each procedure (function and subroutine) that is **not** *contained* into a module or program, at the begining of its definition

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

## <a name="simple"></a> Simple
to be written

Go to [TOP](#top)
