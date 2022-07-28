### Using embedded data files in unit tests.


Sometimes for either integration purposes or for practicality you need files of test data to use in your testing.  How  these files are located by the test code has always been somewhat problematic. 

Plisky.Testing suggests using embedded data files for this scenario, it will work and be practical for most small to medium scale file usage.

First create an assembly and place into it all of your test files, set the build action on each to be "Embedded Resource".

