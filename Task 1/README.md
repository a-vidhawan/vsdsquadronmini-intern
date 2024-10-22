Task 1:

- First, we were asked to setup up a Ubuntu virtual machine on our computers, below is a screenshot of the VM running to spec.

![image](https://github.com/user-attachments/assets/bb6ab50a-c0d6-4d9b-85bc-722a8fcfda82)


- Next, we had to write a simple C program in this Vm, I used the example and wrote a program to calculate the sum of numbers from 1 to an arbitrary number, n.

- We then compile our C code using gcc, abd test to see whether the output is correct or not. As we can see the sum of numbers from 1 to 7 is 28.

- Then, we use the RISC V simulator(compiler) to compile our code, and run a function on it to see the Assembly code behind it.

- Lastly, we change the field from O1 to Ofast, and then again run the objdump function to view the main function's Assembly code. We observe that the number of instructions is lesser now.
