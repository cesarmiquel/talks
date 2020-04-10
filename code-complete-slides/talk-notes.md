Titles:

 **Code Complete**, &iquest;porqué es "La Biblia" del programador?
 
Temario:

  1. &iquest;Qué es **Code Complete**?
  2. Como escribir rutinas/métodos de gran calidad
  3. Consejos útiles en el uso de variables.
  4. 'Self-documenting code' y tips para comentar
  5. Debugging

Aspetos claves del libro:

 * El buen código es simple, corto y entendible. 

# High Quality Routines

## C++ Example of a Low-Quality Routine

    void HandleStuff( CORP_DATA & inputRec, int crntQtr, EMP_DATA empRec,
    double & estimRevenue, double ytdRevenue, int screenX, int screenY,
    COLOR_TYPE & newColor, COLOR_TYPE & prevColor, StatusType & status,
    int expenseType )
    {
        int i;
        for ( i = 0; i < 100; i++ ) {
             inputRec.revenue[i] = 0;
             inputRec.expense[i] = corpExpense[ crntQtr ][ i ];
        }
        UpdateCorpDatabase( empRec );
        estimRevenue = ytdRevenue * 4.0 / (double) crntQtr;
        newColor = prevColor;
        status = SUCCESS;
        if ( expenseType == 1 ) {
        for ( i = 0; i < 12; i++ )
            profit[i] = revenue[i] - expense.type1[i];
        }
        else if ( expenseType == 2 ) {
            profit[i] = revenue[i] - expense.type2[i];
        }
        else if ( expenseType == 3 )
            profit[i] = revenue[i] - expense.type3[i];
    }

* The routine has a bad name. *HandleStuff()* tells you nothing about what the routine does.
* The routine isn’t documented. (The subject of documentation extends beyond the boundaries of individual routines and is discussed in Chapter 32, “Self-Documenting Code.”)
* The routine has a bad layout. The physical organization of the code on the page gives few hints about its logical organization. Layout strategies are used haphazardly,
with different styles in different parts of the routine. Compare the styles where expenseType == 2 and expenseType == 3. (Layout is discussed in Chapter 31, “Layout and Style.”)
* The routine’s input variable, inputRec, is changed. If it’s an input variable, its value should not be modified (and in C++ it should be declared const). If the value of the variable is supposed to be modified, the variable should not be called inputRec.
* The routine reads and writes global variables — it reads from corpExpense and writes to profit. It should communicate with other routines more directly than by reading and writing global variables.
* The routine doesn’t have a single purpose. It initializes some variables, writes to a database, does some calculations—none of which seem to be related to each other in any way. *A routine should have a single, clearly defined purpose.*
* The routine doesn’t defend itself against bad data. If crntQtr equals 0, the expression ytdRevenue * 4.0 / (double) crntQtr causes a divide-by-zero error.
* The routine uses several magic numbers: 100, 4.0, 12, 2, and 3. (Magic numbers are discussed in Section 12.1, “Numbers in General.”)
* Some of the routine’s parameters are unused: screenX and screenY are not referenced within the routine.
* One of the routine’s parameters is passed incorrectly: prevColor is labeled as a reference parameter (&) even though it isn’t assigned a value within the routine.
* The routine has too many parameters. The upper limit for an understandable number of parameters is about 7; this routine has 11. The parameters are laid out in such an unreadable way that most people wouldn’t try to examine them closely or even count them.
* The routine’s parameters are poorly ordered and are not documented. (Parameter ordering is discussed in this chapter. Documentation is discussed in Chapter 32.)

# Valid Reasons to Create a Routine

## Reduce complexity

The single most important reason to create a routine is to reduce a program’s complexity. Create a routine to hide information so that you won’t need to think about it. Sure, you’ll need to think about it when you write the routine. But after it’s written, you should be able to forget the details and use the routine without any knowledge of its internal workings.

## Introduce an intermediate, understandable abstraction

Instead of reading a series of statements like

    if ( node <> NULL ) then
      while ( node.next <> NULL ) do
        node = node.next
        leafName = node.name
      end while
    else
      leafName = ""
    end if

you can read a statement like this:

    leafName = GetLeafName( node )

## Avoid duplicate code

 * Undoubtedly the most popular reason for creating a routine is to avoid duplicate code. 
 * creation of similar code in two routines implies an **error in decomposition**
 * With code in one place, you save the space that would have been used by duplicated code. 

 
## Hide sequences 

* It’s a good idea to hide the order in which events happen to be processed.
* A simple example of a sequence might be found when you have two lines of code that read the top of a stack and decrement a stackTop variable. Put those two lines of code into a *PopStack()* routine 
* this will hide the assumption about the order in which the two operations must be performed.  * Hiding that assumption will be better than baking it into code from one end of the system to the other.

## Improve portability

 * isolates nonportable capabilities
 * nonstandard language features
 * hardware dependencies
 * operating-system dependencies

## Simplify complicated boolean tests

 * Understanding complicated boolean tests in detail is rarely necessary for understanding program flow.
 * the details of the test are out of the way
 * a descriptive function name summarizes the purpose of the test

## Improve performance 

 * You can optimize the code in one place instead of in several places
 * make it easier to profile to find inefficiencies.
 * Centralizing code => single optimization benefits all the code
 * Easier to recode the routine with a more efficient algorithm or in a faster, more efficient language.

# Operations That Seem Too Simple to Put Into Routines

 * One of the strongest mental blocks to creating effective routines is a reluctance to create a simple routine for a simple purpose. 
 * creating a routine to contain two or three lines of code might seem like overkill
 * Improves readability

Example:

    points = deviceUnits * ( POINTS_PER_INCH / DeviceUnitsPerInch() )
    
This can be quickly turned into a rutine called *DeviceUnitsToPoints()*

    points = DeviceUnitsToPoints( deviceUnits )

* This line is more readable—even approaching self-documenting.

    Function DeviceUnitsToPoints( deviceUnits: Integer ) Integer;
      if ( DeviceUnitsPerInch() <> 0 )
        DeviceUnitsToPoints = deviceUnits *
          ( POINTS_PER_INCH / DeviceUnitsPerInch() )
      else
        DeviceUnitsToPoints = 0
      end if
    End Function

# Good Routine Names

## Describe everything the routine does In the routine’s name
 * describe all the outputs and side effects
 * If you have routines with side effects, you’ll have many long, silly names. The cure is not to use less-descriptive
routine names; the cure is to program so that you cause things to happen directly rather than with side effects.

## Avoid meaningless, vague, or wishy-washy verbs##
 * Avoid  names like *HandleCalculation()*, *PerformServices()*,
*OutputUser()*, *ProcessInput()*, and *DealWithOutput()*
 * Sometimes the only problem with a routine is that its name is wishy-washy: *HandleOutput()* is replaced with *FormatAndPrintOutput()*
 * The routine suffers from a weakness of purpose, and the weak name is a symptom.

## To name a function, use a description of the return value

* Good examples: *customerId.Next()*, *printer.IsReady()*, and *pen.CurrentColor()*

## To name a procedure, use a strong verb followed by an object

* Good examples: *PrintDocument()*, *CalcMonthlyRevenues()*, *CheckOrderlnfo()*, and *RepaginateDocument()*
* In object-oriented languages, you don’t need to include the name of the object
* You invoke routines with statements like *document.print()*, *orderInfo.check()*, and *monthlyRevenues.calc()*
* *document.printDocument()* is redundant

## Use opposites precisely 

* Using naming conventions for opposites helps consistency, which helps readability
* Opposite-pairs like first/last are commonly understood.
* Opposite-pairs like FileOpen() and _lclose() are not symmetrical and are confusing.

    add/remove increment/decrement open/close
    begin/end insert/delete show/hide
    create/destroy lock/unlock source/target
    first/last min/max start/stop
    get/put next/previous up/down
    get/set old/new

# How long should a routine be

* 50-150 lines is ideal
* Should avoid longar than 200.

# How to use routine parameters

* Put parameters in input-modify-output order
* If several routines use similar parameters, put the similar parameters in a consistent
order
* Use all the parameters *Unused parameters are correlated with an increased error rate.*
* Don’t use routine parameters as working variables. Use local variables instead.


* Document interface assumptions about parameters.
* Document the assumptions as you make them.
* Even better: use assertions in code
* What to document?
  * Whether parameters are input-only, modified, or output-only
  * Units of numeric parameters (inches, feet, meters, and so on)
  * Meanings of status codes and error values if enumerated types aren’t used
  * Ranges of expected values
  * Specific values that should never appear

* Limit the number of a routine’s parameters to about seven

# CHECKLIST: High-Quality Routines
## Big-Picture Issues
* Is the reason for creating the routine sufficient?
* Have all parts of the routine that would benefit from being put into routines
of their own been put into routines of their own?
* Is the routine’s name a strong, clear verb-plus-object name for a procedure
or a description of the return value for a function?
* Does the routine’s name describe everything the routine does?
* Have you established naming conventions for common operations?
* Does the routine have strong, functional cohesion—doing one and only
one thing and doing it well?
* Do the routines have loose coupling—are the routine’s connections to
other routines small, intimate, visible, and flexible?
* Is the length of the routine determined naturally by its function and logic,
rather than by an artificial coding standard?

## Parameter-Passing Issues
* Does the routine’s parameter list, taken as a whole, present a consistent
interface abstraction?
* Are the routine’s parameters in a sensible order, including matching the
order of parameters in similar routines?
* Are interface assumptions documented?
* Does the routine have seven or fewer parameters?
* Is each input parameter used?
* Does the routine avoid using input parameters as working variables?
* If the routine is a function, does it return a valid value under all possible
circumstances?

# Defensive programming

* Avoid "garbage in, garbage out"
* A good program uses “garbage in, nothing out,” “garbage in, error message out,” or “no garbage allowed in” instead

## Good practices

* Check the values of all data from external sources (file, user, network, other system).
* Barricade Your Program to Contain the Damage Caused by Errors.
* Decide how to handle bad inputs. (more to come)
* Use assertions. Specially in large, complicated programs and in high-reliability programs.

## Assertions

When to use:

* That an input parameter’s value falls within its expected range
* That a file or stream is open (or closed) when a routine begins executing
* That a pointer is non-null

## Error handling techniques

Assertions are used to handle errors that should never occur in the code. How do you handle errors that you do expect to occur?

Options are:

* Return a neutral value.
* Substitute the closest legal value
* Log a warning message to a file
* Return an error code
* Shut down. Some systems shut down whenever they detect an error. This approach is useful in safety-critical applications.

* Normally, you don’t want users to see assertion messages in production code


# Variables

* Declare all variables
* Use naming conventions Establish a naming convention for common suffixes such as Num and No
* Use the cross-reference list to spot both acctNum and acctNo. Show variables but not used
* Initialize each variable close to where it’s first used. 
* In languages that require declaration do both: declare and initialize

## Coding Horror:

    // initialize all variables
    var accountIndex = 0
    var total = 0.0
    var done = false
    
    // ...
    
    // code using accountIndex
    // ..
    
    // code using total
    // ...
    
    //  code using done
    while( ! done ) {
    
    }


## Better version

    // ...
    
    var accountIndex = 0
    // code using accountIndex
    // ..
    
    var total = 0.0
    // code using total
    // ...
    
    var done = false
    //  code using done
    while( ! done ) {
    
    }

Advantages:

* By the time execution of the first example gets to the code that uses done, done could have been modified
* throwing all the initializations together creates the impression that all the variables are used throughout the whole
routine—when
* loops might be built around the code that uses done, and done will need to be reinitialized

## More tips

* Use _final_ or _const_ when possible. Useful in class constants, input-only parameters, and any local variables whose values are intended to remain unchanged after initialization.
* Pay special attention to counters and accumulators. Its a common error to forget to reset it when re-used.
* Take advantage of your compiler’s warning messages. Compilers can warn you of un-initialized vars
* Minimize scope of variables. For the same reason global variables are bad.

Key point on scope: The difference between the “convenience” philosophy and the “intellectual manageability”. Maximizing scope might indeed make programs easy to write, but a program in which any routine can use any variable at any time is harder to understand than a program that uses well-factored routines.

## More tips

* Use each variable for one purpose only. Avoid _temp_ varibales.


Coding horror:

    // Compute roots of a quadratic equation.
    // This code assumes that (b*b-4*a*c) is positive.
    temp = math.sqrt( b*b - 4*a*c )
    root[0] = ( -b + temp ) / ( 2 * a )
    root[1] = ( -b - temp ) / ( 2 * a )
    ...
    // swap the roots
    temp = root[0]
    root[0] = root[1]
    root[1] = temp
    
Better version:

    // Compute roots of a quadratic equation.
    // This code assumes that (b*b-4*a*c) is positive.
    discriminant = b*b - 4*a*c
    root[0] = ( -b + math.sqrt( discriminant ) ) / ( 2 * a )
    root[1] = ( -b - math.sqrt( discriminant ) ) / ( 2 * a )
    ...
    // swap the roots
    oldRoot = root[0]
    root[0] = root[1]
    root[1] = oldRoot

## Mas tips

* Avoid variables with hidden meanings

Examples:

 * The value in the variable pageCount might represent the number of pages printed, unless it equals -1, in which case it indicates that an error has occurred.
 * The variable customerId might represent a customer number, unless its value is greater than 500,000, in which case you subtract 500,000 to get the number of a delinquent account.
 * The variable bytesWritten might be the number of bytes written to an output file, unless its value is negative, in which case it indicates the number of the disk drive used for the output.

In the example with -1 the integer is moonlightning as a boolean

Even if the double use is clear to you, it won’t be to someone else. The extra clarity
you’ll achieve by using two variables to hold two kinds of information will amaze you.

## More tips

* Use all variables


# Variable names

Coding horror:

    x = x - xx
    xxx = fido + self.sales_tax( fido )
    x = x + self.late_fee( x1, x ) + xxx
    x = x + self.interest( x1, x )

Better version:

    balance = balance - lastPayment
    monthlyTotal = newPurchases + self.sales_tax( newPurchases )
    balance = balance + self.late_fee( customerID, balance ) + monthlyTotal
    balance = balance + self.interest( customerID, balance )


# Variable names (cont)

> The most important consideration in naming a variable is that the name fully and
accurately describe the entity the variable represents. An effective technique for coming
up with a good name is to state in words what the variable represents. Often that
statement itself is the best variable name.

Running total of checks written to date: runningTotal, checkTotal  vs. written, ct, checks, CHKTTL, x, x1, x2
Velocity of a bullet train: velocity, trainVelocity, velocityInMph vs. velt, v, tv, x, x1, x2, train
Current date: currentDate, todaysDate vs. cd, current, c, x, x1, x2, date
Lines per page: linesPerPage vs. lpp, lines, l, x, x1, x2

The _Big question_ what is the optimal variable name length?

Too long: numberOfPeopleOnTheUsOlympicTeam, numberOfSeatsInTheStadium, maximumNumberOfPointsInModernOlympics
Too short: n, np, ntm, ns, nsisd, max, points
Just right: numTeamMembers, teamMemberCount, numSeatsInStadium, seatCount, teamPointsMax, pointsRecord

When to use short variables? When the scope is short. Small loop of few lines may use an index called i. It basically says: “This variable is a run-of-the-mill loop counter or array index and doesn’t have any significance outside these few lines of code.”

# Variable names (cont)

* Use opposites precisely

    * begin/end
    * first/last
    * locked/unlocked
    * min/max
    * next/previous
    * old/new
    * opened/closed
    * visible/invisible
    * source/target
    * source/destination
    * up/down

# Variable names (cont)

Loop variables:

 * for short loops you can use i,j,k
 * for nested loops with more code its recommended to use longer var names example:

    for ( teamIndex = 0; teamIndex < teamCount; teamIndex++ ) {
        for ( eventIndex = 0; eventIndex < eventCount[ teamIndex ]; eventIndex++ ) {
            score[ teamIndex ][ eventIndex ] = 0;
        }
    }

Its _much_ clearer to understand:

    score[ teamIndex ][ eventIndex ] 

than

    score[ i ][ j ]

# Variable names: Status variables

> Think of a better name than _flag_ for status variables

Coding horror: 

    if ( flag ) ...
    if ( statusFlag & 0x0F ) ...
    if ( printFlag == 16 ) ...
    if ( computeFlag == 0 ) ...
    flag = 0x1
    statusFlag = 0x80
    printFlag = 16
    computeFlag = 0

Better example:

    if ( dataReady ) ...
    if ( characterType & PRINTABLE_CHAR ) ...
    if ( reportType == ReportType_Annual ) ...
    if ( recalcNeeded = false ) ...
    dataReady = true
    characterType = CONTROL_CHARACTER
    reportType = ReportType_Annual
    recalcNeeded = false


# Variable names: 'temporary' variables

All variables are temporary!

    // Compute solutions of a quadratic equation.
    // This assumes that (b^2-4*a*c) is positive.
    temp = math.sqrt( b^2 - 4*a*c )
    solution[0] = ( -b + temp ) / ( 2 * a )
    solution[1] = ( -b - temp ) / ( 2 * a )

Better example:

    // Compute solutions of a quadratic equation.
    // This assumes that (b^2-4*a*c) is positive.
    discriminant =  b^2 - 4*a*c
    solution[0] = ( -b + math.sqrt( discriminant ) ) / ( 2 * a )
    solution[1] = ( -b - math.sqrt( discriminant ) ) / ( 2 * a )

# Variable names: boolean variables

Good examples: done, error, found, success or ok

Tips:

* Give boolean variables names that imply true or false. Bad example: status and sourceFile. What does it mean if status is true? Does it mean that something has a status? Everything has a status. Does true mean that the status of something is OK? Or does false mean that nothing has gone wrong?
* A good way to test is to use variables like: is_done, is_error or is_found. What is is_status?!
* Don't use negatives: not_done -> if ( ! not_done ) { ... }

# Variable names: language conventions

* Languajes might have conventions.


In Java:

* i and j are integer indexes.
* Constants are in ALL_CAPS separated by underscores.
* Class and interface names capitalize the first letter of each word, including the first word—for example, ClassOrInterfaceName.
* Variable and method names use lowercase for the first word, with the first letter of each following word capitalized—for example, variableOrRoutineName.
* The underscore is not used as a separator within names except for names in all caps.
* The get and set prefixes are used for accessor methods.

# Variable names: short names

> The desire to use short variable names is in some ways a remnant of an earlier age of 
computing. Older languages like assembler, generic Basic, and Fortran limited variable
names to 2–8 characters and forced programmers to create short names

Guidelines for short names:

* Use standard abbreviations (in dictionary)
* Remove all nonleading vowels. (computer becomes cmptr, and screen becomes scrn. apple becomes appl, and integer becomes intgr.)
* Many more in the book


# Varibale names: what names to avoid

* Avoid names with similar meanings. Example: input and inputValue, recordNum and numRecords, and fileNumber and fileIndex
* Avoid variables with different meanings but similar names. Example: clientRecs and clientReps much better to use: clientRecords and clientReports
* Avoid names with numbers. You should probably be using an array.
* Avoid words that are commonly misspelled in English
* Avoid the names of standard types, variables, and routines
* Avoid names containing hard-to-read characters. Nice examples:

eyeChartl - eyeChartI - eyeChartl
TTLCONFUSION - TTLCONFUSION - TTLC0NFUSION
hard2Read - hardZRead - hard2Read
GRANDTOTAL - GRANDTOTAL - 6RANDTOTAL
ttl5 - ttlS - ttlS

# Abusos del lenguaje

* Ejemplo overloading clases en C++
* Abusos de #define (ej: for ever {})
* The International Obfuscated C Code Contest: www.ioccc.org/
* Writing unmaintanable code: http://mindprod.com/jgloss/unmain.html

# Review de codigo de Q3

main unix:

 - https://github.com/id-Software/Quake-III-Arena/blob/master/code/unix/unix_main.c

# 31. Layout and Style
# 32. Self-documenting code
# pg 539: The Devil’s Guide to Debugging

In Dante’s vision of hell, the lowest circle is reserved for Satan himself. In modern
times, Old Scratch has agreed to share the lowest circle with programmers who don’t
learn to debug effectively. He tortures programmers by making them use these common
debugging approaches:
Find the defect by guessing To find the defect, scatter print statements randomly
throughout a program. Examine the output to see where the defect is. If you can’t find
the defect with print statements, try changing things in the program until something
seems to work. Don’t back up the original version of the program, and don’t keep a
record of the changes you’ve made. Programming is more exciting when you’re not
quite sure what the program is doing. Stock up on cola and candy because you’re in
for a long night in front of the terminal.
Don’t waste time trying to understand the problem It’s likely that the problem is
trivial, and you don’t need to understand it completely to fix it. Simply finding it is
enough.
Fix the error with the most obvious fix It’s usually good just to fix the specific problem
you see, rather than wasting a lot of time making some big, ambitious correction
that’s going to affect the whole program. This is a perfect example:
x = Compute( y )
if ( y = 17 )
x = $25.15 -- Compute() doesn't work for y = 17, so fix it
Who needs to dig all the way into Compute() for an obscure problem with the value of
17 when you can just write a special case for it in the obvious place?
Debugging by Superstition
Satan has leased part of hell to programmers who debug by superstition. Every group
has one programmer who has endless problems with demon machines, mysterious
compiler defects, hidden language defects that appear when the moon is full, bad
data, losing important changes, a possessed editor that saves programs incorrectly—
you name it. This is “programming by superstition.”
If you have a problem with a program you’ve written, it’s your fault. It’s not the computer’s
fault, and it’s not the compiler’s fault. The program doesn’t do something different
every time. It didn’t write itself; you wrote it, so take responsibility for it.

> Even if an error at first appears not to be your fault, it’s strongly in your interest to
assume that it is. That assumption helps you debug. It’s hard enough to find a defect
in your code when you’re looking for it; it’s even harder when you assume your code
is error-free. Assuming the error is your fault also improves your credibility. If you
claim that an error arose from someone else’s code, other programmers will believe
that you have checked out the problem carefully. If you assume the error is yours, you
avoid the embarrassment of having to recant publicly later when you find out that it
was your defect after all.
# 
