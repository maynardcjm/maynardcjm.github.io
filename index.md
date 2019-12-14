<!DOCTYPE html>
<html>
<head>
	<title>Tracking Trackers</title>
</head>
	
<body>
	
<h1>Information Is Power</h1>
<h3>Website Description</h3>
<p>This website describes the work and results of my senior research on web tracking.</p>

<h3>About the Process</h3>
<p>Prior to embarking on this research, I never knew the extent of web tracking of user data and movement across the world wide web. Looking into this topic was an eye opener about some of the methods which provide insight into identifying or being able to target web visitors with tailored advertisements. </p>

<h3>Introduction</h3>
<p>This is an on going research of learning the ways of current internet technologies to track users in attempt to gather data on them.</p>



<div>
<h3>Analyzing Cookie Timestamp Python Script</h3>
<p>
# -*- coding: utf-8 -*-
"""
Senior Research COokie Data Processsing Python Script
Collin Maynard 
12/13/2019

"""
# Python Modules for timestamp datastructures
from time import time, ctime
from datetime import datetime


################    FUNCTIONS  SECTION     ################################
"""
Function Name: getRealTime
Function Params: row is the instance of the cookie table
Function Description: 1) Splices information out of the table
                      2) Inserts CRITICALLY important decimal place
                      3) converts information into UTC format 
                      4) stores in list
                      5) Repeats this for two more timestamps
                      6) returns list
"""
def getRealTime(row):
   
    timestamps = [] # creates list to hold three different timestamps
    counter = 0
    
    # set decimal position for timestamp1
    timestamp = row[0:10].strip() + "." + row[10:17].strip() # splicing data
    timestamp = datetime.fromtimestamp(float(timestamp)) # formats to UTC
    timestamps.insert(counter,timestamp) # adds value to list
    
    counter +=1 # incrementer for list insertion
    
    # set decimal position for timestamp2
    timestamp = row[17:27].strip() + "." + row[27:33].strip()# slices data
    # convert to UTC
    timestamp = datetime.fromtimestamp(float(timestamp)) # formats to UTC
    timestamps.insert(counter,timestamp) # adds value to list
    
    counter +=1
    
    # No decimal point needed for the 10-digit timestamps
    timestamp =  row[34:44].strip()# slices data
    timestamp = datetime.fromtimestamp(float(timestamp)) # formats to UTC
    timestamps.insert(counter,timestamp) # adds value to list
    
    return timestamps
################    end of getRealTime func  ################################


"""
Function Name: check_persistence
Function Params: An iterator named counter to keep place, and the cookie dict
Function Description: Calculates and returns difference of creation and expiration time
"""
def check_persistence(cookie, counter):
    return cookie['ID'+str(counter)][2]-cookie['ID'+str(counter)][1]
    
################    end of check_persistence func ############################


"""
Function Name: writeFile
Function Params: cookie dictionary 
Function Description: Creates new table of processed data and writes two files
File Output: 
             File 1: contains timestamp information for last accessed, creation time,                      
                           and expiration time
             File 2: contains the life span of the cookie
"""
def writeFile(cookie):
    
    counter = 1 # iterator for table instances
    newfile = open("mySR_resutls.txt", "w") # creates file for timestamp conversions
    newfile.write("lastAccessed\t\t\t\t creationTime\t\t\t\t exipires\n") # table header

#### Loop 1 ####    
    for row in cookie:
        if counter > 273:
            break
        newfile.write(str(cookie['ID'+str(counter)][0])+"\t") # writes last accessed
        newfile.write(str(cookie['ID'+str(counter)][1])+"\t") # writes creation time
        newfile.write(str(cookie['ID'+str(counter)][2])+"\n") # writes expiration
        
        counter +=1 #
        
###### end of Loop 1 #####
        
    newfile.close() # closes file 1
    counter = 1
    persFile = open("my_SR_persist_cookies.txt","w")
    persFile.write("ID\t\t Persistent\n")
    for row in cookie:
        if counter > 273:
            break
        persFile.write(str(counter) + "\t\t" + str(cookie['persistence'+str(counter)]) + "\n")
        counter +=1
    persFile.close()


"""----------------------------------------------------------------------

def sortPersistenceLIFE(cookie):
    
    itr = 2
    pos = 1
    greatest = cookie['persistence'+str(1)]
    cookie['sorted'+str(1)] = greatest
    while pos < 272:
        for row in cookie:
            if itr > 273:
                break
            if cookie['persistence'+str(itr)] > greatest:
                cookie['sorted'+str(pos)] = greatest
            itr +=1
        pos+=1
    return cookie
"""

#!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
"""
*******************************************************************************
------------------------ MAIN BODY --------------------------------------------
*******************************************************************************
"""
#!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!


#################   FILE LOCATION      ####################################
ALL_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\All_cookies_timestamp.txt"


ORG_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\ORG_DATAset_.txt"

GOV_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\GOV_DataSET_.txt"

EDU_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\EDU_DATAset_.txt" 

NET_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\NET_DATAset_.txt"

COM_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\COM_DATAset_.txt"


#########################   Open up file  ###################################
with open(ALL_file) as infile :
    table = infile.readlines()

#print(table[0]) #prints headers for visual

table = table[1:] # removes headers


itr = int(1) # iterator shifts dictionary key value to allow new instances loop 

cookie_lifecyle={} # dictionary to hold cookies' timestamp data


# Loops through every instance in the table
for row in table:
    
    # populate dictionary with each row instance 
    cookie_lifecyle['ID'+str(itr)] = getRealTime(row)
    cookie_lifecyle['persistence'+str(itr)] = check_persistence(cookie_lifecyle,itr)
    itr = int(itr)+1 # increase iterator

print("Your Cookie-data Files are Ready!!!")
# output test 
#print(cookie_lifecyle['ID1'][0])
#print(cookie_lifecyle['persistence1'])   

 
#sortPersistenceLIFE(cookie_lifecyle)
writeFile(cookie_lifecyle) # method call to write files!
########################################################################
"""
########################     END OF CODE     ###########################
"""
########################################################################


</p>

</div>


<div>

<h3>Python Script for Getting Number of Cookies for Each File</h3>
ORG_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\ORG_DATAset_.txt"

GOV_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\GOV_DataSET_.txt"

EDU_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\EDU_DATAset_.txt" 

NET_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\NET_DATAset_.txt"

COM_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\textData_files_forPYTHON\\COM_DATAset_.txt"

filelist=[ORG_file, GOV_file, EDU_file, NET_file, COM_file]

itr = 0
for f in filelist:
    with open(filelist[itr]) as infile :
        table = infile.readlines()
    
    if itr == 0:
        print("File Sizes")    
    table = table[1:]
    print(len(table))
    itr+=1
    

</div>

<!--Page Break-->
<br/><br/>
<br/><br/>

<div>
	<h1>DATA FILE MANIPULATION CODE </h1>

<p># -*- coding: utf-8 -*-
"""
Created on Sat Dec 14 13:50:23 2019
@author: maynard Collin

This code helps with accessing each indivdual data file for better analysis of data and for performing data manipulations

"""

# file 1 of each data collection ---NOT USALABLE UNTIL MANIPULATION
########    RAWWWWWWWWWWWWWWWWWWWWWWWWWWWW FILE_LOCATION's #################
ORG_file = "C:\\Users\\mayna\Desktop\\Senior Research Snippets\\SR_Data Collection\\Data_as_text_files\\_org__.txt"

GOV_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\SR_Data Collection\\Data_as_text_files\\_gov_.txt"

EDU_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\SR_Data Collection\\Data_as_text_files\\_edu_.txt" 

NET_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\SR_Data Collection\\Data_as_text_files\\_net_.txt"

COM_file = "C:\\Users\\mayna\\Desktop\\Senior Research Snippets\\SR_Data Collection\\Data_as_text_files\\_com_.txt"


# dictionary for each file group location values
#FILE_dictionary={'gov':GOV_file,'org':ORG_file,'net':NET_file,'edu':EDU_file,'com':COM_file}

FILE_dictionary = {}
empy_file_list = [] # holds each group of files for dictionary
full_file_list = [ORG_file, GOV_file, EDU_file, NET_file, COM_file]

key_list = ['gov', 'org', 'net', 'edu', 'com'] # holds dictionary key values



####################################################
# key for adding in differnt numbered files
#print(FILE_dictionary['gov'])
#manipulation = FILE_dictionary['gov'][:-4] + str(itr) + ".txt"
#print(manipulation)
####################################################



"""-----------------------------------------------
LOOP FOR POPULATING DICTIONARY WITH FILE NAMES
loop description:
    
    There are a total of 5 files for each group the filename are unusable without the correct number. Each file are named the same but have a number an example _gov_1.txt for each group these files range from _gov_1.txt through _gov_5
    
    Outer loop loops through each key group org, gov, edu, net, com
    Inner loop loops through each file name and adds it to each group
    
    Example first iteration is 'gov'
    
-----------------------------------------------"""

"""COUNTERS"""
outer_itr = 0 # outter loop iterator
inner_itr = 1 # inner loop iterator
file_iter = 0 
# Code the rest in
for repetition in range(0,5):
    for repetition in range(0,5):
        
        new_key = (str(key_list[outer_itr])) + str(inner_itr) # creates new key for each instance
        #old_key = (str(key_list[outer_itr])) outdated methodology for loop
        
        manipulation = full_file_list[outer_itr][:-4] + str(inner_itr) + ".txt"
        #FILE_dictionary[new_key] = FILE_dictionary[old_key][:-4] + str(inner_itr) + ".txt" # outdated methodology for loop
        
        
        FILE_dictionary[new_key] = str(manipulation) # add file name to dict

        """ITERATOR INCREMENTER"""
        inner_itr +=1
        
    inner_itr = 1 # reset inner loop iterator at the increment of outer loop
    outer_itr+=1


##### FOR TESTING LOOP ##############
#print(FILE_dictionary) # print out all modified file locations
#print(len(FILE_dictionary)) # test the correct number of files
###################################

# open file 
#with open () :

</p>

</div>



</body>

</html>
