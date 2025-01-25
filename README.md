# Samsung-riscv

This is a RISC-V Talent Development Program, powered by Samsung Semiconductor India Research (SSIR) along with VLSI System Design (VSD).


## Basic details about me
**Name:** Preetham P\
**Email:** preethamp161718@gmail.com\
**College:** Vidyavardhaka college of Engineering\
**Linkdin:** [Preetham P](https://www.linkedin.com/in/preetham-p-79412724a/)
-----------------------------------------------------------------------------------------------------------------------------


<details>
<summary><b>Task 1:</b> Install the RISC-V toolchain using the VDI link and Perform GCC Compilation In Both Normal gcc-compiler and RISC-V Based gcc compiler and check the result. </summary>   
<br>
  
* VDI Link: https://forgefunder.com/~kunal/riscv_workshop.vdi and password for the machine is vsdiat

**1. Install Ubuntu 18.04 LTS(Bionic Beaver) on Oracle Virtual Machine Box as given in the file**

**2. Create a Basic C file then Compile it with normal GCC-Compiler and See the Output**
```
$ gvim sum.c
```

![image](https://github.com/user-attachments/assets/70d837e1-79ed-46c4-93d6-9c7aa5b2e15b)

```
$ gcc sum.c
$ ./a.out
```


**3. Now Compile the same file with RISC-V Gcc-Compiler**

```
$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c
```

![WhatsApp Image 2025-01-16 at 11 16 39_9fd4762e](https://github.com/user-attachments/assets/61470c79-3c60-4440-9a25-748c446cdade)


Verify that the file has been compiled using below command

```
$ ls -ltr sum.o
```

**4. Check the assembly level file and know the RISC-V command operations**

Obtain the assembly level code file by using below command.

```
$ riscv64-unknown-elf-objdump -d sum.o
```
![WhatsApp Image 2025-01-16 at 11 16 39_e0b7d930](https://github.com/user-attachments/assets/5f0621a8-f3c3-40d8-8cfc-99a27c47be28)



* Here the **-d** stands for disassemble
* Next run the below command
```
$ riscv64-unknown-elf-objdump -d sum.o | less
```

* Now check for main by pressing **/** .
* The main here refers to int main() of your c file and keep in mind that the main should be present inside the <>.

 
* Now see here mine file took total of **11** instructions to complete the execution of int main() part.
* So, how did I get to know it took 11 instructions simple 101b0-10184=2C. 2C divided by 4 you get 11. 101b0 are last numbers before the atexit part see the image.
* These is when I used ```$ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c``` command
* If Replace -O1 with -Ofast i.e ```$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum.o sum.c``` then you would see reduction in instruction cycle.
  

 * For mine it is same but for you it should change for that please change the **a** value from 10 to 100 or something big number.

</details>

<details>
<summary><b>Task 2:</b> Using SPIKE simulation tool to verify output of the code with respect to gcc-compiler output and understanding of assmebly code calculations on register using the SPIKE. </summary>
<br>

**1. Create a simple c file as in below image and compile it with gcc and see the output.**

![WhatsApp Image 2025-01-16 at 11 17 35_4c35aeb1](https://github.com/user-attachments/assets/70e35837-333e-4243-9568-606e02e2561e)


**2. Now verify that your code is giving same output even when you use RISC-V compiler as shown.**
* Note here that spike command is used in place of ./a.out to see the output and successfully we have obtained same output in both try.
```
$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o num.o num.c

![WhatsApp Image 2025-01-16 at 11 17 36_aa0e6069](https://github.com/user-attachments/assets/642a0089-9581-4640-954f-15bfcc00ce73)


$ spike pk num.o
```
**3. Now see the dumpfile for both -O1 and -Ofast compiler optimization flag.**

* Dump file of -O1

   ```$ riscv64-unknown-elf-objdump -d num.o | less```

![WhatsApp Image 2025-01-16 at 11 17 36_d1aea7c9](https://github.com/user-attachments/assets/97c20c73-f3ee-4dc1-ba20-ec41fc7ed9b4)


* Dump file of -Ofast


**4. Getting to know the assembly code instructions using the SPIKE tool.**

* Note here i am using -Ofast dumpfile to explain the instructions
* First run the below command in image after entering the spike tool. The last part of command is the register hexadecimal address which may vary for you.
  ```
  until pc 0 100b0
  ```

![WhatsApp Image 2025-01-16 at 11 17 37_cafb3715](https://github.com/user-attachments/assets/7c17b406-46c2-47b5-b634-dd63912bb7e3)

  
* What does the above command make is that it will load register operation upto that address
* Now first check the content of a0 by entering ```reg 0 a0``` you will get 0*0000.... which i have skiped here.
* If you press enter without typing it the first instruction lui a0,0x2b gets executed.
* Now what does lui mean ? Its nothing but **load upper immediate** basically a RISC-V register has 32 bits in which the first 7 are opcode and next from 7 to 11 is rd and next remaning bits are immediate to which the value 0x2b is inserted as you can see in the picture.
* Next instruction which is going to be executed according to dumpfile will be addi sp,sp,-48.
* which means 48 decimal value which will be 30 in hexa that much will be subtracted from the current stack pointer value. The changes of values are shown in below images.

 

* Now what does addi mean? well it means add immediate which will add the 48 decimal value to the destination register.
* Like this you can execute all the instruction using SPIKE tool and see what does each one of them actually do.

</details>
<details>
<summary><b>Task 3:</b> Understand the RISC-V Instruction set. </summary>   
<br>

**Below I have left two links to understand the the instruction type of RISC-V and their register length**

* [RISC-V card](https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/notebooks/RISCV/RISCV_CARD.pdf)
* [RISC-V instruction set summary](https://pages.hmc.edu/harris/ddca/ddcarv/DDCArv_AppB_Harris.pdf)

![WhatsApp Image 2025-01-17 at 21 02 43_0ba7c7e3](https://github.com/user-attachments/assets/39d14840-9383-4bde-b6b6-7304db3dcc93)


</details>

<details>
<summary><b>Task 4:</b> By making use of RISCV Core: Verilog Netlist and Testbench, perform an experiment of Functional Simulation and observe the waveforms</summary>  
<br>

>***NOTE:** Since the designing of RISCV Architecture and writing it's testbench is not the part of this Research Internship, so we will use the Verilog Code and Testbench of RISCV that has already been designed. The reference GitHub repository is : [iiitb_rv32i](https://github.com/vinayrayapati/rv32i/)*    
  
### Steps to perform functional simulation of RISCV  
1. Create a new directory with your name ```mkdir <your_name>```
2. Create two files by using ```touch``` command as ```maazm_rv32i.v``` and ```maazm_rv32i_tb.v```  
3. Copy the code from the reference github repo and paste it in your verilog and testbench files  
  
  
4. To run and simulate the verilog code, enter the following command:  
	```
	$ iverilog -o maazm_rv32i maazm_rv32i.v maazm_rv32i_tb.v
	$ ./maazm_rv32i
	```
5. To see the simulation waveform in GTKWave, enter the following command:
	```
	$ gtkwave maazm_rv32i.vcd
	```


6. The GTKWave will be opened and following window will be appeared  
  
![4](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/8cc283c6-87ed-485b-86d1-cdccb2de47d6)
 
#### As shown in the figure below, all the instructions in the given verilog file is hard-coded. Hard-coded means that instead of following the RISCV specifications bit pattern, the designer has hard-coded each instructions based on their own pattern. Hence the 32-bits instruction that we generated in Task-2 will not match with the given instruction.  
  
<img width="500" alt="Instructions" src="https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/24cc896a-7817-4941-be7f-95d44c35d4d8">
  
#### Following are the differences between standard RISCV ISA and the Instruction Set given in the reference repository:  
  
|  **Operation**  |  **Standard RISCV ISA**  |  **Hardcoded ISA**  |  
|  :----:  |  :----:  |  :----:  |  
|  ADD R6, R2, R1  |  32'h00110333  |  32'h02208300  |  
|  SUB R7, R1, R2  |  32'h402083b3  |  32'h02209380  |  
|  AND R8, R1, R3  |  32'h0030f433  |  32'h0230a400  |  
|  OR R9, R2, R5  |  32'h005164b3  |  32'h02513480  |  
|  XOR R10, R1, R4  |  32'h0040c533  |  32'h0240c500  |  
|  SLT R1, R2, R4  |  32'h0045a0b3  |  32'h02415580  |  
|  ADDI R12, R4, 5  |  32'h004120b3  |  32'h00520600  |  
|  BEQ R0, R0, 15  |  32'h00000f63  |  32'h00f00002  |  
|  SW R3, R1, 2  |  32'h0030a123  |  32'h00209181  |  
|  LW R13, R1, 2  |  32'h0020a683  |  32'h00208681  |  
|  SRL R16, R14, R2  |  32'h0030a123  |  32'h00271803  |
|  SLL R15, R1, R2  |  32'h002097b3  |  32'h00208783  |   
  

#### *Analysing the Output Waveform of various instructions that we have covered in TASK-2*  
**```Instruction 1: ADD R6, R2, R1```**  
  
![ADD](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/fff34786-9f52-488b-827d-9516ba655ed1)

**```Instruction 2: SUB R7, R1, R2```**  
  
![SUB](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/a4ce7d65-1e61-4a35-9e9f-9941de9d6e19)

**```Instruction 3: AND R8, R1, R3```**  

![AND](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/28706b39-2cfa-4b29-b0ac-6c1bbc1cbbe9)

**```Instruction 4: OR R9, R2, R5```**  

![OR](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/617b18d4-35f8-42e4-8294-b8259042f1d6)

**```Instruction 5: XOR R10, R1, R4```**  

![XOR](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/724f6c43-6f5c-4be2-899a-061b202cbf34)

**```Instruction 6: SLT R1, R2, R4```**  

![SLT](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/6d0a3063-9a8c-49e2-84ab-eb8f99875d0a)

**```Instruction 7: ADDI R12, R4, 5```**  

![ADDI](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/d8287e99-05d4-4140-b4bc-844da65fe1c8)

**```Instruction 8: BEQ R0, R0, 15```**  
  
![BEQ](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/583e69e5-88ef-4853-8a3b-a282bb8cc90f)
 
**```Instruction 9: BNE R0, R1, 20```**

![BNE](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/d09128b6-172a-4b3a-bfa6-2364142bb9f8)
  
**```Instruction 10: SLL R15, R1, R2```**  

![SLL](https://github.com/maazm007/vsdsquadron-mini-internship/assets/83294849/885a63bc-485e-4594-8d15-52f2ec8da800)

</details>  




