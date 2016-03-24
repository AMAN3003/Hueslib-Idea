


CPLEX LP file format

The CPLEX LP file format provides a facility for entering a problem in a natural, algebraic LP formulation from the keyboard. The problem can be modified and saved from within lpsolve. This procedure is one way to create a file in a format that lpsolve can read. An alternative technique is to create a similar file using a standard text editor and to read it into lpsolve.

The CPLEX LP format is provided as an input alternative to the MPS file format. An LP format file may be easier to generate than an MPS file if your problem already exists in an algebraic format or if you have an application that generates the problem file more readily in algebraic format (such as a C application).

lpsolve will accept any problem saved in an ASCII file provided that it adheres to the following syntax rules.
Syntax Rules of LP File Format

    Anything that follows a backslash (\) is a comment and will be ignored until a return is encountered. Blank lines are also ignored. Blank and comment lines may be placed anywhere and as frequently as you want in the file.

    In general, white space between characters is irrelevant as it is skipped when a file is read. However, white space is not allowed in the keywords used to introduce a new section, such as MAX, MIN, ST, or BOUNDS. Also the keywords must be separated by white space from the rest of the file and must be at the beginning of a line. The maximum line length allowed is 255 characters.

    Skipping spaces may cause lpsolve to misinterpret (and accept) an invalid entry, such as the following:

     x1 x2 = 0

    If the user intended to enter that example as a nonlinear constraint-not valid in LP format, lpsolve would instead interpret it as a constraint specifying that one variable named x1x2 must be equal to zero.

    The problem statement must begin with the word MINIMIZE or MAXIMIZE, MINIMUM or MAXIMUM, or the abbreviations MIN or MAX, in any combination of upper- and lower-case characters. The word introduces the objective function section.

    Variables can be named anything provided that the name does not exceed 16 characters, all of which must be alphanumeric (a-z, A-Z, 0-9) or one of these symbols: ! " # $ % & ( ) / , . ; ? @ _ ` ' { } | ~. Longer names will be truncated to 16 characters. A variable name cannot begin with a number or a period.

    The letter E or e, alone or followed by other valid symbols, or followed by another E or e, should be avoided as this notation is reserved for exponential entries. Thus, variables cannot be named e9, E-24, E8cats, or other names that could be interpreted as an exponent. Even variable names such as eels or example can cause a read error, depending on their placement in an input line.

    Also, the following characters are not valid in variable names (in order to allow for quadratic objective information): ^, *, [ and ].

    The objective function definition must follow MINIMIZE/MAXIMIZE. It may be entered on multiple lines as long as neither a variable, nor a constant, nor a sense indicator is split by a return. For example, this objective function 1x1 + 2x2 +3x3 can be entered like this:

    1x1 + 2x2
    + 3x3

    but not like this:

    1x1 + 2x
    2 + 3x3         \ a bad idea

    because the second style splits the variable name x2 by a return.

    The objective function may be named by typing a name and a colon before the objective function. The objective function name and the colon must appear on the same line. Objective function names must conform to the same guidelines as variable names (Rule 4).

    The constraints section is introduced by the keyword SUBJECT TO. This expression can also appear as such that, st, S.T., or ST. in any mix of upper- and lower-case characters. One of these expressions must precede the first constraint and be separated from it by at least one space.

    Each constraint definition must begin on a new line. A constraint may be named by typing a name and a colon before the constraint. The constraint name and the colon must appear on the same line. Constraint names must adhere to the same guidelines as variable names (Rule 4). If no constraint names are specified, lpsolve will assign the names R1, R2, R3, etc.

    The constraints are entered in the same way as the objective function; however, a constraint must be followed by an indication of its sense and a right-hand side coefficient. The right-hand side coefficient must be typed on the same line as the sense indicator. Acceptable sense indicators are <, <=, =<, >, >=, =>, and =. These are interpreted as =, =, =, =, =, = and =, respectively.

    For example, here is a named constraint:

    time: x1 + x2 <= 10

    The optional BOUNDS section follows the mandatory constraint section. It is preceded by the word bounds or bound in any mix of lower- and upper-case characters.

    Each bound definition must begin on a new line. The format for a bound is ln=xn = un except in the following cases:

    Upper and lower bounds can also be entered separately as ln=xn and xn = un

    with the default lower bound of 0 (zero) and the default upper bound of +8 remaining in effect until the bound is explicitly changed.

    Bounds that fix a variable can be entered as simple equalities. For example, x5 = 5.6 is equivalent to 5.6 <= x5 <= 5.6.

    The bounds +8 (positive infinity) and -8 (negative infinity) must be entered as words: +infinity, -infinity, +inf, -inf.

    A variable with a negative infinity lower bound and positive infinity upper bound may be entered as free, in any mix of upper- and lower-case characters, with a space separating the variable name and the word free. For example, x7 free is equivalent to - infinity <= x7 <= + infinity.

    The file must end with the word end in any combination of upper- and lower-case characters, alone on a line, when it is created with the enter command. This word is not required for files that are read in to lpsolve, but it is strongly recommended. Files that have been corrupted can frequently be detected by a missing last line.

    To specify any of the variables as general integer variables, add a GENERAL section; to specify any of the variables as binary integer variables, add a BINARY section. The GENERAL and BINARY sections follow the BOUNDS section, if one is present; otherwise, they follow the constraints section. Either of the GENERAL or BINARY sections can precede the other. The GENERAL section is preceded by the word GENERAL, GENERALS, or GEN that must appear alone on a line. The following line or lines should list the names of all variables that are to be restricted to general integer values, separated by at least one space. The BINARY section is preceded by the word BINARY, BINARIES, or BIN that must appear alone on a line. The following line or lines should list the names of all variables that are to be restricted to binary integer values, separated by at least one space. Binary variables are automatically given bounds of 0 (zero) and 1 (one), unless alternative bounds are specified in the BOUNDS section, in which case a warning message is issued.

    Here is an example of a problem formulation in LP format where x4 is a general integer:

    Maximize
     obj: x1 + 2 x2 + 3 x3 + x4
    Subject To
     c1: - x1 + x2 + x3 + 10 x4 <= 20
     c2: x1 - 3 x2 + x3 <= 30
     c3: x2 - 3.5 x4 = 0
    Bounds
     0 <= x1 <= 40
     2 <= x4 <= 3
    General
     x4
    End


