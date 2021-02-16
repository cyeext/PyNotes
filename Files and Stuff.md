# Files and Stuff

## Opening Files

- *open* function
  - lives in *io* module but automatically imported
  - file name as its only mandatory argument and returns a file object
  - if the file doesn't exist --> *FileNotFoundError*

### File Modes

- mode argument can take the following values:
  - `'r'` read mode
    - the same effect as not supplying a mode string at all
  - `'w'` write mode
    - create the file if it doesn't exist
    - truncate the existing content if the file exists
  - `'x'` exclusive write mode
    - raises a *FileExistsError* if the file already exists
  - `'a'` append mode
    - keep writing at the end of the file
  - `'+'` read/write mode
    - added to other mode
    - `'r+'` won't truncate, while `'w+'` will
  - `'t'` text mode
    - means your file is treated as encoded Unicode text
      - Decoding and encoding are then performed automatically with UTF-8 as defaut encoding
    - use *encoding* and *errors* keyword argument to specify other encodings and error-handling strategy
    - contains automatic translation of newline characters
      - Python uses *unviersal newline mode*
      - `'\n'`, `'\r'`, `'\r\n'` are recoginized
      - `'rt'` --> `'\r'` and `'\r\n'` are replaced by `'\n'`
      - `'wt'` --> `'\n'` is replaced by the system's default line ending *os.linesep*
    - added to other mode
  - `'b'` binary mode
    - add to othter mode
    - deals with nontextual, binary data, such as a sound clip
    - turns off any text-specific functionality in order to avoid any automatic transformation

## The Basic File Methods

### File-like objects

- *file-like* objects or *streams* are ones supporting a few of the same mothods as a file, most notably either read or write or both
- e.g. objects returned by *urlopen* support *read* and *readline* but not *write* and *isatty*

### Standard Streams

- *sys.stdin*
  - when a program reads from standard input, you can supply text by ...
    - typing
    - link it with standard output of another program, using a *pipe*
- *sys.stdout*
  - recieve text from *print* and prompts for *input*
  - Data written to *sys.stdout*
    - typically appears on your screen
    - can be rerouted to *sys.stdin* of another program with a *pipe*
- *sys.stderr*
  - error messages are written to *sys.stderr*

### Reading and Writing

- *read* method: data --> string
- *write* method: string --> data
- current position
  - each time you call *read*/*write*, it starts from where the last call left

### Random Access

- *seek(offset[, whence])*
  - moves current position to the position described by *offset* and *whence*
    - *offset* is a byte (character) count
    - *whence* specifies where *offset* begins and can take
      - *io.SEEK_SET*/*0*: from the beginning of the file, *offset* is nonnegative
      - *io.SEEK_CUR*/*1*: from the current position
      - *io.SEEK_END*/*2*: from the end of the file, *offset* is nonpositive
- *tell()*
  - returns current position

### Reading and Writing lines

- *readline(limit)* method
  - read a single line
- *readlines* method
  - read all lines of a file and have them returned as a list
- *writelines* method
  - inverse of *readlines*
  - won't add newlines automatically
- *print(values[, file])*
  - automatically adds newlines

### Closing Files

#### Why closing is always preferable

- avoid using up quotas for open files your system might
- avoid uselessly locked against modification
- closing makes your data 'actually' written to the file
  - Python may *buff* the data you've written, if the program crashed, the data would lost
  - use *flush* method to save without closing
- closing a file with *close* method

#### make sure your file is closed

- *try*/*finally* statement
- context manager

 ```python
# open your file here
f = open(filename)
try:
    # write your data here
    f.write()
finally:
    f.close()
 ```

```python
with open(filename) as f:
    f.write(data)

```

## Iterate over File Contents

### One Character/Byte at a Time

- *while* *True*/*break* statement + *read* method

### One Line at a Time

- *while* *True*/*break* statement + *readline* method

### Everything

- *for*...*in*... statement + *read*/*readlines* mothod

### Lazy Line Iteration with fileinput

```python
import fileinput
for line in fileinput.input(filename):
    process(line)

```

### File Iterators

- file objects and streams are actually *iterable*
- you can directly use them in *for* loops to iterate over their lines

