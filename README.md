# Embedded Bootloader for STM32F767ZI

## what is a Bootloader?

A bootloader is a special program which allows user application program to be updated. The new user application can be obtained using alternative download channels, 
such as a USB stick or a network port. The bootloader and the user application should be written and built as two separate projects resulting in two separate executables. The main tasks of the bootloader are to reprogram/replace the user application, if necessary, and to jump to the user application to execute it. The user application doesn’t necessarily need to know the existence of the bootloader. The bootloader is usually placed at the chips flash base address, so that it will be executed by the CPU after reset. 

## STM32F767ZI Memory Organization
* Internal Flash memory also called as Embedded Flash memory of 2MB
* Internal SRAM1 of 368KB
* Internal SRAM2 of 16KB
* System Memory (ROM) of 60KB
* OTP memory of 1024 bytes
* Option bytes memory of 32 bytes
* Backup RAM of 4KB

## Reset Sequence and Memory Aliasing of the MCU

1. When you reset the MCU, the PC of the processor is loaded with the value 0x0000_0000;
2. Then processor reads the value @ memory location 0x0000_0000 into MSP (Main stack pointer register). That means, processor first initializes the Stack pointer register;
3. After that, processor reads the value @ memory location 0x000_0004 in to PC;
4. PC jumps to the reset handler;
5. A reset handler is just a C or Assembly function written by you to carry out any initializations required;
6. From reset handler you call your main function of the application.

## *All ARM Cortex M Based MCUs right after reset does,*
* Load value @ Memory addr. 0x0000_0000 in to MSP.
* Load value @ Memory addr. 0x0000_0004 in to PC (Value = Addr of the reset handler)

## *In STM32 Microcontroller,*
* MSP value stored at 0x0800_0000.
* Vector table starts from 0x800_0004.
* Address of the reset handler found at 0x800_0004.

Don't you think we should somehow link 0x0800_0000 to 0x0000_0000? The answer for this is that both addresses can be linked with the technique called "memory aliasing" and it depends on the MCU. For example, in this case by default the base address of the user flash is mapped onto the base address of the memory map, that is 0x0000_0000. By default, the user flash is aliased to the very first address of the memory map that is 0. 


