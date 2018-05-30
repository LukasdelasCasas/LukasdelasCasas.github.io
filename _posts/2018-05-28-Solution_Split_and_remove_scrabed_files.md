---
layout: post
title: Python Script to split issues and remove articles
image: /img/lukas_voit/Profil_Voit.JPG
---


Object of this session was to write a script that splits the collected newspaper issues.
The downloaded issues were xml files with validated and well formed markup, so the second step was to remove the tags with a python script.

Most parts of the code were already given, so basically the real question was to bring them together.


1. At first i used the split ```results = re.split('', data)``` and the remove code on one of my files and printed them to my console to see what they do. 
2. I noticed that i had to find a spot were i could split the issue into the articles. I used the ```<div3 type="article"``` expression to split the files. 
   This expression resulted in a minor problem which i will explain later.
3. In the next step i tried to use the given ```with open``` expression to access, read the input files and write to the output files.
4. Included a numeration system ```start = 0```, so that i have a optical clue without opening a data file and see a count of the       progress. With every splited article the number increases by one ```start += 1```
5. Now i tried to include the original file directory and the files collected in the directory.
6. I included a for loop to read and split each issue into several articles and a second loop to remove the xml tags.  
7. I made the mistake to run it directly on my files without testing it on a small sample of the files, but could prevent some damage,     because i copied the input files before. Still it was very difficult to the delete the mass of processed files, so i had to use commandline with the ```del *.xml``` to do it.
8. At the end my first script worked with the exception that my used expression to split it was not a good choice. 
   It removed the ```<div3 type="article"``` from every file. After that the regular expression to remove the tags wasn´t working on the  rest of the tag. 
   So in the first line of any article file was the rest of the used tag to split the article. 
9. I wrote a second script with the expression  ```n=.*sample="complete">``` to remove the rest of the tag from the articles. 
   In the end it would be more economic to only use one script with a regular expression that matches the complete ```<div3 ``` tag, but i still got the results.

First Script: 
```
# Use of Regular-expression Module and os Module to open and work with directories
import os
import re


# Description of Input and Output Path
original_path = os.path.abspath('Texte_Homework_Perseus')
output_path = os.path.abspath('Textsplit')

# List of Input and Output File Directories
org_FileNames = os.listdir('Texte_Homework_Perseus')
output_listoffiles = os.listdir('Textsplit')



# Loop to open and read every file in given Directory 
for fileName in org_FileNames:
    with open("Texte_Homework_Perseus\\" +fileName, 'r', encoding = "utf8") as file1:
        data = file1.read()

        # Split Issues into articles at every section, when this Expression occurs
        results = re.split('<div3 type="article"', data)

        # Nummeration of the aticles, starting with 0
        start = 0

        # Loop to save the splitted Issues in several files with given Name 
        for dataNew in results:
            newFileName = fileName + "_modified_" + str(start) + ".xml"

            # Increase every filename by one after a article is saved 
            start += 1

            # Remove XML from 
            text = re.sub("<[^<]+>", "", dataNew)

            # Save Files with removed tags
            with open("Textsplit\\" + newFileName, "w", encoding = "utf8") as file1:
                file1.write(text)

    # close used files again
    file1.close()
```

Second Script to remove the splited <div3- Tag:
```
import os
import re

original_path = os.path.abspath('Textsplit')
output_path = os.path.abspath ('Remove_Rest_Tags')

org_FileNames = os.listdir('Textsplit')
output_listofFiles = os.listdir('Remove_Rest_Tags')



for fileName in org_FileNames:
    with open("Textsplit\\" +fileName, 'r', encoding="utf8") as file1:
        data= file1.read()

        text = re.sub(' n=.*org="uniform" sample="complete">', "", data)

        with open("Remove_Rest_Tags\\" +fileName, "w", encoding="utf8") as file1:
            file1.write(text)


file1.close()
```            
