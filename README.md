# Forensic tools

A project to contribute to the sector of ITEP-RN.

This repository contains programs to explore memory from a HDD with a specific HikVision FileSystem. Therefore, if you do not want apply to this model, it can support to understand a workflow.

It has some program that may be run in certain sequence or if you do not need to use in a sequence, you can skip some runs. Below it has a brief describe what each one does.

### 1. Extract_first_frame (.sh)

A script that will wander through a directory and extract the first frame of all video (with format .dd) that was inside.

To run: ./extract_first_frame.sh <source_directory> <destine_directory>

### 2. Extract_address (.sh)

This script will wander through a binary data, in specific a stream of video, that it is irregular with some pattern, searching a string passed by argument and copy that slice to new file.

To run: ./extract_address.sh <source_directory> <string_to_search> <destine_directory>

### 3. Separate_sign (.cpp)

With a slice that contains all the signatures (or strings), it will separate the content between them to do some analysis. It must be compiled and make a binary to be runned.

To compile: g++ separate_sign.cpp -o separate_sign.o

To run: ./separate_sign.o <inputfile_path> <outputfile_path>

**Ps:inputfile_path must be copy of IDR Table of a Hikvision Filesystem.**


### 4. Check_time (.cpp)

The first analysis is check some pattern of timestamps in phrases marked by a signature. It will verify a pack of 8 characters is a valid timestamp compared with the date visualized in the frames extracted. It must be compiled to run.

To compile: g++ check_time.cpp -o check_time.o

To run: ./check_time.o <inputfile_path> <date_to_compare>

**Ps1: The inputfile must be the result of separate_sign, that is a csv file.**
**Ps2: The date to compare must be obey this format: day month year weekday hour minute seconds, i.e., 18 03 2020 qua 18 28 46**

The table to convert a weekday in English to Portuguese:

|English|Portuguese|
|-------|----------|
|--Sun--|----Dom---|
|--Mon--|----Seg---|
|--Tue--|----Ter---|
|--Wed--|----Qua---|
|--Thu--|----Qui---|
|--Fri--|----Sex---|
|--Sat--|----Sab---|

### 5. First_time (.cpp)

The aim of this program is catch the first valid OFNI8 that has a timestamp in the phrase and it will be used in the next step. The result is a .csv file that contains the date.

To compile: g++ first_time.cpp -o first_time.o

To run: ./first_time.o <inputfile_path> 

**Ps: The recomendation is used the apply_first_time.sh**

### 6. Apply_first_time (.sh)

It will apply the program above in whole directory.

To run: ./apply_first_time.sh <directory_files>

### 7. Fetch_channel (.cpp)

To finalize all the steps, this program will do a search through a HIKBTREE to match the dates in dates.csv and fetch the correspondig channels, creating a new file with the following struct: dates in hex, dates readable, channels in hex, channels readable.


All the programs was used to test and confirm suspects of information presents in the data of HDD of Hikvision FileSystem. The entire project was possible thanks a article Analysis of the HIKVISION DVR file system of Han, Jeong and Lee and DOI: 10.1007/978-3-319-25512-5_13.

Look this is a sequence of steps.

In future, I will discuss the uses of each program in more detail, as some are designed to ensure some suspicious of what information is contained in each file. So, to guide the readers (and even me sometimes :D) I will review all codes and create a better flow. Any question, fell free to ask me (even you Julio).