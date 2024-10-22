# Task 1:

- First, we were asked to setup up a Ubuntu virtual machine on our computers, below is a screenshot of the VM running to spec.

![image](https://github.com/user-attachments/assets/bb6ab50a-c0d6-4d9b-85bc-722a8fcfda82)

- Next, we had to write a simple C program in this VM, I used the example and wrote a program to calculate the sum of numbers from 1 to an arbitrary number, n.

![image](https://github.com/user-attachments/assets/5e2fad4f-804e-426c-8877-8ddd6d6de33a)

- We then compile our C code using gcc, and test to see whether the output is correct or not. As we can see the sum of numbers from 1 to 7 is 28.

  ![image](https://github.com/user-attachments/assets/f931bfc3-fbd2-40ed-8584-78a9b29ebebf)
  ![image](https://github.com/user-attachments/assets/69431583-123a-45da-816f-9e83c5804b5a)

- Then, we use the RISC V simulator(compiler) to compile our code, and run a function on it to see the Assembly code behind it.

![image](https://github.com/user-attachments/assets/2c786f56-4fac-4433-9845-5197848d473f)
![image](https://github.com/user-attachments/assets/37b094f7-02a3-4bbf-8027-f0a0daabe21f)

- Lastly, we change the field from O1 to Ofast, and then again run the objdump function to view the main function's Assembly code. We observe that the number of instructions is lesser now.

![image](https://github.com/user-attachments/assets/f3f7e906-5b2c-4722-b7fd-6a0150c9059b)
![image](https://github.com/user-attachments/assets/86f3b802-cab4-416b-9fd0-807358223663)
