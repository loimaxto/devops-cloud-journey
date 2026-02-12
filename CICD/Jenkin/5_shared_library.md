## Why need it?
when there are many pipeline that has some common logic in Jenkins file, We will create a repe for reusable code and logic.

### Shared library structure
- **vars folder**: functions that we call from Jenkinsfile, execute Groovy function
- **src folder**: contain helper code
- **resources folder**: external libraries, no groovy files

###
Mange Jenkins -> System ->  Global pipeline librareis 