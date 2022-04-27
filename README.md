# ZombieLoad PoC

This repository contains two applications to demonstrate **ZombieLoad as an example of Microarchitectural Data Sampling (MDS)**. For technical information about the bug, refer to the paper:

* [ZombieLoad: Cross-Privilege-Boundary Data Sampling](https://zombieload.com/zombieload.pdf) by Schwarz, Lipp, Moghimi, Van Bulck, Stecklina, Prescher, and Gruss

## Proof of Concept (PoC)

This repository contains a proof-of-concept attack showing ZombieLoad on **Windows 10**. It also includes a victim application to test the leakage in various scenarios. 

This demo was tested with an Intel Core i7-7700k but it should work on any Windows 10 system with any modern Intel Core or Xeon CPU since 2010. 

For the best results, use a fast CPU that supports Intel TSX (e.g. nearly any Intel Core i7-5xxx, i7-6xxx, or i7-7xxx). 

## Building

The PoCs only require MinGW-w64 to compile. 

Building the attacker or victim is as simple as running `make` in the folder of the application. 

## Run

### Attacker Application

This Variant does not require any CPU features or privileges. 
Run the attacker on the first hyperthread (mask: 0b1): `start /affinity 1 .\leak.exe`. It sometimes takes a while until the leakage starts. Starting a different program which uses memory (e.g., an internet browser) sometimes reduces the waiting time. 

## Victim Application

Simply run the victim on the same physical core but a different hyperthread (mask: 0b10000) as the attacker: `start /affinity 16 .\secret.exe`. You can also provide a secret letter to the victim application as a parameter, e.g., `start /affinity 16 .\secret.exe M` to access memory containing 'M's. The default secret letter is 'X'. 

As soon as the victim is started, there should be a clear signal in the attacker process, i.e., the bar for the leaked letter should get longer. 

## Help

* **How do I know which core IDs are hyperthreads?**
 
    You can use the [coreinfo tool from Windows Sysinternals](https://docs.microsoft.com/en-us/sysinternals/downloads/coreinfo). 

* **Can I run the PoC in a virtual machine?**

    Yes, the PoC also works on virtual machines. However, due to the additional layer introduced by a virtual machine, it might not work as good as on native hardware. 

* **It just does not work on my computer, what can I do?**

    There can be a lot of different reasons for that. You can try:
    
    * Ensure that your CPU frequency is at the maximum, and frequency scaling is disabled.
    * If you run it on a mobile device (e.g., a laptop), ensure that it is plugged in to get the best performance.
    * Try to pin the tools to a specific CPU core (e.g. with taskset). Also try different cores and core combinations. Leaking values only works if attacker and victim run on the same physical core. 
    * Vary the load on your computer. On some machines it works better if the load is higher, on others it works better if the load is lower.
    * Try to restart the demos and also your computer. Especially after a standby, the timings are broken on some computers. 
