======================
#Association Rule Implementation
#By HENRY OBINNA OZOALOR

import os
import sys
import math
import csv
import itertools
#----------------------------------------------------
#PART 1 - EXTRACTING THE 34 FEATURES
#----------------------------------------------------
Records = {1: 'republican',\
           2: 'democrat',\
           3: 'handicapped-infants = yes',\
           4: 'handicapped-infants = no',\
           5: 'water-project-cost-sharing = yes',\
           6: 'water-project-cost-sharing = no',\
           7: 'adoption-of-the-budget-resolution = yes',\
           8: 'adoption-of-the-budget-resolution = no',\
           9: 'physician-fee-freeze = yes',\
           10: 'physician-fee-freeze = no',\
           11: 'el-salvador-aid = yes',\
           12: 'el-salvador-aid = no',\
           13: 'religious-groups-in-schools = yes',\
           14: 'religious-groups-in-schools = no',\
           15: 'anti-satellite-test-ban = yes',\
           16: 'anti-satellite-test-ban = no',\
           17: 'aid-to-nicaraguan-contras = yes',\
           18: 'aid-to-nicaraguan-contras = no',\
           19: 'mx-missile = yes',\
           20: 'mx-missile = no',\
           21: 'immigration = yes',\
           22: 'immigration = no',\
           23: 'synfuels-corporation-cutback = yes',\
           24: 'synfuels-corporation-cutback = no',\
           25: 'education-spending = yes',\
           26: 'education-spending = no',\
           27: 'superfund-right-to-sue = yes',\
           28: 'superfund-right-to-sue = no',\
           29: 'crime = yes',\
           30: 'crime = no',\
           31: 'duty-free-exports = yes',\
           32: 'duty-free-exports = no',\
           33: 'export-administration-act = yes',\
           34: 'export-administration-act = n0'}


#This imports the data file house-votes-84 and appends it to dataset
datafile = open(r'C:\Python33\project_2\house_votes.csv')
datareader = csv.reader(datafile)

dataset = []    #Contains the entire dataset from house-votes-84
for line in datareader:
    dataset.append(line)

#converting the features to numbers for easy analysis
i = 0
while(i < len(dataset)):
    j = 0
    while(j < len(dataset[i])):
        if 'republican' in dataset[i]:
            dataset[i][j] = 1
        elif 'democrat' in dataset[i]:
            dataset[i][j] = 2
        j += 1
    i += 1    

i = 0
while(i < len(dataset)):
    if dataset[i][1] is 'y':
        dataset[i][1] = 3            
    elif dataset[i][1] is 'n':
        dataset[i][1] = 4
    i += 1    
i = 0
while(i < len(dataset)):
    if dataset[i][2] is 'y':
        dataset[i][2] = 5            
    elif dataset[i][2] is 'n':
        dataset[i][2] = 6
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][3] is 'y':
        dataset[i][3] = 7           
    elif dataset[i][3] is 'n':
        dataset[i][3] = 8
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][4] is 'y':
        dataset[i][4] = 9        
    elif dataset[i][4] is 'n':
        dataset[i][4] = 10
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][5] is 'y':
        dataset[i][5] = 11           
    elif dataset[i][5] is 'n':
        dataset[i][5] = 12
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][6] is 'y':
        dataset[i][6] = 13           
    elif dataset[i][6] is 'n':
        dataset[i][6] = 14
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][7] is 'y':
        dataset[i][7] = 15            
    elif dataset[i][7] is 'n':
        dataset[i][7] = 16
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][8] is 'y':
        dataset[i][8] = 17           
    elif dataset[i][8] is 'n':
        dataset[i][8] = 18
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][9] is 'y':
        dataset[i][9] = 19        
    elif dataset[i][9] is 'n':
        dataset[i][9] = 20
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][10] is 'y':
        dataset[i][10] = 21           
    elif dataset[i][10] is 'n':
        dataset[i][10] = 22
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][11] is 'y':
        dataset[i][11] = 23           
    elif dataset[i][11] is 'n':
        dataset[i][11] = 24
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][12] is 'y':
        dataset[i][12] = 25            
    elif dataset[i][12] is 'n':
        dataset[i][12] = 26
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][13] is 'y':
        dataset[i][13] = 27           
    elif dataset[i][13] is 'n':
        dataset[i][13] = 28
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][14] is 'y':
        dataset[i][14] = 29        
    elif dataset[i][14] is 'n':
        dataset[i][14] = 30
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][15] is 'y':
        dataset[i][15] = 31           
    elif dataset[i][15] is 'n':
        dataset[i][15] = 32
    i += 1
i = 0
while(i < len(dataset)):
    if dataset[i][16] is 'y':
        dataset[i][16] = 33           
    elif dataset[i][16] is 'n':
        dataset[i][16] = 34
    i += 1

#-------------------------------------------------------
#PART 2 - GENERATING THE FREQUENT ITEM SETS
#-------------------------------------------------------
#Generating frequent item sets of size k = 1
def k1(j, d):
    Min_sup = 0.3
    l1 = []
    sub = []
    
    for item in d:
        if j in item:
            l1.append(item)
            
    sup_data = len(l1)/len(d)
    sub_data = sup_data #j, sup_data
    
    if sup_data >= Min_sup:
        sub.append(sub_data)
        
    return sub
print('/////////////////GENERATING FREQUENT ITEMSETS///////////////')
#i = 1
#for y in range(1,35):
#   print(i, k1(y, dataset))
#   i += 1
print('\n------------- 5 MOST FREQUENT ITEMSETS OF SIZE K = 1 -----------')
print('Rank 1 ',Records[13],'        ',k1(13, dataset))
print('Rank 2 ',Records[33],'        ',k1(33, dataset))
print('Rank 3 ',Records[2],'                                 ',k1(2, dataset))
print('Rank 4 ',Records[24],'                        ',k1(24, dataset))
print('Rank 5 ',Records[7],'                        ',k1(7, dataset))

#Generating frequent item sets of size k = 2 to 4
def kk1(j, d):
    Min_sup = 0.3
    l1 = []
    sub = []    
    for item in d:
        if j in item:
            l1.append(item)            
    sup_data = len(l1)/len(d)
    sub_data = j    
    if sup_data >= Min_sup:
        sub.append(sub_data)        
    return sub       

ease = []
for y in range(1, 34):
    ease.append(kk1(y, dataset))

k_1 = []        #This holds the frequent item set for easy manipulation
i = 0
while(i < len(ease)):
    k_1.append(ease[i][0])
    i += 1
    
def k2(j, d, k):
    comb = []
    for n in itertools.combinations(d, k):
        comb.append(n)
        
    Min_sup = 0.3
    l2 = []
    sub = []
    i = 0
    while(i < len(dataset)):        
        if set(comb[j]).issubset(set(dataset[i])):
            l2.append(comb[j])           
        i += 1
        
    sup_data = len(l2)/len(dataset)
    sub_data = sup_data #comb[j], sup_data

    if sup_data >= Min_sup:
        sub.append(sub_data)
        
    return sub
i = 0
#for y in range(200):
 #   print(i, (k2(y, k_1, 2)))
  #  i += 1
print('\n------------- 5 MOST FREQUENT ITEMSETS OF SIZE K = 2 -----------')
print('Rank 1 ', Records[2],'',Records[10],'           ',k2(39, k_1, 2))
print('Rank 2 ', Records[2],'',Records[7],'            ',k2(36, k_1, 2))
print('Rank 3 ', Records[2],'',Records[17],'                       ',k2(46, k_1, 2))
print('Rank 4 ', Records[2],'',Records[26],'                  ',k2(55, k_1, 2))
print('Rank 5 ', Records[2],'',Records[15],'                  ',k2(44, k_1, 2))

#i = 400
#for y in range(400, 1100):
#    print(i, (k2(y, k_1, 3)))
#    i += 1

print('\n------------- 5 MOST FREQUENT ITEMSETS OF SIZE K = 3 -----------')
print('Rank 1 ', Records[2],'',Records[7],'',Records[10],'              ',k2(612, k_1, 3))
print('Rank 2 ', Records[2],'',Records[10],'',Records[17],'              ',k2(691, k_1, 3))
print('Rank 3 ', Records[2],'',Records[7],'',Records[17],'                           ',k2(619, k_1, 3))
print('Rank 4 ', Records[2],'',Records[10],'',Records[26],'              ',k2(700, k_1, 3))
print('Rank 5 ', Records[2],'',Records[12],'',Records[17],'                         ',k2(734, k_1, 3))

#i = 10000
#for y in range(10000, 15000):
#    print(i, (k2(y, k_1, 4)))
#    i += 1

print('\n------------- 5 MOST FREQUENT ITEMSETS OF SIZE K = 4 -----------')
print('Rank 1 ', Records[2],'',Records[7],'',Records[10],'',Records[17],'      ',k2(6585, k_1, 4))
print('Rank 2 ', Records[2],'',Records[10],'',Records[12],'',Records[17],'                  ',k2(7457, k_1, 4))
print('Rank 3 ', Records[2],'',Records[7],'',Records[10],'',Records[26],'                         ',k2(6594, k_1, 4))
print('Rank 4 ', Records[2],'',Records[7],'',Records[10],'',Records[15],'              ',k2(6583, k_1, 4))
print('Rank 5 ', Records[2],'',Records[7],'',Records[10],' ',Records[12],'                         ',k2(6580, k_1, 4))


#-------------------------------------------------------
#PART 3 - GENERATING THE ASSOCIATION RULES
#-------------------------------------------------------
#Calculating the confidence
def conf(k4, k3):
    min_conf = 0.9
    r3 = []
    r4 = []
    rule = []
    i = 0
    while(i < len(dataset)):
        if set(k4).issubset(set(dataset[i])):
            r4.append(k4)
        i += 1
    i = 0
    while(i < len(dataset)):
        if set(k3).issubset(set(dataset[i])):
            r3.append(k3)
        i += 1

    conf_data = len(r4)/len(r3)
    if conf_data >= min_conf:
        rule.append(conf_data)
    return rule

print('\n/////////////////GENERATING ASSOCIATION RULES///////////////\n')
print('Rank 1 ', Records[7],'',Records[10],'',Records[17], '        --->   ', Records[2],'        ', conf((2,7,10,17),(7,10,17)))
print('Rank 2 ', Records[7],'',Records[10],'',Records[26], '               --->   ', Records[2],'        ', conf((2,7,10,26),(7,10,26)))
print('Rank 3 ', Records[7],'',Records[10],'',Records[15], '          --->   ', Records[2],'        ', conf((2,7,10,15),(7,10,15)))
print('Rank 4 ', Records[7],'',Records[10],'',Records[12], '                   --->   ', Records[2],'        ', conf((2,7,10,12),(7,10,12)))
print('Rank 5 ', Records[10],'',Records[12],'',Records[17], '                --->   ', Records[2],'        ', conf((2,10,12,17),(10,12,17)))


print('\n///////////////////////////////////// END ////////////////////////////////////////////')
