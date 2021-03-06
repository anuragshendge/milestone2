# CSC 591/791 Milestone2 - Test + Analysis
## Team members
1. Krishna Teja Dinavahi (kdinava) 
2. Aneesh Kher (aakher)
3. Anurag Shendge (ashendg)  

- - - 
 
## Project used to test the build
For this milestone, we wrote a small JavaScript program to perform some basic arithmetic operations. This script is a standalone script with no library dependencies. We have included Unit tests, code coverage, static analysis, extended static analysis, and security checks as build steps in order to determine the success or failure of a build. Our project uses the following test and analysis components.

1. Unit Tests
2. Code Coverage
3. Static Analysis
4. Extended Static Analysis
5. Security Checks

- - - 


## Test Section
#### Test setup  
For this project, we have used Jenkins CI to run our build scripts, which consist of the scripts for unit tests, code coverage, static analysis and security checks. We have installed Jenkins on a remote server which we brought up through [DigitalOcean](https://www.digitalocean.com/). On our GitHub repository, we have added a Jenkins Hook URL with which the git repository will communicate whenever an event occurs on the git repository. When a developer commits and pushes code (assuming the secrity check has passed), git communicates with the Jenkins server and the Jenkins server pulls the code from git, and runs the CI tests to report the result.  

#### Test Components
##### 1. Unit tests
We have used the `mocha` JavaScript framework for running our unit tests. A developer will write unit tests as per the return values of the functions. The mocha framework will execute those unit tests, and produce a report format based on the argument provided. In Jenkins, the unit test is run by executing `npm run unit-test` on the execute shell. The execution status is checked by the shell, and the build is failed unless all the unit tests pass. The unit test file `test_project.js` is located in the `test/` directory.

Here is a code snippet of the mocha unit tests
```javascript
describe('Basic addition', function() {
	it('Returns a positive number', function(done) {
		var answerPos = sample.addNumbers(34,45,56,67);
		expect(answerPos).to.equal(202);
		done();
	});

	it('Returns a negative number', function(done) {
		var answerNeg = sample.addNumbers(-3, -4, 4, -7);
		expect(answerNeg).to.equal(-10);
        done();
	});

	it('Returns an undefined value', function(done) {
		var undef = sample.addNumbers(-3, "GarbageString", 4, "Random");
		expect(undef).to.equal(undefined);
		done();
	});
});
```  
  
The unit test code listed above will perform unit tests for the `addNumbers` method in the `sample.js` project file that we have used.  
  

##### 2. Code Coverage
For code coverage, we used the tool introduced to us in [Homework 2](https://github.com/CSC-DevOps/Course/blob/master/HW/HW2.md), and extended it to cover branches and statements of our code. We have used `istanbul` to report our coverage, and have written a wrapper script in Perl to scrape the data and return a non zero exit code in case the coverage is below a threshold specified by us. The test case generation file `main.js` is located in the `app/` directory. As part of the build process, Jenkins will execute `main.js`, which in turn will generate test cases for `sample.js` and will put them in `test.js`. This is done only on the production server on which Jenkins is running. Istanbul will use this file to generate the coverage report.  
Here is a sample of the output generated by the istanbul library for code coverage.  
<pre>
=============================== Coverage summary ===============================
Statements   : 72.41% ( 42/58 )
Branches     : 55.56% ( 20/36 )
Functions    : 100% ( 6/6 )
Lines        : 72.41% ( 42/58 )
================================================================================

</pre>  
  


- - -
  
  
## Analysis section
#### Analysis Components
##### 1. Static Analysis (Base Analysis)
For static analysis, we have used `JSHint` on our source file. The JSHint tool shows various types static errors like, semicolon not present, function library not imported, and so on. These errors can be configured using the `.jshintrc` file to suite your requirements. For example, to force the programmer to include all libraries, the `undef` flag can be set in the .jshintrc file, which will produce an error if the libraries of which functions are being used, aren't required. The build will fail in case the static analysis check is not successful.  
  
Here are some of the errors that `jshint` will report.  
<pre>
sample.js: line 21, col 2, Unnecessary semicolon.
sample.js: line 31, col 13, Expected '===' and instead saw '=='.
sample.js: line 35, col 13, Expected '===' and instead saw '=='.
sample.js: line 84, col 35, Expected '===' and instead saw '=='.
sample.js: line 95, col 2, Unnecessary semicolon.
sample.js: line 101, col 28, Expected '===' and instead saw '=='.

6 errors
</pre>  

##### 2. Extended Static Analysis
To extend the static analysis checks that are being run, we have developed a script which gives us the ratio of comments in a file to the actual lines of code in a file. If this ratio is below (or above) a certain threshold, the program will return a non zero exit status and fail the build. The `comment.js` script, which analyzes the `sample.js` file, is located in the project root directory.   

- - -

## Security check  
In order to protect the private key files and AWS/Digital Ocean tokens, we have developed scripts to check for tokens in the repository files and for some commonly known security files, like id_rsa and .pem files. When code is committed, this script is run through a pre-commit git hook, which will perform the checks. If any of the tokens or files are found in the repository, the script will return a non zero exit status, which will block the commit from going through.  

For achieving this, we have two scripts, `security_dir.js` and `securitygate.js` which perform the analysis. The scripts are run in the form of a pre-commit hook, the contents of which are in the `pre-commit` file.  

- - -

## Screencast  
Here is a link to the screencast demonstrating the test and analysis process. We have demonstrated each step with one failed case and one working case.

### (Click the image below  and you will be redirected to YouTube)
[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/azplTOtj5m4/0.jpg)](https://youtu.be/azplTOtj5m4)

