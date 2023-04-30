Download Link: https://assignmentchef.com/product/solved-mips-emulator-project-2-cs-3339
<br>
<h1>PROBLEM STATEMENT</h1>




In this project, you will write an emulator that reads MIPS machine instructions from the provided binary executable files and correctly simulates their behavior i.e. emulates a MIPS CPU that executes the program.




Write your emulator code in the provided C++ code skeleton, which models several components of a CPU.  I have provided the following files:

<ul>

 <li>A Memory class (h, Memory.cpp) that emulates a byte-addressed memory with load and store operations</li>

 <li>An ALU class (h, ALU.cpp) that performs arithmetic operations given an opcode and two 32bit sources</li>

 <li>A CPU class (h, CPU.cpp) that simulates the behavior of a CPU, i.e. fetching instructions from instruction memory, decoding them, and executing them. The CPU includes a program counter and a register file. It can interact with instruction and data memory banks and instantiates an ALU object on which to execute instructions.</li>

</ul>

<strong>All your changes go in the file </strong><strong>CPU.cpp</strong><strong>, and only in the </strong><strong>decode()</strong><strong> function. </strong>

<ul>

 <li>A driver (cpp) that loads the instruction memory with words from the executable and invokes the CPU execution</li>

 <li>A debug macro (Debug.h) that will turn on additional print statements</li>

 <li>A Makefile</li>

</ul>




Although you only need to change CPU.cpp, you should read and understand all of the given code, as it will form the basis of all the remaining projects.




The jal and trap instructions are already (partially) implemented for you as an example.  You need to implement all the rest of the instructions from Project 1.  Note that depending on how you structure your code, you may also need to add statements to my partial implementation of jal and trap!  <strong><em>The provided implementations do not give meaningful values to all of the control signals.  Remember never to leave a control signal floating at an undefined or unsafe value.</em></strong>




The CPU skeleton already includes code to increment the program counter (PC), to fetch the next instruction from instruction memory, and to invoke the ALU to execute a particular ALU op on two sources (as well as to conditionally use the ALU output as the address of a load or store operation or the next PC for a branch or jump instruction).  It also includes code to update the register file with the ALU output or load data.




Your code, which belongs in the decode() function, needs extract the bit-fields from the instruction (just like in Project 1), and then set up all the control signals used by the ALU, load and store mechanism, and the register file writeback.




Unlike the MIPS single-cycle CPU we will cover in class, you should resolve branch and decode instructions inside decode().  Do not use the ALU to compare the sources for conditional branch instructions.  Rather, do the comparison and update the pc field (as is done in the jal example) inside decode().




<strong>Do not change any code outside of the </strong><strong>CPU::decode() function.  </strong>Your decode() code may not update the register file (e.g., no statements like regFile[x] = y; may go in decode) and may not interact with memory (e.g., no calls to loadWord or storeWord in decode).  Use decode() to correctly set up the control signals so that my execute(), mem(), and writeback() functions behave correctly.




<strong> </strong>

<h1>ASSIGNMENT SPECIFICS</h1>




This project is to be submitted individually, and you should be able to explain all code that you submit.  You are encouraged to discuss, plan, design, and debug with fellow students.




<strong><em>The next 3 projects in this class will depend on the code for this project, so it is particularly important to correctly solve it!  </em></strong>To help with debugging, I have provided debug verbosity output for all inputs except nqueens.  You can diff this output with the output created by your emulator (with debug verbosity turned on; see below) in order to identify where program execution begins to go wrong.




The source files are contained in <strong>cs3339_project2.tgz</strong> and the debug-verbosity expected output files (*.debug_out) are  in <strong>cs3339_project2_debug_out.tgz</strong>.




After moving the compressed tar file to your home space on zeus, the following command will extract the files:




$ tar xzvf cs3339_project2.tgz




This will create a directory called project2 that contains several sample executables (*.mips) for testing your emulator, the corresponding expected output files (*.out), and all of the C++ files described above, including the CPU skeleton (CPU.cpp) to which all of your code must be added.




Once you have added the missing code to CPU.cpp, compile the project with this command:




$ make




Assuming the compilation succeeded, you can run your emulator on the provided *.mips files.  For example, to simulate the execution of sssp.mips, run this command:




$ ./simulator sssp.mips




You can remove the executable and all the intermediate object (.o) files with a single command:




$ make clean




Your emulator must generate precisely the same output as in the provided expected output files.  See the Project 1 handout for a reminder about how to redirect console output to a file and use the ‘diff’ utility to compare it to the expected output.




<h2>Debugging</h2>




You can use your solution for Project 1 to disassemble the executables to find out what instructions should be executed.  (Note that most of the Project 1 *.mips files cannot be executed, however).




To use the provided debug_out reference files:




<ul>

 <li>Turn on additional output by un-commenting out the ‘#define DEBUG’ line in h.</li>

 <li>Recompile your program</li>

 <li>Run your program and redirect the output to a file such as rand_debug_temp.</li>

 <li>Start with the smaller rand.mips and pqueue.mips, don’t run nqueens.mips with debug outputs enabled – it will fill up your drive! Even with the smaller files it’s best to &lt;cntrl-c&gt; after a few seconds, there is usually no need to run them to completion.</li>

 <li>Using the same method as in project 1 diff the provided rand.debug_out and your output file.</li>

 <li>Find the first miscompare; the associated instruction or the one prior is incorrect.  Correct the instruction and repeat the process.</li>

</ul>




If you want to add debugging output to CPU.cpp please do so using the D(x) macro defined in Debug.h so that it will not print in normal operation.  [And note that if you add output, you won’t be able to cleanly diff the expected debug-verbosity output files with mine].




If you get an “unaligned [inst/data] memory access” error when testing your code, it means that your emulator attempted a load (either of an instruction or data) or store that was not to a word-aligned address.  If you get a “[inst/data] memory access out of range” error, it means that your emulator attempted to access a memory location that does not hold either program instructions or data.  In either case, your code either computes load/store or branch/jump addresses incorrectly, or the value in the base register associated with one of those operations is incorrect due to an earlier instruction’s incorrect implementation.  Use my debug-verbosity output files to help you debug why.




Additional Requirements:




<ul>

 <li><strong>Your code must compile with the given </strong><strong>Makefile and run on zeus.cs.txstate.edu </strong></li>

 <li>Add your name to the CPU.cpp header but do not change any code outside of the decode() function in cpp</li>

 <li>Your code must be well-commented, sufficient to prove you understand its operation</li>

 <li>Make sure your code doesn’t produce unwanted output such as debugging messages. (You can accomplish this by using the D(x) macro defined in h)</li>

 <li>Make sure your code is correctly indented and uses a consistent coding style</li>

 <li><strong><em>Do not use TAB characters for indentation!</em></strong> (Use a consistent # of spaces per indent level)</li>

 <li>Clean up your code before submitting: i.e., make sure there are no unused variables, unreachable code, etc.</li>

</ul>







<strong>         </strong>