# Convert Newlines From Windows to Mac/Unix

 If the text has only CR:

    tr '\r' '\n' < inputfile > outputfile

If it has CR+LF,

    tr -d '\r' < inputfile > outputfile
