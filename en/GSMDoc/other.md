## Catalog
*  [Appendix 1 GSM Code Compilation Process](#appendix-1-gsm-code-compilation-process)
*  [Return](./menu.md)

# Appendix 1 GSM Code Compilation Process
I know very little about this part, as I don't have the source code or the rest of the shader code in the shader folder.  
But based on querying the hardcoded text of the executable file and reading the GSM file, the compilation flow should be as follows:  
* 1. Based on the md5 value comparison of the GSM files, determine whether the current shader to be used has been compiled, if not then continue to step 2, if yes then jump to step 6
* 2. Based on the code content, the necessary inputs, outputs and #defines are automatically recognized and packaged as shader_combinations.set
* 3. Converting GSM file to HLSL code, guess the file name is scheme.h (the name that appears in the error report)
* 4. The converted HLSL is merged with the code in the shader folder and compiled into the shader cache
* 5. Storing shader cache in the %LOCALAPPDATA%\Men of War II\shader_cache\ folder.
* 6. Game reads target cache and executes the shader.