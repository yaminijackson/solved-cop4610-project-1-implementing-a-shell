Download Link: https://assignmentchef.com/product/solved-cop4610-project-1-implementing-a-shell
<br>
<strong>Project 1: Implementing a Shell</strong>

The purpose of this project is to familiarize you with the mechanics of process control through the implementation of a shell user interface. This includes the relationship between child and parent processes, the steps needed to create a new process, including search of the path, and an introduction to user-input parsing and verification. Furthermore, you will come to understand how input/output redirection, pipes, and background processes are implemented.

<h1>Problem Statement</h1>

Design and implement a basic shell interface that supports input/output redirection, pipes, background processing, and a series of built in functions as specified below. The shell should be robust (e.g. it should not crash under any circumstance beyond machine failure). The required features should adhere to the operational semantics of the bash shell.

<h1>Project Tasks</h1>

You are tasked with implementing a basic shell. The specification below is divided into parts. When in doubt, test a specification rule against bash. You may access bash on linprog.cs.fsu.edu by logging in and typing the command <em>bash</em>. The default shell on linprog is tcsh.

<h1>Part 1: Parsing</h1>

Before the shell can begin executing commands, it needs to extract the command name, the arguments, input redirection (&lt;), output redirection (&gt;), piping (|), and background execution (&amp;) indicators. Understand the following segments of the project prior to designing your parsing. The ordering of execution of many constructs may influence your parsing strategy. It is also critical that you understand how to parse arguments to a command and what delimits arguments.

<h1>Part 2: Environmental Variables</h1>

Every program runs in its own environment. One example of an environmental variable is $USER, which expands to the current username. For example, if my current username is ‘dennis’, typing:

=&gt; echo $USER outputs:

dennis

In the bash shell, you can type ‘env’ to see a list of all your environmental variables. You will need to use and expand various environmental variables in your shell, and you may use the getenv() library call to do so. The getenv() procedure searchs the environment list for a string that matches the string pointed to by name. The strings are of the fom:

NAME = VALUE

<h1>Part 3: Prompt</h1>

The prompt should always indicate to the user the absolute working directory, who they are, and the machine name. Remember that cd can update the working directory. This is the format:

<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="aefbfdebfceee3efede6e7e0eb">[email protected]</a><em> :</em>: PWD =&gt;

Example:

<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="b1d5d4dfdfd8c2f1ddd8dfc1c3ded682">[email protected]</a> :: /home/grads/dennis/cop4610t =&gt;

<h1>Part 4: Path Resolution</h1>

You will need to convert different file path naming conventions to absolute path names. You can assume that directories are separated with a single forward slash (/).

<ul>

 <li>Directories that can occur anywhere in the path</li>

</ul>

◦ ..

&#x25aa; Expands to the parent of the current working directory

&#x25aa; Signal an error if used on root directory

◦ .

&#x25aa; Expands to the current working directory (the directory doesn’t change)

◦ DIRNAME

&#x25aa; Expands to the child of the current working directory named DIRNAME

&#x25aa; Signal an error if DIRNAME doesn’t exist

&#x25aa; Signal an error if DIRNAME occurs before the final item and is not a directory

<ul>

 <li>Directories that can only occur at the start of the path</li>

</ul>

◦ ~

&#x25aa; Expands to $HOME directory

◦ /

&#x25aa; Root directory

<ul>

 <li>Files that can only occur at the end of the path</li>

</ul>

◦ FILENAME

&#x25aa; Expands to the child of the current working directory named FILENAME

&#x25aa; Signal an error if FILENAME doesn’t exist

You will need to handle commands slightly differently. If the path contains a ‘/’, the path resolution is handled as above, signaling an error if the end target does not exist or is not a file. Otherwise, if the path is just a single name, then you will need to prefix it with each location in the $PATH and search for file existence. The first file in the concatenated path list to exist is the path of the command. If none of the files exist, signal an error.

<h1>Part 5: Execution</h1>

You will need to execute simple commands. First resolve the path as above. If no errors occur, you will need to fork out a child process and then use execv to execute the path within the child process.

<h1>Part 6: I/O Redirection</h1>

Once the shell can handle simple execution, you’ll need to add the ability to redirect input and output from and to files. The following rules describe the expected behavior, note there does not have to be whitespace between the command/file and the redirection symbol.

<ul>

 <li>CMD &gt; FILE</li>

</ul>

◦ CMD redirects its output to FILE

◦ Create FILE if it does not exist

◦ Overwrite FILE if it does exist

<ul>

 <li>CMD &lt; FILE</li>

</ul>

◦ CMD receives input from FILE

◦ Signal an error if FILE does not exist or is not a file

<ul>

 <li>Signal an error for the following</li>

</ul>

<table width="82">

 <tbody>

  <tr>

   <td width="24">◦</td>

   <td width="58">CMD &lt;</td>

  </tr>

  <tr>

   <td width="24">◦</td>

   <td width="58">&lt; FILE</td>

  </tr>

  <tr>

   <td width="24">◦</td>

   <td width="58">&lt;</td>

  </tr>

  <tr>

   <td width="24">◦</td>

   <td width="58">CMD &gt;</td>

  </tr>

  <tr>

   <td width="24">◦</td>

   <td width="58">&gt; FILE</td>

  </tr>

  <tr>

   <td width="24">◦</td>

   <td width="58">&gt;</td>

  </tr>

 </tbody>

</table>

<h1>Part 7: Pipes</h1>

After it can handle redirection, your shell is capable of emulating the functionality of pipes. Pipes should behave in the following manner (again, there does not have to be whitespace between the commands and the symbol):

<ul>

 <li>CMD1 | CMD2</li>

</ul>

◦ CMD1 redirects its standard output to CMD2’s standard input

<ul>

 <li>CMD1 | CMD2 | CMD3</li>

</ul>

◦ CMD1 redirects its standard output to CMD2’s standard input

◦ CMD2 redirects its standard output to CMD3’s standard input

<ul>

 <li>CMD1 | CMD2 | CMD3 | CMD4</li>

</ul>

◦ CMD1 redirects its standard output to CMD2’s standard input

◦ CMD2 redirects its standard output to CMD3’s standard input

◦ CMD3 redirects its standard output to CMD4’s standard input

<ul>

 <li>Signal an error for the following</li>

</ul>

◦ |

◦ CMD |

◦ | CMD

<h1>Part 8: Background Processing</h1>

You will need to handle execution for background processes. There are several ways this can be encountered:

<ul>

 <li>CMD &amp;</li>

</ul>

<table width="522">

 <tbody>

  <tr>

   <td width="24">◦</td>

   <td width="355">Execute CMD in the background</td>

   <td width="143"> </td>

  </tr>

  <tr>

   <td width="24">◦</td>

   <td width="355">When execution starts, print</td>

   <td width="143"> </td>

  </tr>

  <tr>

   <td width="24"> </td>

   <td width="355">[position of CMD in the execution queue]</td>

   <td width="143">[CMD’s PID]</td>

  </tr>

  <tr>

   <td width="24">◦</td>

   <td width="355">When execution completes, print</td>

   <td width="143"> </td>

  </tr>

  <tr>

   <td width="24"> </td>

   <td width="355">[position of CMD in the execution queue]+</td>

   <td width="143">[CMD’s command line]</td>

  </tr>

 </tbody>

</table>

<ul>

 <li>&amp; CMD</li>

</ul>

◦ Executes <em>CMD</em> in the foreground

◦ Ignores &amp;

<ul>

 <li>&amp; CMD &amp;</li>

</ul>

◦ Behaves the sames as CMD &amp; ◦ Ignores first &amp;

<ul>

 <li>CMD1 | CMD2 &amp;</li>

</ul>

◦ Execute <em>CMD1</em> | <em>CMD2</em> in the background ◦ When execution starts, print

[position in the background execution queue]           [CMD1’s PID] [CMD2’s PID] ◦ When execution completes, print

[position in the background execution queue]+       [CMD1 | CMD2 command line]

<ul>

 <li>CMD &gt; FILE &amp;</li>

</ul>

◦ Follow rules for output redirection and background processing

<ul>

 <li>CMD &lt; FILE &amp;</li>

</ul>

◦ Follow rules for input redirection and background processing

<ul>

 <li>Signal an error for anything else</li>

</ul>

◦ Examples includes

&#x25aa; CMD1 &amp; | CMD2 &amp;

&#x25aa; CMD1 &amp; | CMD2

&#x25aa; CMD1 &gt; &amp; FILE

&#x25aa; CMD1 &lt; &amp; FILE

<h1>Part 9: Built-ins</h1>

<ul>

 <li><strong>exit</strong></li>

</ul>

◦ Terminates your running shell process and prints “Exiting Shell…”

◦ Example <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="7e1a1b1010170d3e1217100e0c11194d">[email protected]</a> :: /home/grads/dennis/cop4610t =&gt; exit

Exiting Shell….

(shell terminates)

<ul>

 <li><strong>cd </strong><strong>PATH</strong></li>

</ul>

◦ Changes the present working directory according to the path resolution above

◦ If no arguments are supplied, it behaves as if $HOME is the argument

◦ Signal an error if more than one argument is present

◦ Signal an error if the target is not a directory

<ul>

 <li><strong>echo</strong></li>

</ul>

◦ Outputs whatever the user specifies ◦ For each argument passed to echo

&#x25aa; If the argument does not begin with “$”

<ul>

 <li>Output the argument without modification &#x25aa; If the argument begins with “$”</li>

 <li>Look up the argument in the list of environment variables</li>

 <li>Print the value if it exists</li>

 <li>Signal an error if it does not exist</li>

 <li><strong>etime </strong><strong>COMMAND</strong></li>

</ul>

◦ Record the start time using gettimeofday()

◦ execute the rest of the arguments as per typical execution

&#x25aa; You don’t have to worry about nesting with other built-ins

◦ Record the end time using gettimeofday()

◦ Output the elapsed time in the format of s.us where <em>s</em> is the number of seconds and <em>us</em> is the number of micro seconds (0 padded)

◦ Example <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="36525358585f45765a5f584644595105">[email protected]</a> :: /home/grads =&gt; etime sleep 1

Elapsed Time: 1.000000s

<ul>

 <li><strong>io </strong><strong>COMMAND</strong></li>

</ul>

◦ Execute the supplied commands

&#x25aa; Again you don’t have to worry about nesting with other built-ins

◦ Record /proc/&lt;pid&gt;/io while it executes

◦ When it finishes, output each of the recorded values

&#x25aa; Your output comes after the command finishes

&#x25aa; Your output needs to be in a table format (unlike the io file)

◦ Example <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="82e6e7ececebf1c2eeebecf2f0ede5b1">[email protected]</a> :: /home/grads =&gt; limits sleep 1 rchar:                  0 wchar:                  0 syscr:                  0 syscw:                  0 read_bytes:             0 write_bytes:            0 cancelled_write_bytes:  0

<h1>Restrictions</h1>

<ul>

 <li>Must be implemented in the C Programming Language</li>

 <li>Only fork() and execv() can be used to spawn new processes</li>

</ul>

◦ You can not use system() or any of the other exec family of system calls

<ul>

 <li>You can not use any tokenizing functions like strtok()</li>

 <li>Output must match bash unless specified above</li>

</ul>

<h1>Allowed Assumptions</h1>

<ul>

 <li>No more than three pipes (|) will appear in a single line</li>

 <li>You do not need to handle globs, regular expressions, special characters (other than the ones specified), quotes, escaped characters, etc</li>

 <li>Yo do need to handle expansion of environment variables</li>

</ul>

◦ ls $HOME

&#x25aa; prints contents of home directory

◦ $SHELL

&#x25aa; loads standard shell

◦ $UNDEFINED

&#x25aa; error, undefined environment variable

<ul>

 <li>The user input will be no more than 255 characters</li>

 <li>Pipes and I/O redirection will not occur together</li>

 <li>Multiple redirections of the same type will not appear</li>

 <li>You do not need to implement auto-complete</li>

 <li>You do need to handle zombie processes</li>

 <li>The above decomposition of the project tasks is only a suggestion, you can implement the requirements in any order</li>

 <li>You do not need to support built in commands that are not specified</li>

</ul>

<h1>Extra Credit</h1>

Support multiple pipes (limited by the OS and hardware instead of just 3), input redirection, and output redirection all within a single call.

A novel, useful utility of your choosing. You must provide a written specification on the proper operation of your utility. Some examples include printing other proc files, simple auto-completion, if statements built-ins, loop built-ins, etc. Make sure your utility does not interfere with the other commands (including output).