Introduction to Dynamic-Link Libraries (.DLLs)

When I was quite young, knew quite few about how software work, I often got annoyed by an corruption error like "Cannot find file xxxx.dll" when I tried to open a Windows PC game by clicking an execution file (.exe). Therefore, I feel quite familiar whenever I saw a DLL file, although I did not try to know what this kind of file for. I've just done an Operating System unit in my previous uni semester, and learnt some brief overview of DLL when I was doing some research on the book by William Stallings. So I wrote this article as a study note about Dynamic-Link Libraries.



## Executing a Program

When we are executing a program file stored in the storage disk, it is the process running in the main memory rather than the program itself. When a program is given the execution command, it will (or actually Operating System is the one to do this job) create a process image in the main memory. Generally, a process image contains a Process Control Block (PCB), which is like the identifier of a process. It also contains the program code itself and some data, stack for executing use. Unless the process is killed, the PCB is always stored in the main memory to play an profile role for its process image, while other part like program code might be switched out from main memory back to hard disk temperately if Operating System decide to block this process for a while.

The point here is that program code is some static data stored in the program file (which is stored in the storage disk), and the code need to be copied into the process image (which is stored in the main memory) when executing a program. This process is called **Loading**.



## Loading

How **loading** is conducted in OS? According to *William Stallings*, 3 approaches can be taken in general.

1. Absolute Loading
2. Relocatable Loading
3. Dynamic Run-time Loading

For the first one, absolute loading. As the name indicates, it uses some pre-defined address value to tell program code where it will be addressed in the main memory. Sounds quite clumsy since the address for the program code is often changing due to the switch out process I mentioned in previous section. We have 2 ways to load code in this way. One is let developers decide the address, which means as developers, we might need to say "Load the data from 0x2333 to AC", and this way could probably be possible if we are clearly sure about what happen in each semiconductor in the main memory. The other way to do Absolute Loading is using symbolic addresses which is kind of like using variable name in programming language. Using previous example, we could define a symbol (let's say X here) for the data we want to use later, then we could refer that symbol like "Load the data from X to AC". Both ways could be tried in MARIE, which is a extremely simplified version of Assembly code, and a JavaScript simulator for MARIE could be found at https://marie.js.org/

Relocatable Loading and Dynamic Run-time Loading are quite similar. They all use relative addresses rather than absolute one, which means we could regard the start address of the code as 0, and refer the later address based how far that part is from the start address 0. The difference between these 2 loading ways is that the Program Loader will interpret all of those relative addresses into actual physical address when loading the code in Relocatable Loading. However, if the process is switched out by OS, and switched back later, the start address of the program might be changed, which causes all relative addresses should have new physical addresses. Therefore in Dynamic Run-time Loading, the address is calculated to the actual physical address only until that specific instruction is about to execute. In a other word, it is like whether interpret a whole program code at once or interpret each line one by one.



## Linking

The **Loading** process above will push the **Load Module** (and maybe some dynamic libraries) to the Main memory. Load Module is an integrated set of program and data modules that Loader could use. In each object module, there might be some symbolic variable that references data in other modules, which means all these invoked modules need to be contained in the Load Module for successful execution. Before handover the Load Module to the Loader, there is a process called **Linking**, where the Linker will combine all required object modules as well as static libraries together to produce the Load Module for the Loader.

Things getting interesting here. Not all of the linking must be done before generating the Load Module. Some of modules could be loaded into the main main memory when needed in the execution, which is  **Dynamic Linking**. Why do operating system wants to do things in this way? Because some non-developers-customized modules might be invoked by multiple processes. If every process link those modules to its Load Module, there are could be multiple same modules appeared in the main memory, which is obviously wasting resource. Therefore, such shareable module could be declared as dynamic, which, in Windows environment specifically, are called **Dynamic-Link Libraries (DLLs)**. When a process make an external reference to such a dynamic module, the OS will check if the target module is already loaded into the main memory. In the case that reference to an absent module, the OS will then locate that module, load it to the main memory, and then dynamically link that module to the calling module.



## DLL hell

The use of DLLs can lead to a problem which could be called DLL hell. DLL hell means multiple processes invoke the same DLL module while they are expecting different versions of it. The application might be reinstalled and bring in with it an older version of a DLL file.



## Possible Practice in other Area

This section is just some of my personal thoughts. Nowadays, as developers, we have quite a lot package managers such as NPM, Yarn, etc. Their mechanism are kind of like DLL's working method. For general programmers, they might have lots of projects installed on their computers. In my case, when I learnt lots of JavaScript frameworks I always need first to git clone the project and then using "`npm install`" or other CLI command to install all needed dependencies into my "node_modules" folder, for some big project, the dependencies could be really large size, if the package manager could somehow make use of the dependencies already installed in the computer, that could reduce the size of the project quite a lot. I use NPM quite a lot, and I know NPM does have a choice to make a global install, which is usually used for installing some debug tool rather than actual external packages needed by code execution. Even if we install every packages globally, just like DLL hell, we cannot reduce project size a lot because of different versions. Therefore, it might come to how should NPM record the version number in the "`package.json`" (which record all necessary packages and their version number). Does it have to be a specific version? could it offer a compatible range? If some versions of a package is back-compatible, could some latest version installed globally be applied to that project directly instead of installing an older version of that package?