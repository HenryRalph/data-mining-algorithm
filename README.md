data-mining-algorithm
==================
#A Decision tree Algorithm
#By Henry Obinna Ozoalor

import csv
import os
import sys
import math
import random

#Defining a function for the entropy
#And making an exception when there is a possibility of an error
def entropy_1(x, y, z):
    z = x + y
    if x == 0 or y == 0:
        return 0        
    else:        
        entropy1 = ((-x/z) * math.log(x/z, 2)) - ((y/z) * math.log(y/z, 2))
    return entropy1    

def entropy_2(a, y, c, d, x, b, o):
    d = x + b + o
    if x == 0 or b == 0 or o == 0 or d == 0:
        return 0
    else:
        entropy2 = ((x/d)*a) + ((b/d)*y) + ((o/d)*c)
    return entropy2

#importing the dataset
datafile = open(r'C:\Users\Engr Dazilli\Documents\MY COURSES\Spring 2014\Data Mining (EG6312)\Project Implementations\dataset.csv')
datareader = csv.reader(datafile)
data = []
Plist = []
Nlist = []
for row in datareader:
        data.append(row)
random.shuffle(data)
i = 0
while(i < len(data)):
        if 'positive' in data[i]:
                Plist.append(data[i])
        else:
                Nlist.append(data[i])
        i += 1
#Partitioning the dataset        
train75P = 0.75 * len(Plist)    #469.5 >> 470
test25P = len(Plist) - train75P 
TrainDataP = Plist[:470]
TestDataP = Plist[470:]

train75N = 0.75 * len(Nlist)    #249 >> 250
test25N = len(Nlist) - train75N
TrainDataN = Nlist[:250]
TestDataN = Nlist[250:]

TrainingSet = TrainDataP + TrainDataN   # 75% of dataset
TestSet = TestDataP + TestDataN         # 25% of dataset


#----------------------------------------------------------------
#Finding the root node
#----------------------------------------------------------------
def attr_1(j):
    A0xp = []
    A0xn = []
    A0bp = []
    A0bn = []
    A0op = []
    A0on = []
    i = 0    
    while(i < len(TrainingSet)):        
            if TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                A0xp.append(TrainingSet[i])
            elif TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                A0xn.append(TrainingSet[i])
            elif TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:        
                A0bp.append(TrainingSet[i])
            elif TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                A0bn.append(TrainingSet[i])
            elif TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                A0op.append(TrainingSet[i])
            elif TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                A0on.append(TrainingSet[i])
            i += 1
    totalA0x = len(A0xp) + len(A0xn)          
    entropyA0x = entropy_1(len(A0xp), len(A0xn), totalA0x)         
    totalA0b = len(A0bp) + len(A0bn)
    entropyA0b = entropy_1(len(A0bp), len(A0bn), totalA0b)
    totalA0o = len(A0op) + len(A0on)
    entropyA0o = entropy_1(len(A0op), len(A0on), totalA0o)
    totalA0xbo = totalA0x + totalA0b + totalA0o
    entropyA0xbo = (((totalA0x/totalA0xbo)*entropyA0x) + ((totalA0b/totalA0xbo)*entropyA0b) + ((totalA0o/totalA0xbo)*entropyA0o))
    totalA0p = len(A0op) + len(A0bp) + len(A0xp)
    totalA0n = len(A0on) + len(A0bn) + len(A0xn)
    totalA0 = totalA0p + totalA0n
    entropyA0 = entropy_1(totalA0p, totalA0n, totalA0)
    gain = entropyA0 - entropyA0xbo
    return(gain, '   ', len(A0xp), len(A0xn), len(A0bp), len(A0bn), len(A0op), len(A0on))
for item in range(0, 9):
    print(attr_1(item))
print('root Node-----A4-------\n')
#----------------------------------------------------------------------
#Building the next level    When A4 is x
#--------------------------------------------------------------------
def attr_2(j):
    A02xp = []
    A02xn = []
    A02bp = []
    A02bn = []
    A02op = []
    A02on = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                A02xp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                A02xn.append(TrainingSet[i])
        if TrainingSet[i][4] is 'x' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                A02bp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                A02bn.append(TrainingSet[i])
        if TrainingSet[i][4] is 'x' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                A02op.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                A02on.append(TrainingSet[i])
        i += 1
    totalA02x = len(A02xp) + len(A02xn)
    entropyA02x = entropy_1(len(A02xp), len(A02xn), totalA02x)
    totalA02b = len(A02bp) + len(A02bn)
    entropyA02b = entropy_1(len(A02bp), len(A02bn), totalA02b)
    totalA02o = len(A02op) + len(A02on)
    entropyA02o = entropy_1(len(A02op), len(A02on), totalA02o)
    totalA02xbo = totalA02x + totalA02b + totalA02o     
    entropyA02xbo = (((totalA02x/totalA02xbo) * entropyA02x) + ((totalA02b/totalA02xbo) * entropyA02b) + ((totalA02o/totalA02xbo) * entropyA02o))                
    totalA02p = len(A02xp) + len(A02bp) + len(A02op)
    totalA02n = len(A02xn) + len(A02bn) + len(A02on)
    totalA02 = totalA02p + totalA02n
    entropyA02 = entropy_1(totalA02p, totalA02n, totalA02)
    gainA02 = entropyA02 - entropyA02xbo 
    return(gainA02, '   ', len(A02xp), len(A02xn), len(A02bp), len(A02bn), len(A02op), len(A02on))
for item in range(0, 9):
    print(attr_2(item))
print('Best Node-----A2-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is b
#--------------------------------------------------------------------

def attr_3(j):
    A02bxp = []
    A02bxn = []
    A02bbp = []
    A02bbn = []
    A02bop = []
    A02bon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                A02bxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                A02bxn.append(TrainingSet[i])
        if TrainingSet[i][4] is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                A02bbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                A02bbn.append(TrainingSet[i])
        if TrainingSet[i][4] is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                A02bop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                A02bon.append(TrainingSet[i])
        i += 1
    totalA02bx = len(A02bxp) + len(A02bxn)
    entropyA02bx = entropy_1(len(A02bxp), len(A02bxn), totalA02bx)
    totalA02bb = len(A02bbp) + len(A02bbn)
    entropyA02bb = entropy_1(len(A02bbp), len(A02bbn), totalA02bb)
    totalA02bo = len(A02bop) + len(A02bon)
    entropyA02bo = entropy_1(len(A02bop), len(A02bon), totalA02bo)
    totalA02bxbo = totalA02bx + totalA02bb + totalA02bo     
    entropyA02bxbo = (((totalA02bx/totalA02bxbo) * entropyA02bx) + ((totalA02bb/totalA02bxbo) * entropyA02bb) + ((totalA02bo/totalA02bxbo) * entropyA02bo))
    totalA02bp = len(A02bxp) + len(A02bbp) + len(A02bop)
    totalA02bn = len(A02bxn) + len(A02bbn) + len(A02bon)
    totalA02b = totalA02bp + totalA02bn
    entropyA02b = entropy_1(totalA02bp, totalA02bn, totalA02b)
    gainA02b = entropyA02b - entropyA02bxbo
    return(gainA02b, '   ', len(A02bxp), len(A02bxn), len(A02bbp), len(A02bbn), len(A02bop), len(A02bon))
for item in range(0, 9):    
    print(attr_3(item))
print('Best Node-----A0-------\n')        
#----------------------------------------------------------------------
#Building the next level when A4 is o
#--------------------------------------------------------------------          

def attr_4(j):
    A12cxp = []
    A12cxn = []
    A12cbp = []
    A12cbn = []
    A12cop = []
    A12con = []
    i = 0
    while(i < len(TrainingSet)):        
        if TrainingSet[i][4] is 'o' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                A12cxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                A12cxn.append(TrainingSet[i])
        if TrainingSet[i][4] is 'o' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                A12cbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                A12cbn.append(TrainingSet[i])
        if TrainingSet[i][4] is 'o' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                A12cop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                A12con.append(TrainingSet[i])
        i += 1  
    totalA12cx = len(A12cxp) + len(A12cxn)
    entropyA12cx = entropy_1(len(A12cxp), len(A12cxn), totalA12cx)
    totalA12cb = len(A12cbp) + len(A12cbn)
    entropyA12cb = entropy_1(len(A12cbp), len(A12cbn), totalA12cb)
    totalA12co = len(A12cop) + len(A12con)
    entropyA12co = entropy_1(len(A12cop), len(A12con), totalA12co)
    totalA12cxbo = totalA12cx + totalA12cb + totalA12co     
    entropyA12cxbo = (((totalA12cx/totalA12cxbo) * entropyA12cx) + ((totalA12cb/totalA12cxbo) * entropyA12cb) + ((totalA12co/totalA12cxbo) * entropyA12co))
    totalA12cp = len(A12cxp) + len(A12cbp) + len(A12cop)
    totalA12cn = len(A12cxn) + len(A12cbn) + len(A12con)
    totalA12c = totalA12cp + totalA12cn
    entropyA12c = entropy_1(totalA12cp, totalA12cn, totalA12c)
    gainA12c = entropyA12c - entropyA12cxbo
    return(gainA12c, '   ', len(A12cxp), len(A12cxn), len(A12cbp), len(A12cbn), len(A12cop), len(A12con))
for item in range(0, 9):    
    print(attr_4(item))
print('Best Node------A0------\n')      
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is x
#--------------------------------------------------------------------             

def attr_5(j):
    L1xp = []
    L1xn = []
    L1bp = []
    L1bn = []
    L1op = []
    L1on = []
    i = 0
    while(i < len(TrainingSet)):        
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'x'  and 'positive' in TrainingSet[i]:
                L1xp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                L1xn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'b'  and 'positive' in TrainingSet[i]:
                L1bp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                L1bn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'o'  and 'positive' in TrainingSet[i]:
                L1op.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                L1on.append(TrainingSet[i])
        i += 1
    totalL1x = len(L1xp) + len(L1xn)
    entropyL1x = entropy_1(len(L1xp), len(L1xn), totalL1x)
    totalL1b = len(L1bp) + len(L1bn)
    entropyL1b = entropy_1(len(L1bp), len(L1bn), totalL1b)
    totalL1o = len(L1op) + len(L1on)
    entropyL1o = entropy_1(len(L1op), len(L1on), totalL1o)
    totalL1xbo = totalL1x + totalL1b + totalL1o     
    entropyL1xbo = (((totalL1x/totalL1xbo) * entropyL1x) + ((totalL1b/totalL1xbo) * entropyL1b) + ((totalL1o/totalL1xbo) * entropyL1o))
    totalL1p = len(L1xp) + len(L1bp) + len(L1op)
    totalL1n = len(L1xn) + len(L1bn) + len(L1on)
    totalL1 = totalL1p + totalL1n
    entropyL1 = entropy_1(totalL1p, totalL1n, totalL1)
    gainL1 = entropyL1 - entropyL1xbo 
    return(gainL1, '   ', len(L1xp), len(L1xn), len(L1bp), len(L1bn), len(L1op), len(L1on))
for item in range(0, 9):    
    print(attr_5(item))
print('Best Node-----A6-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is b
#--------------------------------------------------------------------

def attr_6(j):
    M3xp = []
    M3xn = []
    M3bp = []
    M3bn = []
    M3op = []
    M3on = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'x'  and 'positive' in TrainingSet[i]:
                M3xp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                M3xn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'b'  and 'positive' in TrainingSet[i]:
                M3bp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                M3bn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'o'  and 'positive' in TrainingSet[i]:
                M3op.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                M3on.append(TrainingSet[i])
        i += 1
    totalM3x = len(M3xp) + len(M3xn)
    entropyM3x = entropy_1(len(M3xp), len(M3xn), totalM3x)
    totalM3b = len(M3bp) + len(M3bn)
    entropyM3b = entropy_1(len(M3bp), len(M3bn), totalM3b)
    totalM3o = len(M3op) + len(M3on)
    entropyM3o = entropy_1(len(M3op), len(M3on), totalM3o)
    totalM3xbo = totalM3x + totalM3b + totalM3o     
    entropyM3xbo = (((totalM3x/totalM3xbo) * entropyM3x) + ((totalM3b/totalM3xbo) * entropyM3b) + ((totalM3o/totalM3xbo) * entropyM3o))
    totalM3p = len(M3xp) + len(M3bp) + len(M3op)
    totalM3n = len(M3xn) + len(M3bn) + len(M3on)
    totalM3 = totalM3p + totalM3n
    entropyM3 = entropy_1(totalM3p, totalM3n, totalM3)
    gainM3 = entropyM3 - entropyM3xbo
    return(gainM3, '   ', len(M3xp), len(M3xn), len(M3bp), len(M3bn), len(M3op), len(M3on))
for item in range(0, 9):    
    print(attr_6(item))
print('Best Node-----A5-------\n')
#----------------------------------------------------------------------
#Building the third level when A4 is x and A2 is o
#--------------------------------------------------------------------

def attr_7(j):
    N5xp = []
    N5xn = []
    N5bp = []
    N5bn = []
    N5op = []
    N5on = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'x'  and 'positive' in TrainingSet[i]:
                N5xp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                N5xn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'b'  and 'positive' in TrainingSet[i]:
                N5bp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                N5bn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'o'  and 'positive' in TrainingSet[i]:
                N5op.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                N5on.append(TrainingSet[i])
        i += 1
    totalN5x = len(N5xp) + len(N5xn)
    entropyN5x = entropy_1(len(N5xp), len(N5xn), totalN5x)
    totalN5b = len(N5bp) + len(N5bn)
    entropyN5b = entropy_1(len(N5bp), len(N5bn), totalN5b)
    totalN5o = len(N5op) + len(N5on)
    entropyN5o = entropy_1(len(N5op), len(N5on), totalN5o)
    totalN5xbo = totalN5x + totalN5b + totalN5o     
    entropyN5xbo = (((totalN5x/totalN5xbo) * entropyN5x) + ((totalN5b/totalN5xbo) * entropyN5b) + ((totalN5o/totalN5xbo) * entropyN5o))
    totalN5p = len(N5xp) + len(N5bp) + len(N5op)
    totalN5n = len(N5xn) + len(N5bn) + len(N5on)
    totalN5 = totalN5p + totalN5n
    entropyN5 = entropy_1(totalN5p, totalN5n, totalN5)
    gainN5 = entropyN5 - entropyN5xbo
    return(gainN5, '   ', len(N5xp), len(N5xn), len(N5bp), len(N5bn), len(N5op), len(N5on))
for item in range(0, 9):    
    print(attr_7(item))
print('Best Node-----A0-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x
#--------------------------------------------------------------------

def attr_8(j):
    bxp = []
    bxn = []
    bbp = []
    bbn = []
    bop = []
    bon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'x'  and 'positive' in TrainingSet[i]:
                bxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                bxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'b'  and 'positive' in TrainingSet[i]:
                bbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'o'  and 'positive' in TrainingSet[i]:
                bop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                bon.append(TrainingSet[i])
        i += 1
    totalbx = len(bxp) + len(bxn)
    entropybx = entropy_1(len(bxp), len(bxn), totalbx)
    totalbb = len(bbp) + len(bbn)
    entropybb = entropy_1(len(bbp), len(bbn), totalbb)
    totalbo = len(bop) + len(bon)
    entropybo = entropy_1(len(bop), len(bon), totalbo)
    totalbxbo = totalbx + totalbb + totalbo     
    entropybxbo = ((totalbx/totalbxbo) * entropybx) + ((totalbb/totalbxbo) * entropybb) + ((totalbo/totalbxbo) * entropybo)
    totalbp = len(bxp) + len(bbp) + len(bop)
    totalbn = len(bxn) + len(bbn) + len(bon)
    totalb = totalbp + totalbn
    entropyb = entropy_1(totalbp, totalbn, totalb)
    gainb = entropyb - entropybxbo
    return(gainb, '   ', len(bxp), len(bxn), len(bbp), len(bbn), len(bop), len(bon))
for item in range(0, 9):    
    print(attr_8(item))
print('Best Node-----A8-------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is o
#--------------------------------------------------------------------

def attr_9(j):
    cxp = []
    cxn = []
    cbp = []
    cbn = []
    cop = []
    con = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'x'  and 'positive' in TrainingSet[i]:
                cxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                cxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'b'  and 'positive' in TrainingSet[i]:
                cbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                cbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'o'  and 'positive' in TrainingSet[i]:
                cop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                con.append(TrainingSet[i])
        i += 1
    totalcx = len(cxp) + len(cxn)
    entropycx = entropy_1(len(cxp), len(cxn), totalcx)
    totalcb = len(cbp) + len(cbn)
    entropycb = entropy_1(len(cbp), len(cbn), totalcb)
    totalco = len(cop) + len(con)
    entropyco = entropy_1(len(cop), len(con), totalco)
    totalcxo = totalcx + totalcb + totalco    
    entropycxo = ((totalcx/totalcxo) * entropycx) + ((totalcb/totalcxo) * entropycb) + ((totalco/totalcxo) * entropyco)
    totalcp = len(cxp) + len(cbp) + len(cop)
    totalcn = len(cxn) + len(cbn) + len(con)
    totalc = totalcp + totalcn
    entropyc = entropy_1(totalcp, totalcn, totalc)
    gainc = entropyc - entropycxo
    return(gainc, '   ', len(cxp), len(cxn), len(cbp), len(cbn), len(cop), len(con))
for item in range(0, 9):    
    print(attr_9(item))
print('Best Node-----A8-------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is b and A0 is x
#--------------------------------------------------------------------

def attr_10(j):
    dxp = []
    dxn = []
    dbp = []
    dbn = []
    dop = []
    don = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'x'  and 'positive' in TrainingSet[i]:
                dxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                dxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'b'  and 'positive' in TrainingSet[i]:
                dbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                dbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'o'  and 'positive' in TrainingSet[i]:
                dop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                don.append(TrainingSet[i])
        i += 1
    totaldx = len(dxp) + len(dxn)
    entropydx = entropy_1(len(dxp), len(dxn), totaldx)
    totaldb = len(dbp) + len(dbn)
    entropydb = entropy_1(len(dbp), len(dbn), totaldb)
    totaldo = len(dop) + len(don)
    entropydo = entropy_1(len(dop), len(don), totaldo)
    totaldxo = totaldx + totaldb + totaldo    
    entropydxo = ((totaldx/totaldxo) * entropydx) + ((totaldb/totaldxo) * entropydb) + ((totaldo/totaldxo) * entropydo)
    totaldp = len(dxp) + len(dbp) + len(dop)
    totaldn = len(dxn) + len(dbn) + len(don)
    totald = totaldp + totaldn
    entropyd = entropy_1(totaldp, totaldn, totald)
    gaind = entropyd - entropydxo
    return(gaind, '   ', len(dxp), len(dxn), len(dbp), len(dbn), len(dop), len(don))
for item in range(0, 9):    
    print(attr_10(item))
print('Best Node------A8------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is b and A0 is o
#--------------------------------------------------------------------

def attr_11(j):
    exp = []
    exn = []
    ebp = []
    ebn = []
    eop = []
    eon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'x'  and 'positive' in TrainingSet[i]:
                exp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                exn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'b'  and 'positive' in TrainingSet[i]:
                ebp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                ebn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'o'  and 'positive' in TrainingSet[i]:
                eop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'b' and TrainingSet[i][0] is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                eon.append(TrainingSet[i])
        i += 1
    totalex = len(exp) + len(exn)
    entropyex = entropy_1(len(exp), len(exn), totalex)
    totaleb = len(ebp) + len(ebn)
    entropyeb = entropy_1(len(ebp), len(ebn), totaleb)
    totaleo = len(eop) + len(eon)
    entropyeo = entropy_1(len(eop), len(eon), totaleo)
    totalexo = totalex + totaleb + totaleo    
    entropyexo = ((totalex/totalexo) * entropyex) + ((totaleb/totalexo) * entropyeb) + ((totaleo/totalexo) * entropyeo)
    totalep = len(exp) + len(ebp) + len(eop)
    totalen = len(exn) + len(ebn) + len(eon)
    totale = totalep + totalen
    entropye = entropy_1(totalep, totalen, totale)
    gaine = entropye - entropyexo
    return(gaine, '   ', len(exp), len(exn), len(ebp), len(ebn), len(eop), len(eon))
for item in range(0, 9):    
    print(attr_11(item))
print('Best Node-----A8-------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is b and A5 is x
#--------------------------------------------------------------------

def attr_12(j):
    fxp = []
    fxn = []
    fbp = []
    fbn = []
    fop = []
    fon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                fxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                fxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                fbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                fbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                fop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                fon.append(TrainingSet[i])
        i += 1
    totalfx = len(fxp) + len(fxn)
    entropyfx = entropy_1(len(fxp), len(fxn), totalfx)
    totalfb = len(fbp) + len(fbn)
    entropyfb = entropy_1(len(fbp), len(fbn), totalfb)
    totalfo = len(fop) + len(fon)
    entropyfo = entropy_1(len(fop), len(fon), totalfo)
    totalfxo = totalfx + totalfb + totalfo    
    entropyfxo = ((totalfx/totalfxo) * entropyfx) + ((totalfb/totalfxo) * entropyfb) + ((totalfo/totalfxo) * entropyfo)
    totalfp = len(fxp) + len(fbp) + len(fop)
    totalfn = len(fxn) + len(fbn) + len(fon)
    totalf = totalfp + totalfn
    entropyf = entropy_1(totalfp, totalfn, totalf)
    gainf = entropyf - entropyfxo
    return(gainf, '   ', len(fxp), len(fxn), len(fbp), len(fbn), len(fop), len(fon))
for item in range(0, 9):    
    print(attr_12(item))
print('Best Node----A3--------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is b and A5 is b
#--------------------------------------------------------------------

def attr_13(j):
    gxp = []
    gxn = []
    gbp = []
    gbn = []
    gop = []
    gon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'b'  and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                gxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'b'  and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                gxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'b'  and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                gbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'b'  and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                gbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'b'  and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                gop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'b'  and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                gon.append(TrainingSet[i])
        i += 1
    totalgx = len(gxp) + len(gxn)
    entropygx = entropy_1(len(gxp), len(gxn), totalgx)
    totalgb = len(gbp) + len(gbn)
    entropygb = entropy_1(len(gbp), len(gbn), totalgb)
    totalgo = len(gop) + len(gon)
    entropygo = entropy_1(len(gop), len(gon), totalgo)
    totalgxo = totalgx + totalgb + totalgo    
    entropygxo = ((totalgx/totalgxo) * entropygx) + ((totalgb/totalgxo) * entropygb) + ((totalgo/totalgxo) * entropygo)
    totalgp = len(gxp) + len(gbp) + len(gop)
    totalgn = len(gxn) + len(gbn) + len(gon)
    totalg = totalgp + totalgn
    entropyg = entropy_1(totalgp, totalgn, totalg)
    gaing = entropyg - entropygxo
    return(gaing, '   ', len(gxp), len(gxn), len(gbp), len(gbn), len(gop), len(gon))
for item in range(0, 9):    
    print(attr_13(item))
print('Best Node------A6------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is b and A5 is o
#--------------------------------------------------------------------

def attr_14(j):
    hxp = []
    hxn = []
    hbp = []
    hbn = []
    hop = []
    hon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'o'  and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                hxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'o'  and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                hxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'o'  and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                hbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'o'  and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                hbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'o'  and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                hop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'o'  and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                hon.append(TrainingSet[i])
        i += 1
    totalhx = len(hxp) + len(hxn)
    entropyhx = entropy_1(len(hxp), len(hxn), totalhx)
    totalhb = len(hbp) + len(hbn)
    entropyhb = entropy_1(len(hbp), len(hbn), totalhb)
    totalho = len(hop) + len(hon)
    entropyho = entropy_1(len(hop), len(hon), totalho)
    totalhxo = totalhx + totalhb + totalho    
    entropyhxo = ((totalhx/totalhxo) * entropyhx) + ((totalhb/totalhxo) * entropyhb) + ((totalho/totalhxo) * entropyho)
    totalhp = len(hxp) + len(hbp) + len(hop)
    totalhn = len(hxn) + len(hbn) + len(hon)
    totalh = totalhp + totalhn
    entropyh = entropy_1(totalhp, totalhn, totalh)
    gainh = entropyh - entropyhxo
    return(gainh, '   ', len(hxp), len(hxn), len(hbp), len(hbn), len(hop), len(hon))
for item in range(0, 9):    
    print(attr_14(item))
print('Best Node-----A8-------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is x
#--------------------------------------------------------------------

def attr_15(j):
    ixp = []
    ixn = []
    ibp = []
    ibn = []
    iop = []
    ion = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                ixp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                ixn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                ibp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                ibn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                iop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                ion.append(TrainingSet[i])
        i += 1
    totalix = len(ixp) + len(ixn)
    entropyix = entropy_1(len(ixp), len(ixn), totalix)
    totalib = len(ibp) + len(ibn)
    entropyib = entropy_1(len(ibp), len(ibn), totalib)
    totalio = len(iop) + len(ion)
    entropyio = entropy_1(len(iop), len(ion), totalio)
    totalixo = totalix + totalib + totalio    
    entropyixo = ((totalix/totalixo) * entropyix) + ((totalib/totalixo) * entropyib) + ((totalio/totalixo) * entropyio)
    totalip = len(ixp) + len(ibp) + len(iop)
    totalin = len(ixn) + len(ibn) + len(ion)
    totali = totalip + totalin
    entropyi = entropy_1(totalip, totalin, totali)
    gaini = entropyi - entropyixo
    return(gaini, '   ', len(ixp), len(ixn), len(ibp), len(ibn), len(iop), len(ion))
for item in range(0, 9):    
    print(attr_15(item))
print('Best Node-----A2-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is b
#--------------------------------------------------------------------

def attr_16(j):
    jxp = []
    jxn = []
    jbp = []
    jbn = []
    jop = []
    jon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                jxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                jxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                jbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                jbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                jop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                jon.append(TrainingSet[i])
        i += 1
    totaljx = len(jxp) + len(jxn)
    entropyjx = entropy_1(len(jxp), len(jxn), totaljx)
    totaljb = len(jbp) + len(jbn)
    entropyjb = entropy_1(len(jbp), len(jbn), totaljb)
    totaljo = len(jop) + len(jon)
    entropyjo = entropy_1(len(jop), len(jon), totaljo)
    totaljxo = totaljx + totaljb + totaljo    
    entropyjxo = ((totaljx/totaljxo) * entropyjx) + ((totaljb/totaljxo) * entropyjb) + ((totaljo/totaljxo) * entropyjo)
    totaljp = len(jxp) + len(jbp) + len(jop)
    totaljn = len(jxn) + len(jbn) + len(jon)
    totalj = totaljp + totaljn
    entropyj = entropy_1(totaljp, totaljn, totalj)
    gainj = entropyj - entropyjxo
    return(gainj, '   ', len(jxp), len(jxn), len(jbp), len(jbn), len(jop), len(jon))
for item in range(0, 9):    
    print(attr_16(item))
print('Best Node----A7--------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o
#--------------------------------------------------------------------

def attr_17(j):
    kxp = []
    kxn = []
    kbp = []
    kbn = []
    kop = []
    kon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                kxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                kxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                kbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                kbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                kop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                kon.append(TrainingSet[i])
        i += 1
    totalkx = len(kxp) + len(kxn)
    entropykx = entropy_1(len(kxp), len(kxn), totalkx)
    totalkb = len(kbp) + len(kbn)
    entropykb = entropy_1(len(kbp), len(kbn), totalkb)
    totalko = len(kop) + len(kon)
    entropyko = entropy_1(len(kop), len(kon), totalko)
    totalkxo = totalkx + totalkb + totalko    
    entropykxo = ((totalkx/totalkxo) * entropykx) + ((totalkb/totalkxo) * entropykb) + ((totalko/totalkxo) * entropyko)
    totalkp = len(kxp) + len(kbp) + len(kop)
    totalkn = len(kxn) + len(kbn) + len(kon)
    totalk = totalkp + totalkn
    entropyk = entropy_1(totalkp, totalkn, totalk)
    gaink = entropyk - entropykxo
    return(gaink, '   ', len(kxp), len(kxn), len(kbp), len(kbn), len(kop), len(kon))
for item in range(0, 9):    
    print(attr_17(item))
print('Best Node-----A7-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is o and A0 is x
#--------------------------------------------------------------------

def attr_18(j):
    lxp = []
    lxn = []
    lbp = []
    lbn = []
    lop = []
    lon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'x'  and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                lxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'x'  and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                lxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'x'  and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                lbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'x'  and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                lbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'x'  and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                lop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'x'  and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                lon.append(TrainingSet[i])
        i += 1
    totallx = len(lxp) + len(lxn)
    entropylx = entropy_1(len(lxp), len(lxn), totallx)
    totallb = len(lbp) + len(lbn)
    entropylb = entropy_1(len(lbp), len(lbn), totallb)
    totallo = len(lop) + len(lon)
    entropylo = entropy_1(len(lop), len(lon), totallo)
    totallxo = totallx + totallb + totallo    
    entropylxo = ((totallx/totallxo) * entropylx) + ((totallb/totallxo) * entropylb) + ((totallo/totallxo) * entropylo)
    totallp = len(lxp) + len(lbp) + len(lop)
    totalln = len(lxn) + len(lbn) + len(lon)
    totall = totallp + totalln
    entropyl = entropy_1(totallp, totalln, totall)
    gainl = entropyl - entropylxo
    return(gainl, '   ', len(lxp), len(lxn), len(lbp), len(lbn), len(lop), len(lon))
for item in range(0, 9):    
    print(attr_18(item))
print('Best Node-----A8-------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is o and A0 is o
#--------------------------------------------------------------------

def attr_19(j):
    mxp = []
    mxn = []
    mbp = []
    mbn = []
    mop = []
    mon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                mxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                mxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                mbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                mbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                mop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                mon.append(TrainingSet[i])
        i += 1
    totalmx = len(mxp) + len(mxn)
    entropymx = entropy_1(len(mxp), len(mxn), totalmx)
    totalmb = len(mbp) + len(mbn)
    entropymb = entropy_1(len(mbp), len(mbn), totalmb)
    totalmo = len(mop) + len(mon)
    entropymo = entropy_1(len(mop), len(mon), totalmo)
    totalmxo = totalmx + totalmb + totalmo    
    entropymxo = ((totalmx/totalmxo) * entropymx) + ((totalmb/totalmxo) * entropymb) + ((totalmo/totalmxo) * entropymo)
    totalmp = len(mxp) + len(mbp) + len(mop)
    totalmn = len(mxn) + len(mbn) + len(mon)
    totalm = totalmp + totalmn
    entropym = entropy_1(totalmp, totalmn, totalm)
    gainm = entropym - entropymxo
    return(gainm, '   ', len(mxp), len(mxn), len(mbp), len(mbn), len(mop), len(mon))
for item in range(0, 9):    
    print(attr_19(item))
print('Best Node-----A1-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is x and A2 is x
#--------------------------------------------------------------------

def attr_20(j):
    nxp = []
    nxn = []
    nbp = []
    nbn = []
    nop = []
    non = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
           is 'x' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                nxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                nxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                nbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                nbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                nop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                non.append(TrainingSet[i])
        i += 1
    totalnx = len(nxp) + len(nxn)
    entropynx = entropy_1(len(nxp), len(nxn), totalnx)
    totalnb = len(nbp) + len(nbn)
    entropynb = entropy_1(len(nbp), len(nbn), totalnb)
    totalno = len(nop) + len(non)
    entropyno = entropy_1(len(nop), len(non), totalno)
    totalnxo = totalnx + totalnb + totalno    
    entropynxo = ((totalnx/totalnxo) * entropynx) + ((totalnb/totalnxo) * entropynb) + ((totalno/totalnxo) * entropyno)
    totalnp = len(nxp) + len(nbp) + len(nop)
    totalnn = len(nxn) + len(nbn) + len(non)
    totaln = totalnp + totalnn
    entropyn = entropy_1(totalnp, totalnn, totaln)
    gainn = entropyn - entropynxo
    return(gainn, '   ', len(nxp), len(nxn), len(nbp), len(nbn), len(nop), len(non))
for item in range(0, 9):    
    print(attr_20(item))
print('Best Node------A1------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is x and A2 is b
#--------------------------------------------------------------------

def attr_21(j):
    oxp = []
    oxn = []
    obp = []
    obn = []
    oop = []
    oon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
           is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                oxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                oxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                obp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                obn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                oop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                oon.append(TrainingSet[i])
        i += 1
    totalox = len(oxp) + len(oxn)
    entropyox = entropy_1(len(oxp), len(oxn), totalox)
    totalob = len(obp) + len(obn)
    entropyob = entropy_1(len(obp), len(obn), totalob)
    totaloo = len(oop) + len(oon)
    entropyoo = entropy_1(len(oop), len(oon), totaloo)
    totaloxo = totalox + totalob + totaloo    
    entropyoxo = ((totalox/totaloxo) * entropyox) + ((totalob/totaloxo) * entropyob) + ((totaloo/totaloxo) * entropyoo)
    totalop = len(oxp) + len(obp) + len(oop)
    totalon = len(oxn) + len(obn) + len(oon)
    totalo = totalop + totalon
    entropyo = entropy_1(totalop, totalon, totalo)
    gaino = entropyo - entropyoxo
    return(gaino, '   ', len(oxp), len(oxn), len(obp), len(obn), len(oop), len(oon))
for item in range(0, 9):    
    print(attr_21(item))
print('Best Node----A6--------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is x and A2 is o
#--------------------------------------------------------------------

def attr_22(j):
    pxp = []
    pxn = []
    pbp = []
    pbn = []
    pop = []
    pon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
           is 'o' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                pxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                pxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'o' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                pbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                pbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'o' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                pop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                pon.append(TrainingSet[i])
        i += 1
    totalpx = len(pxp) + len(pxn)
    entropypx = entropy_1(len(pxp), len(pxn), totalpx)
    totalpb = len(pbp) + len(pbn)
    entropypb = entropy_1(len(pbp), len(pbn), totalpb)
    totalpo = len(pop) + len(pon)
    entropypo = entropy_1(len(pop), len(pon), totalpo)
    totalpxo = totalpx + totalpb + totalpo    
    entropypxo = ((totalpx/totalpxo) * entropypx) + ((totalpb/totalpxo) * entropypb) + ((totalpo/totalpxo) * entropypo)
    totalpp = len(pxp) + len(pbp) + len(pop)
    totalpn = len(pxn) + len(pbn) + len(pon)
    totalp = totalpp + totalpn
    entropyp = entropy_1(totalpp, totalpn, totalp)
    gainp = entropyp - entropypxo
    return(gainp, '   ', len(pxp), len(pxn), len(pbp), len(pbn), len(pop), len(pon))
for item in range(0, 9):    
    print(attr_22(item))
print('Best Node------A6------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is b and A7 is x
#--------------------------------------------------------------------

def attr_23(j):
    qxp = []
    qxn = []
    qbp = []
    qbn = []
    qop = []
    qon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
           is 'x' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                qxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                qxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                qbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                qbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                qop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                qon.append(TrainingSet[i])
        i += 1
    totalqx = len(qxp) + len(qxn)
    entropyqx = entropy_1(len(qxp), len(qxn), totalqx)
    totalqb = len(qbp) + len(qbn)
    entropyqb = entropy_1(len(qbp), len(qbn), totalqb)
    totalqo = len(qop) + len(qon)
    entropyqo = entropy_1(len(qop), len(qon), totalqo)
    totalqxo = totalqx + totalqb + totalqo    
    entropyqxo = ((totalqx/totalqxo) * entropyqx) + ((totalqb/totalqxo) * entropyqb) + ((totalqo/totalqxo) * entropyqo)
    totalqp = len(qxp) + len(qbp) + len(qop)
    totalqn = len(qxn) + len(qbn) + len(qon)
    totalq = totalqp + totalqn
    entropyq = entropy_1(totalqp, totalqn, totalq)
    gainq = entropyq - entropyqxo
    return(gainq, '   ', len(qxp), len(qxn), len(qbp), len(qbn), len(qop), len(qon))
for item in range(0, 9):    
    print(attr_23(item))
print('Best Node------A5------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is b and A7 is b
#--------------------------------------------------------------------

def attr_24(j):
    rxp = []
    rxn = []
    rbp = []
    rbn = []
    rop = []
    ron = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
           is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                rxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                rxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                rbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                rbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                rop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                ron.append(TrainingSet[i])
        i += 1
    totalrx = len(rxp) + len(rxn)
    entropyrx = entropy_1(len(rxp), len(rxn), totalrx)
    totalrb = len(rbp) + len(rbn)
    entropyrb = entropy_1(len(rbp), len(rbn), totalrb)
    totalro = len(rop) + len(ron)
    entropyro = entropy_1(len(rop), len(ron), totalro)
    totalrxo = totalrx + totalrb + totalro    
    entropyrxo = ((totalrx/totalrxo) * entropyrx) + ((totalrb/totalrxo) * entropyrb) + ((totalro/totalrxo) * entropyro)
    totalrp = len(rxp) + len(rbp) + len(rop)
    totalrn = len(rxn) + len(rbn) + len(ron)
    totalr = totalrp + totalrn
    entropyr = entropy_1(totalrp, totalrn, totalr)
    gainr = entropyr - entropyrxo
    return(gainr, '   ', len(rxp), len(rxn), len(rbp), len(rbn), len(rop), len(ron))
for item in range(0, 9):    
    print(attr_24(item))
print('Best Node-------A1-----\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is b and A7 is o
#--------------------------------------------------------------------

def attr_25(j):
    sxp = []
    sxn = []
    sbp = []
    sbn = []
    sop = []
    son = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
           is 'o' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                sxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                sxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                sbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                sbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                sop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                son.append(TrainingSet[i])
        i += 1
    totalsx = len(sxp) + len(sxn)
    entropysx = entropy_1(len(sxp), len(sxn), totalsx)
    totalsb = len(sbp) + len(sbn)
    entropysb = entropy_1(len(sbp), len(sbn), totalsb)
    totalso = len(sop) + len(son)
    entropyso = entropy_1(len(sop), len(son), totalso)
    totalsxo = totalsx + totalsb + totalso    
    entropysxo = ((totalsx/totalsxo) * entropysx) + ((totalsb/totalsxo) * entropysb) + ((totalso/totalsxo) * entropyso)
    totalsp = len(sxp) + len(sbp) + len(sop)
    totalsn = len(sxn) + len(sbn) + len(son)
    totals = totalsp + totalsn
    entropys = entropy_1(totalsp, totalsn, totals)
    gains = entropys - entropysxo
    return(gains, '   ', len(sxp), len(sxn), len(sbp), len(sbn), len(sop), len(son))
for item in range(0, 9):    
    print(attr_25(item))
print('Best Node-----A1-------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o and A7 is x
#--------------------------------------------------------------------

def attr_26(j):
    txp = []
    txn = []
    tbp = []
    tbn = []
    top = []
    ton = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
           is 'x' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                txp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                txn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                tbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                tbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                top.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                ton.append(TrainingSet[i])
        i += 1
    totaltx = len(txp) + len(txn)
    entropytx = entropy_1(len(txp), len(txn), totaltx)
    totaltb = len(tbp) + len(tbn)
    entropytb = entropy_1(len(tbp), len(tbn), totaltb)
    totalto = len(top) + len(ton)
    entropyto = entropy_1(len(top), len(ton), totalto)
    totaltxo = totaltx + totaltb + totalto    
    entropytxo = ((totaltx/totaltxo) * entropytx) + ((totaltb/totaltxo) * entropytb) + ((totalto/totaltxo) * entropyto)
    totaltp = len(txp) + len(tbp) + len(top)
    totaltn = len(txn) + len(tbn) + len(ton)
    totalt = totaltp + totaltn
    entropyt = entropy_1(totaltp, totaltn, totalt)
    gaint = entropyt - entropytxo
    return(gaint, '   ', len(txp), len(txn), len(tbp), len(tbn), len(top), len(ton))
for item in range(0, 9):    
    print(attr_26(item))
print('Best Node------A2------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o and A7 is b
#--------------------------------------------------------------------

def attr_27(j):
    uxp = []
    uxn = []
    ubp = []
    ubn = []
    uop = []
    uon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
           is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                uxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                uxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                ubp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                ubn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                uop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                uon.append(TrainingSet[i])
        i += 1
    totalux = len(uxp) + len(uxn)
    entropyux = entropy_1(len(uxp), len(uxn), totalux)
    totalub = len(ubp) + len(ubn)
    entropyub = entropy_1(len(ubp), len(ubn), totalub)
    totaluo = len(uop) + len(uon)
    entropyuo = entropy_1(len(uop), len(uon), totaluo)
    totaluxo = totalux + totalub + totaluo    
    entropyuxo = ((totalux/totaluxo) * entropyux) + ((totalub/totaluxo) * entropyub) + ((totaluo/totaluxo) * entropyuo)
    totalup = len(uxp) + len(ubp) + len(uop)
    totalun = len(uxn) + len(ubn) + len(uon)
    totalu = totalup + totalun
    entropyu = entropy_1(totalup, totalun, totalu)
    gainu = entropyu - entropyuxo
    return(gainu, '   ', len(uxp), len(uxn), len(ubp), len(ubn), len(uop), len(uon))
for item in range(0, 9):    
    print(attr_27(item))
print('Best Node------A2------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o and A7 is o
#--------------------------------------------------------------------

def attr_28(j):
    vxp = []
    vxn = []
    vbp = []
    vbn = []
    vop = []
    von = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
           is 'o' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                vxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                vxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                vbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                vbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                vop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                von.append(TrainingSet[i])
        i += 1
    totalvx = len(vxp) + len(vxn)
    entropyvx = entropy_1(len(vxp), len(vxn), totalvx)
    totalvb = len(vbp) + len(vbn)
    entropyvb = entropy_1(len(vbp), len(vbn), totalvb)
    totalvo = len(vop) + len(von)
    entropyvo = entropy_1(len(vop), len(von), totalvo)
    totalvxo = totalvx + totalvb + totalvo    
    entropyvxo = ((totalvx/totalvxo) * entropyvx) + ((totalvb/totalvxo) * entropyvb) + ((totalvo/totalvxo) * entropyvo)
    totalvp = len(vxp) + len(vbp) + len(vop)
    totalvn = len(vxn) + len(vbn) + len(von)
    totalv = totalvp + totalvn
    entropyv = entropy_1(totalvp, totalvn, totalv)
    gainv = entropyv - entropyvxo
    return(gainv, '   ', len(vxp), len(vxn), len(vbp), len(vbn), len(vop), len(von))
for item in range(0, 9):    
    print(attr_28(item))
print('Best Node------A1------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o and A7 is x and A2 is x
#--------------------------------------------------------------------

def attr_29(j):
    wxp = []
    wxn = []
    wbp = []
    wbn = []
    wop = []
    won = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
           is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                wxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                wxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                wbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                wbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                wop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                won.append(TrainingSet[i])
        i += 1
    totalwx = len(wxp) + len(wxn)
    entropywx = entropy_1(len(wxp), len(wxn), totalwx)
    totalwb = len(wbp) + len(wbn)
    entropywb = entropy_1(len(wbp), len(wbn), totalwb)
    totalwo = len(wop) + len(won)
    entropywo = entropy_1(len(wop), len(won), totalwo)
    totalwxo = totalwx + totalwb + totalwo    
    entropywxo = entropy_2(entropywx, entropywb, entropywo, totalwxo, totalwx, totalwb, totalwo)
    totalwp = len(wxp) + len(wbp) + len(wop)
    totalwn = len(wxn) + len(wbn) + len(won)
    totalw = totalwp + totalwn
    entropyw = entropy_1(totalwp, totalwn, totalw)
    gainw = entropyw - entropywxo
    return(gainw, '   ', len(wxp), len(wxn), len(wbp), len(wbn), len(wop), len(won))
for item in range(0, 9):    
    print(attr_29(item))
print('Best Node-----A1-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o and A7 is x and A2 is b
#--------------------------------------------------------------------

def attr_30(j):
    xxp = []
    xxn = []
    xbp = []
    xbn = []
    xop = []
    xon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
           is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                xbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                xbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                xop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                xon.append(TrainingSet[i])
        i += 1
    totalxx = len(xxp) + len(xxn)
    entropyxx = entropy_1(len(xxp), len(xxn), totalxx)
    totalxb = len(xbp) + len(xbn)
    entropyxb = entropy_1(len(xbp), len(xbn), totalxb)
    totalxo = len(xop) + len(xon)
    entropyxo = entropy_1(len(xop), len(xon), totalxo)
    totalxxo = totalxx + totalxb + totalxo    
    entropyxxo = entropy_2(entropyxx, entropyxb, entropyxo, totalxxo, totalxx, totalxb, totalxo)
    totalxp = len(xxp) + len(xbp) + len(xop)
    totalxn = len(xxn) + len(xbn) + len(xon)
    totalx = totalxp + totalxn
    entropyx = entropy_1(totalxp, totalxn, totalx)
    gainx = entropyx - entropyxxo
    return(gainx, '   ', len(xxp), len(xxn), len(xbp), len(xbn), len(xop), len(xon))
for item in range(0, 9):    
    print(attr_30(item))
print('Best Node------A1------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o and A7 is x and A2 is o
#--------------------------------------------------------------------

def attr_31(j):
    yxp = []
    yxn = []
    ybp = []
    ybn = []
    yop = []
    yon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
           is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                yxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                yxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                ybp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                ybn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                yop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                yon.append(TrainingSet[i])
        i += 1
    totalyx = len(yxp) + len(yxn)
    entropyyx = entropy_1(len(yxp), len(yxn), totalyx)
    totalyb = len(ybp) + len(ybn)
    entropyyb = entropy_1(len(ybp), len(ybn), totalyb)
    totalyo = len(yop) + len(yon)
    entropyyo = entropy_1(len(yop), len(yon), totalyo)
    totalyxo = totalyx + totalyb + totalyo    
    entropyyxo = entropy_2(entropyyx, entropyyb, entropyyo, totalyxo, totalyx, totalyb, totalyo)
    totalyp = len(yxp) + len(ybp) + len(yop)
    totalyn = len(yxn) + len(ybn) + len(yon)
    totaly = totalyp + totalyn
    entropyy = entropy_1(totalyp, totalyn, totaly)
    gainy = entropyy - entropyyxo
    return(gainy, '   ', len(yxp), len(yxn), len(ybp), len(ybn), len(yop), len(yon))
for item in range(0, 9):    
    print(attr_31(item))
print('Best Node----A1--------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is x and A2 is x and A1 is b
#--------------------------------------------------------------------

def attr_32(j):
    zxp = []
    zxn = []
    zbp = []
    zbn = []
    zop = []
    zon = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
           is 'x' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                zxp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                zxn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                zbp.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                zbn.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                zop.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                zon.append(TrainingSet[i])
        i += 1
    totalzx = len(zxp) + len(zxn)
    entropyzx = entropy_1(len(zxp), len(zxn), totalzx)
    totalzb = len(zbp) + len(zbn)
    entropyzb = entropy_1(len(zbp), len(zbn), totalzb)
    totalzo = len(zop) + len(zon)
    entropyzo = entropy_1(len(zop), len(zon), totalzo)
    totalzxo = totalzx + totalzb + totalzo    
    entropyzxo = entropy_2(entropyzx, entropyzb, entropyzo, totalzxo, totalzx, totalzb, totalzo)
    totalzp = len(zxp) + len(zbp) + len(zop)
    totalzn = len(zxn) + len(zbn) + len(zon)
    totalz = totalzp + totalzn
    entropyz = entropy_1(totalzp, totalzn, totalz)
    gainz = entropyz - entropyzxo
    return(gainz, '   ', len(zxp), len(zxn), len(zbp), len(zbn), len(zop), len(zon))
for item in range(0, 9):    
    print(attr_32(item))
print('Best Node------A5------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is x and A2 is x and A1 is o
#--------------------------------------------------------------------

def attr_33(j):
    xp1 = []
    xn1 = []
    bp1 = []
    bn1 = []
    op1 = []
    on1 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
           is 'x' and TrainingSet[i][1] is 'o' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp1.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn1.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'o' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp1.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn1.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'o' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op1.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'x'  and TrainingSet[i][2]\
             is 'x' and TrainingSet[i][1] is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on1.append(TrainingSet[i])
        i += 1
    totalx1 = len(xp1) + len(xn1)
    entropyx1 = entropy_1(len(xp1), len(xn1), totalx1)
    totalb1 = len(bp1) + len(bn1)
    entropyb1 = entropy_1(len(bp1), len(bn1), totalb1)
    totalo1 = len(op1) + len(on1)
    entropyo1 = entropy_1(len(op1), len(on1), totalo1)
    totalxo1 = totalx1 + totalb1 + totalo1    
    entropyxo1 = entropy_2(entropyx1, entropyb1, entropyo1, totalxo1, totalx1, totalb1, totalo1)
    totalp1 = len(xp1) + len(bp1) + len(op1)
    totaln1 = len(xn1) + len(bn1) + len(on1)
    total1 = totalp1 + totaln1
    entropy1 = entropy_1(totalp1, totaln1, total1)
    gain1 = entropy1 - entropyxo1
    return(gain1, '   ', len(xp1), len(xn1), len(bp1), len(bn1), len(op1), len(on1))
for item in range(0, 9):    
    print(attr_33(item))
print('Best Node------A5------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is b and A7 is x and A5 is b
#--------------------------------------------------------------------

def attr_34(j):
    xp2 = []
    xn2 = []
    bp2 = []
    bn2 = []
    op2 = []
    on2 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
           is 'x' and TrainingSet[i][5] is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp2.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn2.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp2.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn2.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op2.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on2.append(TrainingSet[i])
        i += 1
    totalx2 = len(xp2) + len(xn2)
    entropyx2 = entropy_1(len(xp2), len(xn2), totalx2)
    totalb2 = len(bp2) + len(bn2)
    entropyb2 = entropy_1(len(bp2), len(bn2), totalb2)
    totalo2 = len(op2) + len(on2)
    entropyo2 = entropy_1(len(op2), len(on2), totalo2)
    totalxo2 = totalx2 + totalb2 + totalo2    
    entropyxo2 = entropy_2(entropyx2, entropyb2, entropyo2, totalxo2, totalx2, totalb2, totalo2)
    totalp2 = len(xp2) + len(bp2) + len(op2)
    totaln2 = len(xn2) + len(bn2) + len(on2)
    total2 = totalp2 + totaln2
    entropy2 = entropy_1(totalp2, totaln2, total2)
    gain2 = entropy2 - entropyxo2
    return(gain2, '   ', len(xp2), len(xn2), len(bp2), len(bn2), len(op2), len(on2))
for item in range(0, 9):    
    print(attr_34(item))
print('Best Node------A3------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is b and A7 is x and A5 is b
#--------------------------------------------------------------------

def attr_35(j):
    xp3 = []
    xn3 = []
    bp3 = []
    bn3 = []
    op3 = []
    on3 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
           is 'x' and TrainingSet[i][5] is 'o' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp3.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn3.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'o' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp3.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn3.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'o' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op3.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'x' and TrainingSet[i][5] is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on3.append(TrainingSet[i])
        i += 1
    totalx3 = len(xp3) + len(xn3)
    entropyx3 = entropy_1(len(xp3), len(xn3), totalx3)
    totalb3 = len(bp3) + len(bn3)
    entropyb3 = entropy_1(len(bp3), len(bn3), totalb3)
    totalo3 = len(op3) + len(on3)
    entropyo3 = entropy_1(len(op3), len(on3), totalo3)
    totalxo3 = totalx3 + totalb3 + totalo3    
    entropyxo3 = entropy_2(entropyx3, entropyb3, entropyo3, totalxo3, totalx3, totalb3, totalo3)
    totalp3 = len(xp3) + len(bp3) + len(op3)
    totaln3 = len(xn3) + len(bn3) + len(on3)
    total3 = totalp3 + totaln3
    entropy3 = entropy_1(totalp3, totaln3, total3)
    gain3 = entropy3 - entropyxo3
    return(gain3, '   ', len(xp3), len(xn3), len(bp3), len(bn3), len(op3), len(on3))
for item in range(0, 9):    
    print(attr_35(item))
print('Best Node------A3------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is b and A7 is b and A1 is x
#--------------------------------------------------------------------

def attr_36(j):
    xp4 = []
    xn4 = []
    bp4 = []
    bn4 = []
    op4 = []
    on4 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
           is 'b' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp4.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn4.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp4.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn4.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op4.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on4.append(TrainingSet[i])
        i += 1
    totalx4 = len(xp4) + len(xn4)
    entropyx4 = entropy_1(len(xp4), len(xn4), totalx4)
    totalb4 = len(bp4) + len(bn4)
    entropyb4 = entropy_1(len(bp4), len(bn4), totalb4)
    totalo4 = len(op4) + len(on4)
    entropyo4 = entropy_1(len(op4), len(on4), totalo4)
    totalxo4 = totalx4 + totalb4 + totalo4    
    entropyxo4 = entropy_2(entropyx4, entropyb4, entropyo4, totalxo4, totalx4, totalb4, totalo4)
    totalp4 = len(xp4) + len(bp4) + len(op4)
    totaln4 = len(xn4) + len(bn4) + len(on4)
    total4 = totalp4 + totaln4
    entropy4 = entropy_1(totalp4, totaln4, total4)
    gain4 = entropy4 - entropyxo4
    return(gain4, '   ', len(xp4), len(xn4), len(bp4), len(bn4), len(op4), len(on4))
for item in range(0, 9):    
    print(attr_36(item))
print('Best Node------A2------\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is b and A7 is b and A1 is b
#--------------------------------------------------------------------

def attr_37(j):
    xp5 = []
    xn5 = []
    bp5 = []
    bn5 = []
    op5 = []
    on5 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
           is 'b' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp5.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn5.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp5.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn5.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op5.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'b'  and TrainingSet[i][7]\
             is 'b' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on5.append(TrainingSet[i])
        i += 1
    totalx5 = len(xp5) + len(xn5)
    entropyx5 = entropy_1(len(xp5), len(xn5), totalx5)
    totalb5 = len(bp5) + len(bn5)
    entropyb5 = entropy_1(len(bp5), len(bn5), totalb5)
    totalo5 = len(op5) + len(on5)
    entropyo5 = entropy_1(len(op5), len(on5), totalo5)
    totalxo5 = totalx5 + totalb5 + totalo5
    entropyxo5 = entropy_2(entropyx5, entropyb5, entropyo5, totalxo5, totalx5, totalb5, totalo5)
    totalp5 = len(xp5) + len(bp5) + len(op5)
    totaln5 = len(xn5) + len(bn5) + len(on5)
    total5 = totalp5 + totaln5
    entropy5 = entropy_1(totalp5, totaln5, total5)
    gain5 = entropy5 - entropyxo5
    return(gain5, '   ', len(xp5), len(xn5), len(bp5), len(bn5), len(op5), len(on5))
for item in range(0, 9):    
    print(attr_37(item))
print('Best Node-------A3-----\n')

#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o and A7 is o and A1 is x
#--------------------------------------------------------------------

def attr_38(j):
    xp6 = []
    xn6 = []
    bp6 = []
    bn6 = []
    op6 = []
    on6 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
           is 'o' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp6.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn6.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp6.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn6.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op6.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on6.append(TrainingSet[i])
        i += 1
    totalx6 = len(xp6) + len(xn6)
    entropyx6 = entropy_1(len(xp6), len(xn6), totalx6)
    totalb6 = len(bp6) + len(bn6)
    entropyb6 = entropy_1(len(bp6), len(bn6), totalb6)
    totalo6 = len(op6) + len(on6)
    entropyo6 = entropy_1(len(op6), len(on6), totalo6)
    totalxo6 = totalx6 + totalb6 + totalo6
    entropyxo6 = entropy_2(entropyx6, entropyb6, entropyo6, totalxo6, totalx6, totalb6, totalo6)
    totalp6 = len(xp6) + len(bp6) + len(op6)
    totaln6 = len(xn6) + len(bn6) + len(on6)
    total6 = totalp6 + totaln6
    entropy6 = entropy_1(totalp6, totaln6, total6)
    gain6 = entropy6 - entropyxo6
    return(gain6, '   ', len(xp6), len(xn6), len(bp6), len(bn6), len(op6), len(on6))
for item in range(0, 9):    
    print(attr_38(item))
print('Best Node-------A6-----\n')
#----------------------------------------------------------------------
#Building the next level when A4 is o and A0 is x and A8 is o and A7 is o and A1 is b
#--------------------------------------------------------------------

def attr_39(j):
    xp7 = []
    xn7 = []
    bp7 = []
    bn7 = []
    op7 = []
    on7 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
           is 'o' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp7.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn7.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp7.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn7.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op7.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'o' and TrainingSet[i][0] is 'x' and TrainingSet[i][8] is 'o'  and TrainingSet[i][7]\
             is 'o' and TrainingSet[i][1] is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on7.append(TrainingSet[i])
        i += 1
    totalx7 = len(xp7) + len(xn7)
    entropyx7 = entropy_1(len(xp7), len(xn7), totalx7)
    totalb7 = len(bp7) + len(bn7)
    entropyb7 = entropy_1(len(bp7), len(bn7), totalb7)
    totalo7 = len(op7) + len(on7)
    entropyo7 = entropy_1(len(op7), len(on7), totalo7)
    totalxo7 = totalx7 + totalb7 + totalo7
    entropyxo7 = entropy_2(entropyx7, entropyb7, entropyo7, totalxo7, totalx7, totalb7, totalo7)
    totalp7 = len(xp7) + len(bp7) + len(op7)
    totaln7 = len(xn7) + len(bn7) + len(on7)
    total7 = totalp7 + totaln7
    entropy7 = entropy_1(totalp7, totaln7, total7)
    gain7 = entropy7 - entropyxo7
    return(gain7, '   ', len(xp7), len(xn7), len(bp7), len(bn7), len(op7), len(on7))
for item in range(0, 9):    
    print(attr_39(item))
print('Best Node-----A6-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is b and A5 is x and A3 is b
#--------------------------------------------------------------------

def attr_40(j):
    xp8 = []
    xn8 = []
    bp8 = []
    bn8 = []
    op8 = []
    on8 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
           is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp8.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn8.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp8.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn8.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op8.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on8.append(TrainingSet[i])
        i += 1
    totalx8 = len(xp8) + len(xn8)
    entropyx8 = entropy_1(len(xp8), len(xn8), totalx8)
    totalb8 = len(bp8) + len(bn8)
    entropyb8 = entropy_1(len(bp8), len(bn8), totalb8)
    totalo8 = len(op8) + len(on8)
    entropyo8 = entropy_1(len(op8), len(on8), totalo8)
    totalxo8 = totalx8 + totalb8 + totalo8
    entropyxo8 = entropy_2(entropyx8, entropyb8, entropyo8, totalxo8, totalx8, totalb8, totalo8)
    totalp8 = len(xp8) + len(bp8) + len(op8)
    totaln8 = len(xn8) + len(bn8) + len(on8)
    total8 = totalp8 + totaln8
    entropy8 = entropy_1(totalp8, totaln8, total8)
    gain8 = entropy8 - entropyxo8
    return(gain8, '   ', len(xp8), len(xn8), len(bp8), len(bn8), len(op8), len(on8))
for item in range(0, 9):    
    print(attr_40(item))
print('Best Node-----A1-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is b and A5 is x and A3 is o
#--------------------------------------------------------------------

def attr_41(j):
    xp9 = []
    xn9 = []
    bp9 = []
    bn9 = []
    op9 = []
    on9 = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
           is 'o' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xp9.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'o' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xn9.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'o' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bp9.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'o' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bn9.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'o' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                op9.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'b' and TrainingSet[i][5] is 'x'  and TrainingSet[i][3]\
             is 'o' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                on9.append(TrainingSet[i])
        i += 1
    totalx9 = len(xp9) + len(xn9)
    entropyx9 = entropy_1(len(xp9), len(xn9), totalx9)
    totalb9 = len(bp9) + len(bn9)
    entropyb9 = entropy_1(len(bp9), len(bn9), totalb9)
    totalo9 = len(op9) + len(on9)
    entropyo9 = entropy_1(len(op9), len(on9), totalo9)
    totalxo9 = totalx9 + totalb9 + totalo9
    entropyxo9 = entropy_2(entropyx9, entropyb9, entropyo9, totalxo9, totalx9, totalb9, totalo9)
    totalp9 = len(xp9) + len(bp9) + len(op9)
    totaln9 = len(xn9) + len(bn9) + len(on9)
    total9 = totalp9 + totaln9
    entropy9 = entropy_1(totalp9, totaln9, total9)
    gain9 = entropy9 - entropyxo9
    return(gain9, '   ', len(xp9), len(xn9), len(bp9), len(bn9), len(op9), len(on9))
for item in range(0, 9):    
    print(attr_41(item))
print('Best Node-----A6-------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is o and A0 is o and A1 is x
#--------------------------------------------------------------------

def attr_42(j):
    xpa = []
    xna = []
    bpa = []
    bna = []
    opa = []
    ona = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
           is 'x' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xpa.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'x' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xna.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'x' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bpa.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'x' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bna.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'x' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                opa.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'x' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                ona.append(TrainingSet[i])
        i += 1
    totalxa = len(xpa) + len(xna)
    entropyxa = entropy_1(len(xpa), len(xna), totalxa)
    totalba = len(bpa) + len(bna)
    entropyba = entropy_1(len(bpa), len(bna), totalba)
    totaloa = len(opa) + len(ona)
    entropyoa = entropy_1(len(opa), len(ona), totaloa)
    totalxoa = totalxa + totalba + totaloa
    entropyxoa = entropy_2(entropyxa, entropyba, entropyoa, totalxoa, totalxa, totalba, totaloa)
    totalpa = len(xpa) + len(bpa) + len(opa)
    totalna = len(xna) + len(bna) + len(ona)
    totala = totalpa + totalna
    entropya = entropy_1(totalpa, totalna, totala)
    gaina = entropya - entropyxoa
    return(gaina, '   ', len(xpa), len(xna), len(bpa), len(bna), len(opa), len(ona))
for item in range(0, 9):    
    print(attr_42(item))
print('Best Node------A6------\n')
#----------------------------------------------------------------------
#Building the next level when A4 is x and A2 is o and A0 is o and A1 is b
#--------------------------------------------------------------------

def attr_43(j):
    xpb = []
    xnb = []
    bpb = []
    bnb = []
    opb = []
    onb = []
    i = 0
    while(i < len(TrainingSet)):
        if TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
           is 'b' and TrainingSet[i][j] is 'x' and 'positive' in TrainingSet[i]:
                xpb.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'b' and TrainingSet[i][j] is 'x' and 'negative' in TrainingSet[i]:
                xnb.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'b' and TrainingSet[i][j] is 'b' and 'positive' in TrainingSet[i]:
                bpb.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'b' and TrainingSet[i][j] is 'b' and 'negative' in TrainingSet[i]:
                bnb.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'b' and TrainingSet[i][j] is 'o' and 'positive' in TrainingSet[i]:
                opb.append(TrainingSet[i])
        elif TrainingSet[i][4] is 'x' and TrainingSet[i][2] is 'o' and TrainingSet[i][0] is 'o'  and TrainingSet[i][1]\
             is 'b' and TrainingSet[i][j] is 'o' and 'negative' in TrainingSet[i]:
                onb.append(TrainingSet[i])
        i += 1
    totalxb = len(xpb) + len(xnb)
    entropyxb = entropy_1(len(xpb), len(xnb), totalxb)
    totalbb = len(bpb) + len(bnb)
    entropybb = entropy_1(len(bpb), len(bnb), totalbb)
    totalob = len(opb) + len(onb)
    entropyob = entropy_1(len(opb), len(onb), totalob)
    totalxob = totalxb + totalbb + totalob
    entropyxob = entropy_2(entropyxb, entropybb, entropyob, totalxob, totalxb, totalbb, totalob)
    totalpb = len(xpb) + len(bpb) + len(opb)
    totalnb = len(xnb) + len(bnb) + len(onb)
    totalb = totalpb + totalnb
    entropyb = entropy_1(totalpb, totalnb, totalb)
    gainb = entropyb - entropyxob
    return(gainb, '   ', len(xpb), len(xnb), len(bpb), len(bnb), len(opb), len(onb))
for item in range(0, 9):    
    print(attr_43(item))
print('Best Node------A7------\n')

#----------------------------------------------------------------------
#Applying the tree on provided test data to determine class labels
#----------------------------------------------------------------------

xpc = []
xnc = []
bpc = []
bnc = []
opc = []
onc = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
       is 'b' and TestSet[i][7] is 'x' and 'positive' in TestSet[i]:
        xpc.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][7] is 'x' and 'negative' in TestSet[i]:
        xnc.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][7] is 'b' and 'positive' in TestSet[i]:
        bpc.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][7] is 'b' and 'negative' in TestSet[i]:
        bnc.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][7] is 'o' and 'positive' in TestSet[i]:
        opc.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][7] is 'o' and 'negative' in TestSet[i]:
        onc.append(TestSet[i])
    i += 1
print(len(xpc), len(xnc), len(bpc), len(bnc), len(opc), len(onc))
#--------------------------------------------------------------------------
xpd = []
xnd = []
bpd = []
bnd = []
opd = []
ond = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
       is 'b' and TestSet[i][6] is 'x' and 'positive' in TestSet[i]:
        xpd.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][6] is 'x' and 'negative' in TestSet[i]:
        xnd.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][6] is 'b' and 'positive' in TestSet[i]:
        bpd.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][6] is 'b' and 'negative' in TestSet[i]:
        bnd.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][6] is 'o' and 'positive' in TestSet[i]:
        opd.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1]\
         is 'b' and TestSet[i][6] is 'o' and 'negative' in TestSet[i]:
        ond.append(TestSet[i])
    i += 1
print(len(xpd), len(xnd), len(bpd), len(bnd), len(opd), len(ond))
#-------------------------------------------------------------------------------------------

ope = []
one = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1] is 'o' and 'positive' in TestSet[i]:        
        ope.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'o'  and TestSet[i][1] is 'o' and 'negative' in TestSet[i]:
        one.append(TestSet[i])
    i += 1
print(len(ope), len(one))
#--------------------------------------------------------------------------
xpf = []
xnf = []
bpf = []
bnf = []
opf = []
onf = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'x'  and TestSet[i][8] is 'x' and 'positive' in TestSet[i]:
        xpf.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'x'  and TestSet[i][8] is 'x' and 'negative' in TestSet[i]:
        xnf.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'x'  and TestSet[i][8] is 'b' and 'positive' in TestSet[i]:
        bpf.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'x'  and TestSet[i][8] is 'b' and 'negative' in TestSet[i]:
        bnf.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'x'  and TestSet[i][8] is 'o' and 'positive' in TestSet[i]:
        opf.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'o' and TestSet[i][0] is 'x'  and TestSet[i][8] is 'o' and 'negative' in TestSet[i]:
        onf.append(TestSet[i])
    i += 1
print(len(xpf), len(xnf), len(bpf), len(bnf), len(opf), len(onf))
#-------------------------------------------------------------------------------------------
xpg = []
xng = []
bpg = []
bng = []
opg = []
ong = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'o'  and TestSet[i][8] is 'x' and 'positive' in TestSet[i]:
        xpg.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'o'  and TestSet[i][8] is 'x' and 'negative' in TestSet[i]:
        xng.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'o'  and TestSet[i][8] is 'b' and 'positive' in TestSet[i]:
        bpg.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'o'  and TestSet[i][8] is 'b' and 'negative' in TestSet[i]:
        bng.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'o'  and TestSet[i][8] is 'o' and 'positive' in TestSet[i]:
        opg.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'o'  and TestSet[i][8] is 'o' and 'negative' in TestSet[i]:
        ong.append(TestSet[i])
    i += 1
print(len(xpg), len(xng), len(bpg), len(bng), len(opg), len(ong))
#-------------------------------------------------------------------------------------------
xph = []
xnh = []
bph = []
bnh = []
oph = []
onh = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'b'  and TestSet[i][6] is 'x' and 'positive' in TestSet[i]:
        xph.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'b'  and TestSet[i][6] is 'x' and 'negative' in TestSet[i]:
        xnh.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'b'  and TestSet[i][6] is 'b' and 'positive' in TestSet[i]:
        bph.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'b'  and TestSet[i][6] is 'b' and 'negative' in TestSet[i]:
        bnh.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'b'  and TestSet[i][6] is 'o' and 'positive' in TestSet[i]:
        oph.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'b'  and TestSet[i][6] is 'o' and 'negative' in TestSet[i]:
        onh.append(TestSet[i])
    i += 1
print(len(xph), len(xnh), len(bph), len(bnh), len(oph), len(onh))
#-------------------------------------------------------------------------------------------
bpi = []
bni = []
opi = []
oni = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'o' and TestSet[i][6] is 'b' and\
       'positive' in TestSet[i]:
        bpi.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'o' and TestSet[i][6] is 'b' and\
         'negative' in TestSet[i]:
        bni.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'o' and TestSet[i][6] is 'o' and\
         'positive' in TestSet[i]:
        opi.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'o' and TestSet[i][6] is 'o' and\
         'negative' in TestSet[i]:
        oni.append(TestSet[i])
    i += 1
print(len(bpi), len(bni), len(opi), len(oni))
#-------------------------------------------------------------------------------------------
xpj = []
xnj = []
bpj = []
bnj = []
opj = []
onj = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'b' and TestSet[i][1] is 'x' and\
       'positive' in TestSet[i]:
        xpj.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'b' and TestSet[i][1] is 'x' and\
         'negative' in TestSet[i]:
        xnj.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'b' and TestSet[i][1] is 'b' and\
         'positive' in TestSet[i]:
        bpj.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'b' and TestSet[i][1] is 'b' and\
         'negative' in TestSet[i]:
        bnj.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'b' and TestSet[i][1] is 'o' and\
         'positive' in TestSet[i]:
        opj.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x' and TestSet[i][3] is 'b' and TestSet[i][1] is 'o' and\
         'negative' in TestSet[i]:
        onj.append(TestSet[i])
    i += 1
print(len(xpj), len(xnj), len(bpj), len(bnj), len(opj), len(onj))
#-------------------------------------------------------------------------------------------
opk = []
onk = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x'  and TestSet[i][3] is 'x' and 'positive' in TestSet[i]:        
        opk.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'b' and TestSet[i][5] is 'x'  and TestSet[i][3] is 'x' and 'negative' in TestSet[i]:
        onk.append(TestSet[i])
    i += 1
print(len(opk), len(onk))
#--------------------------------------------------------------------------
xpl = []
xnl = []
bpl = []
bnl = []
opl = []
onl = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'x' and TestSet[i][2] is 'x' and TestSet[i][6] is 'x' and 'positive' in TestSet[i]:
        xpl.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'x' and TestSet[i][6] is 'x' and 'negative' in TestSet[i]:
        xnl.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'x' and TestSet[i][6] is 'b' and 'positive' in TestSet[i]:
        bpl.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'x' and TestSet[i][6] is 'b' and 'negative' in TestSet[i]:
        bnl.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'x' and TestSet[i][6] is 'o' and 'positive' in TestSet[i]:
        opl.append(TestSet[i])
    elif TestSet[i][4] is 'x' and TestSet[i][2] is 'x' and TestSet[i][6] is 'o' and 'negative' in TestSet[i]:
        onl.append(TestSet[i])
    i += 1
print(len(xpl), len(xnl), len(bpl), len(bnl), len(opl), len(onl))
#-------------------------------------------------------------------------------------------
xpm = []
xnm = []
bpm = []
bnm = []
opm = []
onm = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'b' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and 'positive' in TestSet[i]:
        xpm.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and 'negative' in TestSet[i]:
        xnm.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b' and 'positive' in TestSet[i]:
        bpm.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b' and 'negative' in TestSet[i]:
        bnm.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o' and 'positive' in TestSet[i]:
        opm.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o' and 'negative' in TestSet[i]:
        onm.append(TestSet[i])
    i += 1
print(len(xpm), len(xnm), len(bpm), len(bnm), len(opm), len(onm))
#-------------------------------------------------------------------------------------------
xpn = []
xnn = []
bpn = []
bnn = []
opn = []
onn = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'b' and TestSet[i][0] is 'o' and TestSet[i][8] is 'x' and 'positive' in TestSet[i]:
        xpn.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'o' and TestSet[i][8] is 'x' and 'negative' in TestSet[i]:
        xnn.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'o' and TestSet[i][8] is 'b' and 'positive' in TestSet[i]:
        bpn.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'o' and TestSet[i][8] is 'b' and 'negative' in TestSet[i]:
        bnn.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'o' and TestSet[i][8] is 'o' and 'positive' in TestSet[i]:
        opn.append(TestSet[i])
    elif TestSet[i][4] is 'b' and TestSet[i][0] is 'o' and TestSet[i][8] is 'o' and 'negative' in TestSet[i]:
        onn.append(TestSet[i])
    i += 1
print(len(xpn), len(xnn), len(bpn), len(bnn), len(opn), len(onn))
#-------------------------------------------------------------------------------------------
xpo = []
xno = []
bpo = []
bno = []
opo = []
ono = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'o' and TestSet[i][8] is 'x' and 'positive' in TestSet[i]:
        xpo.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'o' and TestSet[i][8] is 'x' and 'negative' in TestSet[i]:
        xno.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'o' and TestSet[i][8] is 'b' and 'positive' in TestSet[i]:
        bpo.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'o' and TestSet[i][8] is 'b' and 'negative' in TestSet[i]:
        bno.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'o' and TestSet[i][8] is 'o' and 'positive' in TestSet[i]:
        opo.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'o' and TestSet[i][8] is 'o' and 'negative' in TestSet[i]:
        ono.append(TestSet[i])
    i += 1
print(len(xpo), len(xno), len(bpo), len(bno), len(opo), len(ono))
#-------------------------------------------------------------------------------------------
xpp = []
xnp = []
bpp = []
bnp = []
opp = []
onp = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
       is 'o' and TestSet[i][1] is 'x' and TestSet[i][6] is 'x' and 'positive' in TestSet[i]:
        xpp.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'x' and TestSet[i][6] is 'x' and 'negative' in TestSet[i]:
        xnp.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'x' and TestSet[i][6] is 'b' and 'positive' in TestSet[i]:
        bpp.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'x' and TestSet[i][6] is 'b' and 'negative' in TestSet[i]:
        bnp.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'x' and TestSet[i][6] is 'o' and 'positive' in TestSet[i]:
        opp.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'x' and TestSet[i][6] is 'o' and 'negative' in TestSet[i]:
        onp.append(TestSet[i])
    i += 1
print(len(xpp), len(xnp), len(bpp), len(bnp), len(opp), len(onp))
#-------------------------------------------------------------------------------------------
xpq = []
xnq = []
bpq = []
bnq = []
opq = []
onq = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
       is 'o' and TestSet[i][1] is 'b' and TestSet[i][6] is 'x' and 'positive' in TestSet[i]:
        xpq.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'b' and TestSet[i][6] is 'x' and 'negative' in TestSet[i]:
        xnq.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'b' and TestSet[i][6] is 'o' and 'positive' in TestSet[i]:
        bpq.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'b' and TestSet[i][6] is 'o' and 'negative' in TestSet[i]:
        bnq.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'o' and 'positive' in TestSet[i]:
        opq.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'o' and 'negative' in TestSet[i]:
        onq.append(TestSet[i])
    i += 1
print(len(xpq), len(xnq), len(bpq), len(bnq), len(opq), len(onq))
#-------------------------------------------------------------------------------------------
xpr = []
xnr = []
bpr = []
bnr = []
opr = []
onr = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o' and TestSet[i][7]\
       is 'b' and TestSet[i][2] is 'x' and 'positive' in TestSet[i]:
        xpr.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'b' and TestSet[i][2] is 'x' and 'negative' in TestSet[i]:
        xnr.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'b' and TestSet[i][2] is 'b' and 'positive' in TestSet[i]:
        bpr.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'b' and TestSet[i][2] is 'b' and 'negative' in TestSet[i]:
        bnr.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'b' and TestSet[i][2] is 'o' and 'positive' in TestSet[i]:
        opr.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'b' and TestSet[i][2] is 'o' and 'negative' in TestSet[i]:
        onr.append(TestSet[i])
    i += 1
print(len(xpr), len(xnr), len(bpr), len(bnr), len(opr), len(onr))
#-------------------------------------------------------------------------------------------
xps = []
xns = []
bps = []
bns = []
ops = []
ons = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
       is 'x' and TestSet[i][2] is 'o' and TestSet[i][1] is 'x' and 'positive' in TestSet[i]:
        xps.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'o' and TestSet[i][1] is 'x' and 'negative' in TestSet[i]:
        xns.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'o' and TestSet[i][1] is 'b' and 'positive' in TestSet[i]:
        bps.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'o' and TestSet[i][1] is 'b' and 'negative' in TestSet[i]:
        bns.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'o' and TestSet[i][1] is 'o' and 'positive' in TestSet[i]:
        ops.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'o' and TestSet[i][1] is 'o' and 'negative' in TestSet[i]:
        ons.append(TestSet[i])
    i += 1
print(len(xps), len(xns), len(bps), len(bns), len(ops), len(ons))
#-------------------------------------------------------------------------------------------
xpt = []
xnt = []
bpt = []
bnt = []
opt = []
ont = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
       is 'x' and TestSet[i][2] is 'b' and TestSet[i][1] is 'x' and 'positive' in TestSet[i]:
        xpt.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'b' and TestSet[i][1] is 'x' and 'negative' in TestSet[i]:
        xnt.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'b' and TestSet[i][1] is 'b' and 'positive' in TestSet[i]:
        bpt.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'b' and TestSet[i][1] is 'b' and 'negative' in TestSet[i]:
        bnt.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'b' and TestSet[i][1] is 'o' and 'positive' in TestSet[i]:
        opt.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'b' and TestSet[i][1] is 'o' and 'negative' in TestSet[i]:
        ont.append(TestSet[i])
    i += 1
print(len(xpt), len(xnt), len(bpt), len(bnt), len(opt), len(ont))
#-------------------------------------------------------------------------------------------
xpu = []
xnu = []
bpu = []
bnu = []
opu = []
onu = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
       is 'x' and TestSet[i][2] is 'x' and TestSet[i][1] is 'x' and 'positive' in TestSet[i]:
        xpu.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'x' and TestSet[i][1] is 'x' and 'negative' in TestSet[i]:
        xnu.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'x' and TestSet[i][1] is 'b' and 'positive' in TestSet[i]:
        bpu.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'x' and TestSet[i][1] is 'b' and 'negative' in TestSet[i]:
        bnu.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'x' and TestSet[i][1] is 'o' and 'positive' in TestSet[i]:
        opu.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'o'  and TestSet[i][7]\
         is 'x' and TestSet[i][2] is 'x' and TestSet[i][1] is 'o' and 'negative' in TestSet[i]:
        onu.append(TestSet[i])
    i += 1
print(len(xpu), len(xnu), len(bpu), len(bnu), len(opu), len(onu))
#-------------------------------------------------------------------------------------------
xpv = []
xnv = []
bpv = []
bnv = []
opv = []
onv = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
       is 'o' and TestSet[i][1] is 'x' and 'positive' in TestSet[i]:
        xpv.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'x' and 'negative' in TestSet[i]:
        xnv.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'b' and 'positive' in TestSet[i]:
        bpv.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'b' and 'negative' in TestSet[i]:
        bnv.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'o' and 'positive' in TestSet[i]:
        opv.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'o' and TestSet[i][1] is 'o' and 'negative' in TestSet[i]:
        onv.append(TestSet[i])
    i += 1
print(len(xpv), len(xnv), len(bpv), len(bnv), len(opv), len(onv))
#-------------------------------------------------------------------------------------------
xpw = []
xnw = []
bpw = []
bnw = []
opw = []
onw = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
       is 'b' and TestSet[i][1] is 'x' and TestSet[i][2] is 'x' and 'positive' in TestSet[i]:
        xpw.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'x' and TestSet[i][2] is 'x' and 'negative' in TestSet[i]:
        xnw.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'x' and TestSet[i][2] is 'b' and 'positive' in TestSet[i]:
        bpw.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'x' and TestSet[i][2] is 'b' and 'negative' in TestSet[i]:
        bnw.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'x' and TestSet[i][2] is 'o' and 'positive' in TestSet[i]:
        opw.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'x' and TestSet[i][2] is 'o' and 'negative' in TestSet[i]:
        onw.append(TestSet[i])
    i += 1
print(len(xpw), len(xnw), len(bpw), len(bnw), len(opw), len(onw))
#-------------------------------------------------------------------------------------------
xpx = []
xnx = []
bpx = []
bnx = []
opx = []
onx = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
       is 'b' and TestSet[i][1] is 'b' and TestSet[i][3] is 'x' and 'positive' in TestSet[i]:
        xpx.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'b' and TestSet[i][3] is 'x' and 'negative' in TestSet[i]:
        xnx.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'b' and TestSet[i][3] is 'o' and 'positive' in TestSet[i]:
        bpx.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'b' and TestSet[i][3] is 'o' and 'negative' in TestSet[i]:
        bnx.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'o' and 'positive' in TestSet[i]:
        opx.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'b' and TestSet[i][1] is 'o' and 'negative' in TestSet[i]:
        onx.append(TestSet[i])
    i += 1
print(len(xpx), len(xnx), len(bpx), len(bnx), len(opx), len(onx))
#-------------------------------------------------------------------------------------------
xpy = []
xny = []
bpy = []
bny = []
opy = []
ony = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
       is 'x' and TestSet[i][5] is 'o' and TestSet[i][3] is 'x' and 'positive' in TestSet[i]:
        xpy.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'o' and TestSet[i][3] is 'x' and 'negative' in TestSet[i]:
        xny.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'o' and TestSet[i][3] is 'b' and 'positive' in TestSet[i]:
        bpy.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'o' and TestSet[i][3] is 'b' and 'negative' in TestSet[i]:
        bny.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'o' and TestSet[i][3] is 'o' and 'positive' in TestSet[i]:
        opy.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'o' and TestSet[i][3] is 'o' and 'negative' in TestSet[i]:
        ony.append(TestSet[i])
    i += 1
print(len(xpy), len(xny), len(bpy), len(bny), len(opy), len(ony))
#-------------------------------------------------------------------------------------------
xpz = []
xnz = []
bpz = []
bnz = []
opz = []
onz = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
       is 'x' and TestSet[i][5] is 'b' and TestSet[i][3] is 'x' and 'positive' in TestSet[i]:
        xpz.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'b' and TestSet[i][3] is 'x' and 'negative' in TestSet[i]:
        xnz.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'b' and TestSet[i][3] is 'b' and 'positive' in TestSet[i]:
        bpz.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'b' and TestSet[i][3] is 'b' and 'negative' in TestSet[i]:
        bnz.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'b' and TestSet[i][3] is 'o' and 'positive' in TestSet[i]:
        opz.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'b' and TestSet[i][3] is 'o' and 'negative' in TestSet[i]:
        onz.append(TestSet[i])
    i += 1
print(len(xpz), len(xnz), len(bpz), len(bnz), len(opz), len(onz))
#-------------------------------------------------------------------------------------------
x1p = []
x1n = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
       is 'x' and TestSet[i][5] is 'x' and 'positive' in TestSet[i]:
        xpz.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'b'  and TestSet[i][7]\
         is 'x' and TestSet[i][5] is 'x' and 'negative' in TestSet[i]:
        xnz.append(TestSet[i])
    i += 1
print(len(x1p), len(x1n))
#-------------------------------------------------------------------------------------------
x2p = []
x2n = []
b2p = []
b2n = []
o2p = []
o2n = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x'  and TestSet[i][2]\
       is 'o' and TestSet[i][6] is 'x' and 'positive' in TestSet[i]:
        x2p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x'  and TestSet[i][2]\
         is 'o' and TestSet[i][6] is 'x' and 'negative' in TestSet[i]:
        x2n.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x'  and TestSet[i][2]\
         is 'o' and TestSet[i][6] is 'b' and 'positive' in TestSet[i]:
        b2p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x'  and TestSet[i][2]\
         is 'o' and TestSet[i][6] is 'b' and 'negative' in TestSet[i]:
        b2n.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x'  and TestSet[i][2]\
         is 'o' and TestSet[i][6] is 'o' and 'positive' in TestSet[i]:
        o2p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x'  and TestSet[i][2]\
         is 'o' and TestSet[i][6] is 'o' and 'negative' in TestSet[i]:
        o2n.append(TestSet[i])
    i += 1
print(len(x2p), len(x2n), len(b2p), len(b2n), len(o2p), len(o2n))
#-------------------------------------------------------------------------------------------        
x3p = []
x3n = []
b3p = []
b3n = []
o3p = []
o3n = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
       is 'b' and TestSet[i][6] is 'x' and 'positive' in TestSet[i]:
        x3p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'b' and TestSet[i][6] is 'x' and 'negative' in TestSet[i]:
        x3n.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'b' and TestSet[i][6] is 'b' and 'positive' in TestSet[i]:
        b3p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'b' and TestSet[i][6] is 'b' and 'negative' in TestSet[i]:
        b3n.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'b' and TestSet[i][6] is 'o' and 'positive' in TestSet[i]:
        o3p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'b' and TestSet[i][6] is 'o' and 'negative' in TestSet[i]:
        o3n.append(TestSet[i])
    i += 1
print(len(x3p), len(x3n), len(b3p), len(b3n), len(o3p), len(o3n))
#-------------------------------------------------------------------------------------------

x4p = []
x4n = []
b4p = []
b4n = []
o4p = []
o4n = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
       is 'x' and TestSet[i][1] is 'o' and TestSet[i][5] is 'x' and 'positive' in TestSet[i]:
        x4p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'o' and TestSet[i][5] is 'x' and 'negative' in TestSet[i]:
        x4n.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'o' and TestSet[i][5] is 'b' and 'positive' in TestSet[i]:
        b4p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'o' and TestSet[i][5] is 'b' and 'negative' in TestSet[i]:
        b4n.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'o' and TestSet[i][5] is 'o' and 'positive' in TestSet[i]:
        o4p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'o' and TestSet[i][5] is 'o' and 'negative' in TestSet[i]:
        o4n.append(TestSet[i])
    i += 1
print(len(x4p), len(x4n), len(b4p), len(b4n), len(o4p), len(o4n))
#-------------------------------------------------------------------------------------------
x5p = []
x5n = []
b5p = []
b5n = []
o5p = []
o5n = []
i = 0
while(i < len(TestSet)):
    if TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
       is 'x' and TestSet[i][1] is 'b' and TestSet[i][5] is 'x' and 'positive' in TestSet[i]:
        x5p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'b' and TestSet[i][5] is 'x' and 'negative' in TestSet[i]:
        x5n.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'b' and TestSet[i][5] is 'o' and 'positive' in TestSet[i]:
        b5p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'b' and TestSet[i][5] is 'o' and 'negative' in TestSet[i]:
        b5n.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'x' and 'positive' in TestSet[i]:
        o5p.append(TestSet[i])
    elif TestSet[i][4] is 'o' and TestSet[i][0] is 'x' and TestSet[i][8] is 'x' and TestSet[i][2]\
         is 'x' and TestSet[i][1] is 'x' and 'negative' in TestSet[i]:
        o5n.append(TestSet[i])
    i += 1
print(len(x5p), len(x5n), len(b5p), len(b5n), len(o5p), len(o5n))

#-------------------------------------------------------------------------------------------
#-------------------------------------------------------------------------------------------
#Calculating the Generalization Errors
#--------------------------------------------------------------------------------------------
#For the training error
#Because I kept track of the number of positives and negatives, so I just extracted
#the minimum numbers of the unclassified class from the output and added them
E = 21 + 12 + 13 + 8 + 4 + 3 + 1 + 6 + 3 + 3 + 1 + 1 + 1 + 1 + 2 + 2 + 2
opterr = E/len(TrainingSet)
#For the test error
Generr = 28/len(TestSet)
print('------------------------------------------')
print('-----GENERALIZATION ERRORS---------')
print('ERROR RATE ON TRAINING DATA :', opterr)
print('ERROR RATE ON TEST DATA :', Generr)

#--------------------------------------------------------------------------------------------
#Pruning the tree
print('\n-----------Pruning-----------')
#In pruning the tree, I actually removed some of the nodes of the unclassified
#class.
#------------------------------------------------------------------------------------------
#removing node A3 when A4 is o and A0 is x and A8 is b and A7 is x and A5 is o
E1 = 21 + 12 + 13 + 8 + 4 + 3 + 1 + 6 + 3 + 3 + 1 + 1 + 1 + 2 + 2 + 2
opterr1 = E1/len(TrainingSet)
print(opterr1)
#removing node A3 when A4 is o and A0 is x and A8 is b and A7 is x and A5 is b
E2 = 21 + 12 + 13 + 8 + 4 + 3 + 1 + 6 + 3 + 3 + 1 + 2 + 2 + 2
opterr2 = E2/len(TrainingSet)
print(opterr2)
#removing node A5 when A4 is o and A0 is x and A8 is x and A2 is x and A1 is o
E3 = 21 + 12 + 13 + 8 + 4 + 3 + 1 + 6 + 3 + 3 + 1 + 2 + 2 + 2
opterr3 = E3/len(TrainingSet)
print(opterr3)
#removing node A2 when A4 is o and A0 is x and A8 is b and A7 is b and A1 is x
E4 = 21 + 12 + 13 + 8 + 4 + 3 + 6 + 3 + 3 + 1 + 2 + 2 + 2
opterr4 = E4/len(TrainingSet)
print(opterr4)
#removing node A1 when A4 is o and A0 is x and A8 is o and A7 is x and A2 is o
E5 = 21 + 12 + 13 + 8 + 4 + 3 + 6 + 3 + 3 + 2 + 2 + 2
opterr5 = E5/len(TrainingSet)
print(opterr5)
print('-----------GENERALIZATION ERRORS--------')
print('ERROR RATE ON TRAINING DATA:', opterr5)
Generr = 20/len(TestSet)
print('ERROR RATE ON TEST DATA:', Generr)


