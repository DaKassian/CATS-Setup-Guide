# Setup-Linux


### Step 0: Install gcc

This program relies on `gcc` to compile the C code. 
Check if `gcc` is installed on your system by running:

	gcc --version

If not installed, refer to
[How to Install GCC Compiler on Linux?](https://www.geeksforgeeks.org/how-to-install-gcc-compiler-on-linux/)
for installation instructions.


### Step 1: Download CATS from GitHub

Open your terminal and run the following command:

	git clone https://github.com/kevinlb1/CATS.git
	cd CATS 


### Step 2: Test compiling the program

Execute:

	make

A new file ``cats`` should have been created.
Use
	
	ls

to list all files. 
If ``cats`` exists (not ``cats.exe``) the compilation was successful and you can continue with [Step 4](#step-4-test-the-execution-of-the-program).

If errors occur, proceed to [Step 3](#step-3-make-the-program-compilable).

### Step 3: Make the program compilable

There are two known problems which cause problems when compiling the program.

1. Double definition of Max
2. Use of IBM CPLEX

The first one causes errors like:

	error: macro "max" requires 2 arguments, but only 1 given

The second one is identified via:

	../featureCalc.h:5:10: fatal error: cplex.h: No such file or directory
    5 | #include <cplex.h>


#### Step 3.1: Fix double definition of Max

The ``max`` function is defined twice in the files ``main.cpp`` and ``Param.cpp``.
For a correct compilation, only one is needed.

Open ``Param.cpp`` in a texteditor or IDE of your choice.
For instance in NANO via:

	nano Param.cpp

Locate and comment out the line containing the double definition of `max` by adding `\\` before `#`.

	#define max(a,b) (a>b?a:b)

Save the changes and either repeat [Step 2](#step-2-test-compiling-the-program) or proceed with [Step 3.2](#step-32-compile-without-using-ibm-cplex) first.


#### Step 3.2: Compile without using IBM CPLEX


The CATS program supports compilation with ``IBM CPLEX``, a powerful tool recommended by the author, especially for larger and more complex calculations. 
However, this tutorial prioritizes simplicity in the compilation process. 
To achieve this, we demonstrate an alternative method that does not require a commercial or educational license for IBM CPLEX.
By default, CATS uses IBM CPLEX, so we'll guide you through changing the configuration to use the ``LPSOLVE`` library instead.

Open ``Makefile`` in a texteditor or IDE of your choice.
For instance in NANO via:

	nano Makefile

Make the following changes:

1. Comment out the lines 10-13 and 16-17 by adding a ``#`` before.
2. Uncomment the lines 23-25 and 29-32 by removing the ``#`` on first position.

Save the changes and retry [Step 2](#step-2-test-compiling-the-program).


### Step 4: Test the execution of the program



Test the execution of the program with a random generation of test files:

	./cats -d arbitrary -goods 5 -bids 10     

If the output resembles the provided example, the setup was successful and you can continue with the [Output Explanation](/Output-Explanation.md)

	Number of non-dominated bids: 10  Number of bidders: 12
	CATS running 1 of 1....

	Goods: 5
	Bids: 10
	Distribution: arbitrary
	Runs: 1
	Seed: 1700762199

	Arbitrary Distribution Parameters:
	Max Substitutable Bids = 5
	Removing HV Neighbor   = 0.1
	Adding DIAG Neighbor   = 0.2
	Additional Location    = 0.9
	Additivity             = 0.2
	Budget Factor          = 1.5
	Resale Factor          = 0.5
	Normal Private Value:
	  Mean                 = 0
	  Standard Deviation   = 30



	Wrote CATS output file "0000.txt".



If you encounter a `./cats: Permission denied error`, grant executable rights:

	sudo chmod +x cats

Retry the test.
