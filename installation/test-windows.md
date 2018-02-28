# Execute FreeLing on Windows

FreeLing provides a main program that allows to execute most of its capabilities.
However, remember that FreeLing is a library and that much more functionalities are accessible writting your own main program.

The provided main program can be executed running the command:
```
   FREELINGDIR\freeling\bin\analyze.bat -f en.cfg < myfile.txt
```
Where the folder `FREELINGDIR` is:

* If you installed a Windows binary package, `FREELINGDIR` is the folder where you uncompressed it)
* If you installed on Windows compiling from source, `FREELINGDIR` is the folder you used as `CMAKE_INSTALL_PREFIX`.  If you used the default installation, it is `C:\Program Files`.

The script `analyze.bat` behaves just the Linux `analyze` script. Check FreeLing manual (https://talp-upc.gitbooks.io/freeling-user-manual/), section "Using the analyze script", to find out the parameters it accepts.

Note that the input text in `myfile.txt` must be UTF8 encoded. You can also provide input from the terminal and end it with ctl-Z. 
Take into account windows command prompt is not UTF8, so you will find problems if you use non-ascii characters.

See sections [Execute FreeLing demo](#execute-analyzer) and [Call FreeLing Library](#call-library) to find out more on how to use FreeLing.        


