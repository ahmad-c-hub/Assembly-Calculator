.data
prompt: .asciiz "\nSelect operation:\n 1) Add\n 2) Subtract\n 3) Multiply\n 4) Divide\n 5) Store Result\n 6) Recall Memory\n 7) Clear Memory\n 8) View History\n 9) Exit\nEnter choice: "
inputPrompt: .asciiz "\nEnter number: "
divByZeroMsg: .asciiz "\nError: Division by zero."
resultStoredMsg: .asciiz "\nResult stored in memory."
memoryClearedMsg: .asciiz "\nMemory cleared."
historyMsg: .asciiz "\nViewing history:\n"
memoryValue: .word 0   # Memory for storing a calculation result
history: .space 40     # Allocate space for 10 results (10 words * 4 bytes each)
historyIndex: .word 0  # Index to keep track of the next free slot in history

.text
.globl main


main:
    
menu_loop:
    li $v0, 4       
    la $a0, prompt  
    syscall

    
    li $v0, 5       
    syscall
    move $t0, $v0   

    
    beqz $t0, menu_loop  
    

    jal menu_selected

menu_selected:

    li $t1, 1       
    beq $t0, $t1, add
    li $t1, 2       
    beq $t0, $t1, subtract
    li $t1, 3       
    beq $t0, $t1, multiply
    li $t1, 4       
    beq $t0, $t1, divide
    li $t1, 5       
    beq $t0, $t1, store_result
    li $t1, 6       
    beq $t0, $t1, recall_memory
    li $t1, 7      
    beq $t0, $t1, clear_memory
    li $t1, 8       
    beq $t0, $t1, view_history
    li $t1, 9      
    beq $t0, $t1, exit_program
    j menu_loop     



.text


add:
    li $v0, 4
    la $a0, inputPrompt
    syscall

    li $v0, 5      
    syscall
    move $t2, $v0   

    li $v0, 4
    la $a0, inputPrompt
    syscall

    li $v0, 5       
    syscall

    add $t3, $t2, $v0  

    li $v0, 1       
    move $a0, $t3
    syscall
    
    jal update_history

    j menu_loop    


subtract:
   
    li $v0, 4
    la $a0, inputPrompt
    syscall


    li $v0, 5       
    syscall
    move $t2, $v0  

  
    li $v0, 4
    la $a0, inputPrompt
    syscall


    li $v0, 5       
    syscall


    sub $t3, $t2, $v0  


    li $v0, 1       
    move $a0, $t3
    syscall
    
    jal update_history
    
    j menu_loop     

multiply:

    li $v0, 4
    la $a0, inputPrompt
    syscall


    li $v0, 5       
    syscall
    move $t2, $v0   


    li $v0, 4
    la $a0, inputPrompt
    syscall


    li $v0, 5       
    syscall


    mul $t3, $t2, $v0  


    li $v0, 1       
    move $a0, $t3
    syscall

    jal update_history
        
    
    j menu_loop     


divide:

    li $v0, 4
    la $a0, inputPrompt
    syscall

    
    li $v0, 5       
    syscall
    move $t2, $v0   

 
    li $v0, 4
    la $a0, inputPrompt
    syscall


    li $v0, 5       
    syscall
    beqz $v0, div_by_zero  
    move $t4, $v0   


    div $t2, $t4    
    mflo $t3        


    li $v0, 1       
    move $a0, $t3
    syscall

    jal update_history
    
    j menu_loop     

div_by_zero:
    li $v0, 4
    la $a0, divByZeroMsg
    syscall
    j menu_loop     

store_result:
    sw $t3, memoryValue  
    li $v0, 4
    la $a0, resultStoredMsg  
    syscall
    j menu_loop

recall_memory:
    li $v0, 4
    la $a0, inputPrompt  
    syscall

    lw $t3, memoryValue  
    li $v0, 1            
    move $a0, $t3
    syscall

    j menu_loop

clear_memory:
    li $t3, 0            
    sw $t3, memoryValue  
    li $v0, 4
    la $a0, memoryClearedMsg 
    syscall
    j menu_loop
    

update_history:
    lw $t0, historyIndex       
    sll $t0, $t0, 2            
    la $t1, history            
    add $t0, $t0, $t1          
    sw $t3, 0($t0)             

    lw $t1, historyIndex      
    addi $t1, $t1, 1           
    sw $t1, historyIndex       
    jr $ra                     

view_history:
    li $t0, 0  

view_loop:
    lw $t1, historyIndex  
    blt $t0, $t1, print_history  
    j menu_loop            

print_history:
    sll $t2, $t0, 2           
    la $t3, history       
    add $t3, $t3, $t2         
    lw $a0, 0($t3)             
    li $v0, 1                  
    syscall

    addi $t0, $t0, 1           
    lw $t1, historyIndex       
    bge $t0, $t1, menu_loop    
    li $v0, 4                
    la $a0, space_str          
    syscall
    j view_loop                

.data
space_str: .asciiz " "         

exit_program:
    li $v0, 10      
    syscall
