## SET

This top-level directive allows the user to enter data directly into the
[run-time database](NWChem-Architecture#database-structure).
The format of the directive is as follows:
```
SET <string name> [<string type default automatic>] <type data>
```
The entry for variable <name> is the name of data to be entered into the
database. This must be specified; there is no default. The variable
<type>, which is optional, allows the user to define a string specifying
the type of data in the array <name>. The data type can be explicitly
specified as integer, real, double, logical, or string. If no entry for
<type> is specified on the directive, its value is inferred from the
data type of the first datum. In such a case, floating-point data
entered using this directive must include either an exponent or a
decimal point, to ensure that the correct default type will be inferred.
The correct default type will be inferred for logical values if
logical-true values are specified as .true., true, or t, and
logical-false values are specified as .false., false, or f. One
exception to the automatic detection of the data type is that the data
type **must** be explicitly stated to input integer ranges, unless the
first element in the list is an integer that is not a
[range](Getting-Started#input-format-and-syntax-for-directives).
For example,
```
set atomid 1 3:7 21
```
will be interpreted as a list of integers. However,
```
set atomid 3:7 21 
```
will not work since the first element will be interpreted as a string
and not an integer. To work around this feature, use instead
```
set atomid integer 3:7 21
```
which says to write three through seven, as well as twenty-one.

The SET directive is useful for providing indirection by associating the
name of a basis set or geometry with the standard object names (such as
"ao basis" or geometry) used by NWChem. The following input file shows
an example using the SET directive to direct different tasks to
different geometries. The required input lines are as follows:

`title "Ar dimer BSSE corrected MP2 interaction energy" `  
`geometry "Ar+Ar" `  
`  Ar1 0 0 0 `  
`  Ar2 0 0 2 `  
`end`  
`geometry "Ar+ghost" `  
`  Ar1 0 0 0 `  
`  Bq2 0 0 2 `  
`end`  
`basis `  
`  Ar1 library aug-cc-pvdz `  
`  Ar2 library aug-cc-pvdz `  
`  Bq2 library Ar aug-cc-pvdz `  
`end`  
`set geometry "Ar+Ar" task mp2 `  
`scf; vectors atomic; end`  
`set geometry "Ar+ghost" task mp2 `

This input tells the code to perform MP2 energy calculations on an argon
dimer in the first task, and then on the argon atom in the presence of
the "ghost" basis of the other atom.

The SET directive can also be used as an indirect means of supplying
input to a part of the code that does not have a separate input module
(e.g., the [atomic
SCF](Hartree-Fock-Theory-for-Molecules#atomic-guess-orbitals-with-charged_atoms)).
Additional examples of applications of this directive can be found in
the [sample input
files](Getting-Started#water-molecule-sample-input-file), and
its usage with [basis sets](Basis) and
[geometries](Geometry). Also see [database
section](NWChem-Architecture#database-structure) for an example of how to
store an array in the database.
