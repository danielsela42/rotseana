AUTHOR: Daniel Sela

This is the description of using python version of findburst and coordinates finding tool, developed by me during summer research 2017. It is encouraged to use this tool for some useful and robust variable star search.
=============================================================================
WARNING: This is not a complete substitute of the analysis of Variable star search with ROTSE data. But I bet this will save tremendous amount of time finding the same stars over several nights and so on

COMMENTS/SUGGESTIONS: danielsela42@gmail.com

ACKNOWLEDGEMENT: Arnon Sela, Guvinda Dhungana

Enjoy the cool work!

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Objective:
At the beginning of the process of finding variable stars, just getting the data from one star can take an hour. After finding the coordinates of a star, one must search through all the nights to find the objid's of the same star. Then, findburst_gd must be run in IDL to extract the stars data from each night. This package attempts to simplify this process.  Some of these programs are similar to the their IDL counterparts, while others are added to shorten this process. Together, these programs can shorten the time it takes to gather the data of a star from an hour to a minute or less.

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Instruction to use this package:

1. Enter bash: bash

2. Activate the virtual environment: source ~/pyvarstar/.bashrc

3. You can now run a program that is in ~/pyvarstar/vstars from any location.

Quick Start: common process:

1. Use getcoords to create a single file with the coordinates and filtered observations over all the nights.

2. After finding the coordinates of one star though the result of find_burst, use matchcoords_gd to gather the data over all the nights. 
   The inputed file can be the output of getcoords.

3. Alternatively, if you do not wish to use getcoords, you can use findcoords_gd to gather the data over all the nights of a single star. 
   Though findcoords_gd will take longer because it has to filter observations and search through the FIT or MATCH file for the coordinates and object id's. 
   The output of getcoords can be used as the inputed file.

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Programs:
=========

find_burst
-----------

creates a PDF file instead of a postscript of all the stars within the designated file. The file types supported are .datc, .dat, and .fit.

Parameters:
    --mindelta: Minimum delta
    --minsig: Minimum sigma
    --minchisq: Minimum chisq
    --log: Saves the output under name.log. The .log must be .pdf.

To run:

    find_burst --mindelta VALUE --minsig VALUE --minchisq VALUE  --log name.log -f FILE

Example: 

    find_burst --mindelta 0.1 --minsig 1.0 --minchisq 2.0 --log sky1.pdf -f ../000906_sky0001_1c_match.datc
    
For more information run find_burst -h (or --help)

findburst_gd
------------

Extracts the date, magnitude, and magnitude error of a star given an object idea from a file and saves it as a text file.

Parameters:
    --match: Works on match structured files (.dat and .datc). Only use when the file you are retrieving the data from is a match structured file.
    --fits: Works on FIT files. Only use when the file you are retrieving the data from is a FIT structured file.
    --mindelta: Minimum delta
    --minsig: Minimum sigma
    --minchisq: Minimum chisq
    --objid: Processes a specific objid
    --log: Saves the output as a .txt.

To run using a match structured file:

    findburst_gd --match FILE --mindelta VALUE --minsig VALUE --minchisq VALUE  --log name.txt

To run using fit structured file:

    findburst_gd --fits FILE --mindelta VALUE --minsig VALUE --minchisq VALUE  --objid VALUE --log name.txt

Example:

    findburst_gd --match 000409_xtetrans_1a_match.dat --mindelta 0.1 --minsig 1.0 --minchisq 2.0  --objid 115 --log name.txt

getcoords
---------

Extracts the coordinates from one or more files and prints them to the terminal. You can concatenate the output into a text file. getcoords filters out bad observations. Negative RA coordinates are skipped as part of the filtering process. If you are going to use getcoords, it is recommended that you run it over in advance over multiple directories. It takes time for it to run on multiple files due to the filter observation that would be executed per object on each file. Furthermore, it is recommended that you collaborate with other users on the output, since only one output needs to be created per directory.

Parameters:

    -f or --file: Processes the specified file.

To run:

    getcoords -f FILE

Example:

    getcoords -f 000409_xtetrans_1a_match.dat

matchcoords
---------

Extract object ids of similar coordinates in a file within a specified error. matchcoords works on a text coordinate file that can be produced by getcoords. When typing the coordinates, there should be no spaces, and a capital "J" in the beginning. matchcoords process is similar to findcoords, however, since it is working on previously generated good coordinates file, it is much faster. Therefore, it is recommended to generate coordinate files per directory in advance, and search through the files using matchcoords.

Parameters:

    -e: error with a float value.

To run:

    matchoords -e ERRORVALUE -f FILE -c "COORDINATES"

Example:
    
    getcoords -f 000409*_match.dat > all_coords.txt
    matchoords -e 10 -f all_coords.txt -c "J110526.404+501802.085"
    
matchcoords_gd
---------

Extract the date, magnitude, and magnitude error of a star given the coordinates and an error and output it into a text file. matchcoords_gd works on a text coordinate file that can be produced by getcoords.

Parameters:
    -e: error with with a float value.
    --w-ref: adds the objid and the name of the file from which the data was extracted to the text file.
    -c or --coord: coordinates with a string "". Do not use any spaces and use a capital "J" at the beginning.
    -f or --file: the file(s) that the data will be extracted from.

To run without reference:

    matchcoords_gd -e ERRORVALUE --log NAME -c "COORDINATES" -f FILE

To run with reference:

    matchcoords_gd -e ERRORVALUE --w-ref --log NAME -c "COORDINATES" -f FILE

Example:

    matchcoords_gd -e 10 -—w-ref --log name_gd  -c “J111734.010+501526.228” -f ../000409_xtetrans_1a_match.dat ../*.fit

findcoords
---------

Extract object ids of similar coordinates in a file within a specified error.

Parameters:
    -e: error with with a float value.
    -c or --coord: coordinates with a string "". Do not use any spaces and use a capital "J" at the beginning.
    -f or --file: the file(s) that the data will be extracted from.

To run:

    findcoords -e ERRORVALUE -c "COORDINATES" -f FILE

Example:

    findcoords -e 10 -c "J110526.404+501802.085" -f 000409_xtetrans_1a_match.dat

findcoords_gd
---------

Extract the date, magnitude, and magnitude error of a star given the coordinates and an error. This program saves the output into a text file.

Parameters:
    -e: error with with a float value.
    --w-ref: adds the objid and the name of the file from which the data was extracted to the text file.
    -c or --coord: coordinates with a string "". Do not use any spaces and use a capital "J" at the beginning.
    -f or --file: the file(s) that the data will be extracted from.

To run without reference:

    findcoords_gd -e ERRORVALUE --log NAME -c "COORDINATES" -f FILE

To run with reference:

    findcoords_gd -e ERRORVALUE --w-ref --log NAME -c "COORDINATES" -f FILE

Example:

    findcoords_gd -e 10  -—w-ref --log name_gd  -c “J111734.010+501526.228” -f ../000409_xtetrans_1a_match.dat ../*.fit

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Package organization:

1. The directory to locate the files is ~/pyvarstar

2. Within versions there are the two latest versions with the same sub-directories.

pyvarstar/
|-- .bashrc
|-- README
|-- vstars -> versions/vstars_20170804/ : link to active version
|-- versions/ : keep versions for qiuck fallback
|   |-- vstars_20170804/ 
|   |   |-- lib/
|   |   |   |-- vstars/ : python programs
|   |   |-- bin/ : pyhton executables
|   |   |   |-- getcoords
|   |   |   |-- find_burst
|   |   |   |-- findcoords_gd
|   |   |   |-- __init__.py
|   |   |   |-- findburst_gd
|   |   |   |-- findcoords
|   |   |   |-- matchcoords
|   |-- vstars_20170803/
|-- venv/ : python virtual environments
|   |-- vstars34/ python vstars virtual environment
|   |   |-- bin/
|   |   |   |-- activate
|   |   |   |-- activate.csh