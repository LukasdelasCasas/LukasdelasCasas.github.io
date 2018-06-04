---
layout: post
title: Python Script to structure collected data
image: /img/lukas_voit/Profil_Voit.JPG
---


Object of this session was to write a script that structures the collected data in a tsv/csv file.


Most parts of the code were already given, so basically the real question was edit the code correctly, so it can fullfill the task.

1. At first i added my absolute path directions and included a list. The list should later collect the structured data results and store them in it.
2. I added the direction of the downloaded input files. 

    ```
    import re, os

    source = os.path.abspath('Perseus_Test_Texte')
    target = os.path.abspath('Output_ordner')

    lof = os.listdir('Perseus_Test_Texte')
    counter = 0 # general counter to keep track of the progress

    # list to include data in variable
    list = []
    ```

3. I stored the variable of the processed data files in the list. I also changed the variable from "\n" = new line to "\t" = tab, so that the entries of one splitted issue parts are stored in separat lines. 
A issue was that i at first forgot to tell python to store each item on a different line with ``` +"/n"```. 
So several times python just stored the data on one line.

    ```
    var = "\t".join([itemID,dateVar,unitType,header,text]) + "\n"
    ```

4. I added a for loop to write the collected and sorted data in a .tsv file. I also chaneged the code in the open function, so that it gets stored in a .tsv and not in a .txt file.
Problem was that a for loop is necessary to write the in a list collected data into a file as write function does not accept lists but only 
strings. I tried to work around that by using ```str(list)``` as an input argument in the write function, but as a result i only got the total data in one line. Erica than gave me the hint to use a loop 
to solve this problem. So i finally got the the collected stuctured entries and metadata of the issues on separat lines. 

    ```
    for item in list:
        f9.write(item)
    ```

5. At last i out commanded the print Header line, because it was slowing down my running script.

    ```
    # print("\nNo header found!\n")
    ```

6. The full script:

```
import re, os

source = os.path.abspath('Perseus_Test_Texte')
target = os.path.abspath('Output_ordner')

lof = os.listdir('Perseus_Test_Texte')
counter = 0 # general counter to keep track of the progress

# list to include data in variable
list = []

for f in lof:
    if f.startswith("dltext"): # fileName test        
        with open(source + '/' + f, "r", encoding="utf8") as f1:
            text = f1.read()

            # try to find the date
            date = re.search(r'<date value="([\d-]+)"', text).group(1)

            # splitting the issue into articles/items
            split = re.split("<div3 ", text)

            c = 0 # item counter
            for s in split[1:]:
                c += 1
                s = "<div3 " + s # a step to restore the integrity of items
                #input(s)

                # try to find a unitType
                try:
                    unitType = re.search(r'type="([^\"]+)"', s).group(1)
                except:
                    unitType = "noType"
                    print(s)

                # try to find a header
                try:
                    header = re.search(r'<head>(.*)</head>', s).group(1)
                    header = re.sub("<[^<]+>", "", header)
                except:
                    header = "NO HEADER"
                   # print("\nNo header found!\n")

                text = re.sub("<[^<]+>", "", s)
                text = re.sub(" +\n|\n +", "\n", text)
                text = re.sub("\n+", ";;; ", text)

                # generating necessary bits 
                # fName = date+"_"+unitType+"_"+str(c)

                itemID = "#ID: " + date+"_"+unitType+"_"+str(c)
                dateVar   = "#DATE: " + date
                unitType = "#TYPE: " + unitType
                header = "#HEADER: " + header
                text = "#TEXT: " + text

                # creating a text variable with each entry on a new line
                var = "\n".join([itemID,dateVar,unitType,header,text]) + "\n"
                #input(var)
                # appending variable to list
                list.append(var)

                # saving


with open(target + "/" + "total_data_structured" +".tsv", "w", encoding="utf8") as f9:
    for item in list:
        f9.write(item)

        # count processed issues and print progress counter at every 100        
        counter += 1
        if counter % 100 == 0:
            print(counter)
```
