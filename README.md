This is a basic template for creating a diff-based autograder.

# A step-by-step walkthrough for creating a diff-based autograder assignment #

## Create your autograder repo ##

Create an **empty** private repo for your autograder on github.ucsb.edu. Do not initialize it with a readme.

Locally clone this repo (diff-autograder-template) into a directory with the same name as the repo you just created.

```
git clone https://github.com/ucsb-gradescope-tools/diff-autograder-template.git your-repo-name
```

Push this to the remote repo:

```
cd your-repo-name
git remote set-url origin your-repo-url
git push origin master
```

## Create your reference solution ##

Create a directory called REFERENCE-SOLUTION:

```
mkdir REFERENCE-SOLUTION
```

Put your reference solution in this directory. This is what will be used to generate the expected output for tests. You must include a makefile either in the REFERENCE-SOLUTION or EXECUTION-FILES directories.


## (optional) Add execution files if your assignment needs them  ##

If the lab on submit.cs you are converting from has files under "EXECUTION-FILES", then
create a directory called EXECUTION-FILES:

```
mkdir EXECUTION-FILES
```

All files in the EXECUTION-FILES directory will be present when the reference and student solutions are building/running. Use this directory to store any files you want to use in testing (e.g. to use as input for a test).


## (optional) Add build files if your assignment needs them  ##

If the lab on submit.cs you are converting from has files under "BUILD-FILES", then
create a directory called BUILD-FILES:

```
mkdir BUILD-FILES
```

All files in the BUILD-FILES directory will be present when the reference and student solutions are building/running. Use this directory to store your makefile, **unless** you expect students to upload their own. You can also use this directory to store any source file for program code that should be present in the solution, but that the students are not uploading, e.g. secret tests, files that are supplied to the students that they should not modify.


## Write your tests ##

**Warning: editing bash scripts inside a Windows environment renders them unable to be run by Gradescope. This is due to differences in how Windows and Unix handle line endings. To prevent this problem, do all file editing in a Unix/Mac environment or use a tool to convert the files back to the format expected by Unix.**

In diffs.sh, write the tests you want Gradescope to run. See [this page](https://github.com/ucsb-gradescope-tools/gs-diff-based-testing/blob/master/README.md) for further documentation on the test format. For example, the test

```
#@test{"stdout":10}
./helloWorld
```

will run the program `./helloWorld` and record its stdout and stderr. If the stdout from the reference and student programs match, the student is given 10 points; otherwise, they get 0 points. For further examples, see the `diffs.sh` file in [this sample repo](https://github.com/ucsb-gradescope-tools/sample-cpp-autograder).

To see what your reference output will be, run `./MAKE-REFERENCE.sh`. **Don't commit any files generated by this command.** The expected output will be stored in the directory `diffs.sh-reference`. Tests are labeled by their line number in `diffs.sh`, starting from 1.

## Edit `grade.sh` ##

At the top of `grade.sh`, copy all submission files from `/autograder/submission/` to the current directory. For example, if my assignment expects students to only turn in a file named `helloWorld.cpp`, my `grade.sh` would begin with:

```
cp /autograder/submission/helloWorld.cpp .
```

**Only the files copied by `grade.sh` will be used for grading the student's assignment.**

Do not edit anything else inside `grade.sh`.

## (optional) Edit `apt-get.sh` ##

Any dependencies of your assignment (e.g. g++, clang) must be explicitly installed on the Docker container that the tests are running on. This is done through `apt-get.sh`. The `apt-get.sh` from diff-autograder-template will install g++ and clang by default. If your assignment requires different dependencies, edit this file.

## Create your `autograder.zip` ##

Follow the instructions [here](https://github.com/ucsb-gradescope-tools/link-gs-zip-with-repo). (If you have followed the steps in this document, step 1 is already complete.)

## You're done! ##
