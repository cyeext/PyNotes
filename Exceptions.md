
## Exceptions

#### Exceptional conditions

- errors
- conditions that are beyond one's expectations

#### Exception in Python

- exception *object*
  - it will terminate the program if not handled (ignored)
  - use *raise* clause to actively make the program go wrong
  - use *try* ... *except* ... clause to handle excpetions
  - use *finally* to do the housekeep

#### Exception class

- BaseException
  - SysExit
  - KeyboardInterrupt
  - Exception
 
#### Custom exception

- *class* SomeException(Exception):

#### Some Built-in Exceptions

|Class Name|Description|
|---|---|
|Exception|The base class for almost all excpetions.|
|AttributeError|Raised when attribute reference or assignment fails.|
|OsError|Raised when the operating system fails performing a task.|
|IndexError|Raised when using a nonexistent index on a sequence. Subclass of LookUpError.|
|KeyError|Raised when using a nonexistent key on a mappping. Subclass of LookUpError.|
|NameError|Raised when name (variable) is not found.|
|SyntaxError|Raised when the code is ill-formed.|
|TypeError|Raised when a built-in operator or function is applied to an object of wrong type.|
|ValueError|Raised when a built-in operator or function is applied to an object with a right type but with an inappropriate value.|
|ZeroDivisionError|Raised when the second argument of a division or modulo is zero.|

#### *raise* clause

- *raise* SomeException
  - SomeException should be a class subclassing BaseException
  - or an instance of BaseException
- *raise* (with no argument)
  - combine with *exception* clause
  - raise the trapped exception again
  - the trapped exception will be the context (cause) of the raised one
  - context will be a part of the error message
- *raise* Exception1 *from* Exception2
  - if Exception2 is None, it will suppress the context from the trapped exception
  - or else, it will make Exception2 the context of Exception1

#### *try* ... *except* ... clause

- catch all
  - *except*:
  - Ctrl+C no longer works
- catch exceptions subclassing Exception
  - *except* Exception:
  - an alternative to catch all
- catch exception and raise again later
  - combine *except*  clause with *raise* cluase
- catch exception object
  - *except* ... *as* ...
- catch multiple exception
  - use multiple *except* clause to handle them differently
  - use *except* (Exception1, Exception2, ...) to cath them in one block

#### *else* clause

- run when everything is ok

#### *finally* clause

- run whether there is an exception or not
j

#### Propagation of exceptions

- without a handler, exceptiosn bubble up
- inside a function --> the place where function is called --> global scope --> stack trace

#### Why *try* ... *except* ... rather than *if* ... *else*

- more efficient
- more "Pythonic"

#### Warning

- warnings module
- all warnings subclass *Warning*

|Function|Description|
|---|---|
|warnings.warn(message, category=None)|used to issue warnings <br> displayed only once|
|warnings.filtwarnings(action, category=warning, ...)|used to filter out warnings <br> action can be 'ignore', 'error', ...|

