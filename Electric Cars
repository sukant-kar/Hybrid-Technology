##################################################
## Importing library #############################
##################################################
import os
import psspy
import csv
# -------------------------------------------------------------------
##################################################
## Defining Functions ############################
##################################################
def array2dict(dict_keys, dict_values):
 '''Convert array to dictionary of arrays.
 Returns dictionary as {dict_keys:dict_values}
 '''
 tmpdict = {}
 for i in range(len(dict_keys)):
 tmpdict[dict_keys[i].lower()] = dict_values[i]
 return tmpdict
# -------------------------------------------------------------------
def busindexes(busnum, busnumlist):
 '''Find indexes of a bus in list of buses.
 Returns list with indexes of 'busnum' in 'busnumlist'.
 '''
 busidxes = []
 startidx = 0
 buscounts = busnumlist.count(busnum)
 if buscounts:
 for i in range(buscounts):
 tmpidx = busnumlist.index(busnum,startidx)
 busidxes.append(tmpidx)
 startidx = tmpidx+1
 return busidxes
# -------------------------------------------------------------------
def splitstring_commaspace(tmpstr):
 '''Split string first at comma and then by space. Example:
 Input tmpstr = a1 a2, ,a4 a5 ,,,a8,a9
 Output strlst = ['a1', 'a2', ' ', 'a4', 'a5', ' ', ' ', 'a8',
'a9']
 '''
 strlst = []
 commalst = tmpstr.split(',')
 for each in commalst:
 eachlst = each.split()
 if eachlst:
 strlst.extend(eachlst)
 else:
 strlst.extend(' ')
 return strlst
#--------------------------------------------------------------------
def readloaddata(loadcsvfile):
 j=0 loaddict = {}
 readcsv = csv.reader(open(loadcsvfile))
 for row in readcsv:
 i=0
 for string in row:
 if j <= 1:
 row[i] = int(float(row[i]))
 else:
 row[i] = float(row[i])
 if EOFError:
 i = i + 1
 loaddict[j] = row
 j = j+1
 return loaddict
##################################################
## Get the integer and real data##################
##################################################
ierr1, number_of_loads = psspy.aloadcount(-1, 1) # only inservice loads including those at type 4 buses
ierr2, number_of_loadbuses = psspy.alodbuscount(-1, 2) # only inservice load buses including those with only out-of-service loads
ierr3, load_id = psspy.aloadchar ( -1, 2, ['ID'] )
ierr4, NAZOS = psspy.aloadint (-1, 2, ['NUMBER', 'AREA' , 'ZONE' ,
'OWNER' , 'STATUS' ] )
ierr6, lbv = psspy.alodbusreal (-1,4 , ['KV'])
NAZOS = array2dict(['NUMBER' , 'AREA' , 'ZONE' , 'OWNER' , 'STATUS'
], NAZOS)
load_data = NAZOS
load_data['ID'] = load_id [0]
##################################################
## Remove repeated load bus data #################
##################################################
p=range(len(load_data['number']))
m=[]
for h in p:
 for d in p:
 if load_data['number'][h] == load_data['number'][d] and d >
h:
 n = 0
 for s in range(len(m)):
 if m[s] == d:
 n = n + 1
 if n == 0:
 m.append(d)
m.sort()
g=0
for r in range(len(m)):
 del load_data['number'][m[r]-g]
 del load_data['area'][m[r]-g]
 del load_data['zone'][m[r]-g]
 del load_data['owner'][m[r]-g]
 del load_data['status'][m[r]-g]
 del load_data['ID'][m[r]-g]
 g = g + 1
##################################################
## Write Output Prompt ###########################
##################################################
psspy.prompt(' Please enter a CVS file wit hthe following format: \n\
 \n\- The CSV file can be saved in the same directory as
Python file in advance or just type a new file name')
psspy.prompt("\n\
 - The file name has to be typed in the following space in
this format: \n\
 \n\
*********************'FILENAME.csv'**************************")
# -------------------------------------------------------------------
##################################################
## Get load data from a CSV file #################
##################################################
hourload = readloaddata('load.csv')
# -------------------------------------------------------------------
##################################################
## Changing the load in the system ###############
##################################################
busvoltage={}
for k in range(24):
 k = k + 2
 for i in range(len(hourload[0])):
 busnumber = hourload[0][i]
 busid = str(hourload [1][i])
 for j in range(len(load_data ['ID'])):
 busid_PSSE = load_data ['ID'][j]
 if busid_PSSE[1] == ' ' :
 busid_PSSE = busid_PSSE[0]
 if busid == busid_PSSE and busnumber == load_data
['number'][j]:
 ierr5 =
psspy.load_data(load_data['number'][j],load_data['ID'][j],[load_data[
'status'][j] ,load_data['area'][j] ,load_data['zone'][j],

load_data['owner'][j]],[hourload[k][i],0 , 0 ,0 ,0,0])
 if i <= 19:
 reactive = hourload[k][i+1]
 ierr5 =
psspy.load_data(load_data['number'][j],load_data['ID'][j],[load_data[
'status'][j] ,load_data['area'][j] ,load_data['zone'][j],
load_data['owner'][j]],[hourload[k][i],reactive,0,0,0])
# -------------------------------------------------------------------
##################################################
## Run the power flow agai for new load ##########
##################################################
## if k == 80:
## ErrLF = psspy.fnsl([1,0,0,1,0,1,0,0])
## else:
 ErrLF = psspy.fdns([0,0,0,0,0,0,0,0]) #
1.Tap:disable///2.AreaExchange:disable///2.PhaseShif:disable///4.dcTa
p:disable///5.ShuntAdj:disable///#
6.FlatStart:enable///7.ApplyVarL:on interations///8.nondivergent:disable
# -------------------------------------------------------------------
##################################################
## Get the new voltage magnitude for new load ####
##################################################
 ierr, rval = psspy.alodbusreal(-1 ,2 ,['KV'])
 busvoltage [k-2] = rval [0]
##-------------------------------------------------------------------##################################################
## Send the voltage to the provided csv file #####
##################################################
rvaldimension = len(rval[0])
csvfile = input('Please one .csv file')
if csvfile: # open CSV file to write
 csvfile_h = open(csvfile,'w')
 report = csvfile_h.write
else: # send results to PSS/E
report window
 psspy.beginreport()
 report = psspy.report
for i in range(len(busvoltage[1])):
 busn = load_data['number'][i]
 report("%(busn)6d," %vars())
 for j in range(len(busvoltage)):
 busvoltag = busvoltage[j][i]
 report("%(busvoltag)3.4F," %vars())
 report("\n ")
# -------------------------------------------------------------------
##################################################
## Close The CSV file ############################
##################################################
if csvfile:
 csvfile_h.close()
 print '\n Done ..... Power Flow Results Report saved to file %s'
% csvfile
else:
 print '\n Done ..... Power Flow Results Report created in Report
window.'
Python code for losses report
##################################################
## Importing library #############################
##################################################
import os
import psspy
import csv
##################################################
## Defining Functions ############################
##################################################
def array2dict(dict_keys, dict_values):
 '''Convert array to dictionary of arrays.
 Returns dictionary as {dict_keys:dict_values}
 '''
 tmpdict = {}
 for i in range(len(dict_keys)):
 tmpdict[dict_keys[i].lower()] = dict_values[i]
 return tmpdict
def busindexes(busnum, busnumlist):
 '''Find indexes of a bus in list of buses.
 Returns list with indexes of 'busnum' in 'busnumlist'.
 '''
 busidxes = []
 startidx = 0
 buscounts = busnumlist.count(busnum)
 if buscounts:
 for i in range(buscounts):tmpidx = busnumlist.index(busnum,startidx)
 busidxes.append(tmpidx)
 startidx = tmpidx+1
 return busidxes
# -------------------------------------------------------------------
---------------------------------------------------------------------
------------------
def splitstring_commaspace(tmpstr):
 '''Split string first at comma and then by space. Example:
 Input tmpstr = a1 a2, ,a4 a5 ,,,a8,a9
 Output strlst = ['a1', 'a2', ' ', 'a4', 'a5', ' ', ' ', 'a8',
'a9']
 '''
 strlst = []
 commalst = tmpstr.split(',')
 for each in commalst:
 eachlst = each.split()
 if eachlst:
 strlst.extend(eachlst)
 else:
 strlst.extend(' ')
 return strlst
#--------------------------------------------------------------------
---------------------------------------------------------------------
-----------------
def readloaddata(loadcsvfile):
 j=0
 loaddict = {}
 readcsv = csv.reader(open(loadcsvfile))
 for row in readcsv:
 i=0
 for string in row:
 if j <= 1:
 row[i] = int(float(row[i]))
 else:
 row[i] = float(row[i])
 if EOFError:
 i = i + 1
 loaddict[j] = row
 j = j+1
 return loaddict
#--------------------------------------------------------------------
---------------------------------------------------------------------
-----------------
##################################################
## Get the integer and real data##################
##################################################
ierr1, number_of_loads = psspy.aloadcount(-1, 1) # only inservice loads including those at type 4 buses
ierr2, number_of_loadbuses = psspy.alodbuscount(-1, 2) # only inservice load buses including those with only out-of-service loads
ierr3, load_id = psspy.aloadchar ( -1, 2, ['ID'] )
ierr4, NAZOS = psspy.aloadint (-1, 2, ['NUMBER', 'AREA' , 'ZONE' ,
'OWNER' , 'STATUS' ] )
ierr6, lbv = psspy.alodbusreal (-1,4 , ['KV'])
ierr9, area = psspy.aloadint (-1, 2, ['AREA' ] )
NAZOS = array2dict(['NUMBER' , 'AREA' , 'ZONE' , 'OWNER' , 'STATUS'
], NAZOS)load_data = NAZOS
load_data['ID'] = load_id [0]
# -------------------------------------------------------------------
---------------------------------------------------------------------
------------------
##################################################
## Remove repeated laod bus data #################
##################################################
p=range(len(load_data['number']))
m=[]
for h in p:
 for d in p:
 if load_data['number'][h] == load_data['number'][d] and d >
h:
 n = 0
 for s in range(len(m)):
 if m[s] == d:
 n = n + 1
 if n == 0:
 m.append(d)
m.sort()
g=0
for r in range(len(m)):
 del load_data['number'][m[r]-g]
 del load_data['area'][m[r]-g]
 del load_data['zone'][m[r]-g]
 del load_data['owner'][m[r]-g]
 del load_data['status'][m[r]-g]
 del load_data['ID'][m[r]-g]
 g = g + 1
##-------------------------------------------------------------------
---------------------------------------------------------------------
------------------
##################################################
## Write Output Prompt ###########################
##################################################
psspy.prompt(' Please enter a CSV file wit hthe following format: \n\
 \n\
 - The CSV file can be saved in the same directory as
Python file in advance or just type a new file name')
psspy.prompt("\n\
 - The file name has to be typed in the following space in
this format: \n\
 \n\

*********************'FILENAME.csv'**************************")
##################################################
## Find area numbers #############################
##################################################
area = area[0]
p=range(len(area))
m=[]
for h in p:
 for d in p:
 if area[h] == area[d] and d > h:
 n = 0
 for s in range(len(m)):
 if m[s] == d: n = n + 1
 if n == 0:
 m.append(d)
m.sort()
g=0
for r in range(len(m)):
 del area[m[r]-g]
 g = g + 1
##Area = load_data['area']
##for q in range(len(load_data['area'])):
## for w in range(len(Area)):
## if load_data['area'][q] == Area[w]:
## Area['area'][w] = 0
# -------------------------------------------------------------------
---------------------------------------------------------------------
------------------
##################################################
## Get load data from a CSV file #################
##################################################
hourload = readloaddata('load.csv')
# -------------------------------------------------------------------
---------------------------------------------------------------------
------------------
##################################################
## Changing the load in the system ###############
##################################################
Lossa=[]
Loss = {}
dict = 0
for k in range(24):
 k = k + 2
 for i in range(len(hourload[0])):
 busnumber = hourload[0][i]
 busid = str(hourload [1][i])
 for j in range(len(load_data ['ID'])):
 busid_PSSE = load_data ['ID'][j]
 if busid_PSSE[1] == ' ' :
 busid_PSSE = busid_PSSE[0]
 if busid == busid_PSSE and busnumber == load_data
['number'][j]:
 ierr5 =
psspy.load_data(load_data['number'][j],load_data['ID'][j],[load_data[
'status'][j] ,load_data['area'][j] ,load_data['zone'][j],

load_data['owner'][j]],[hourload[k][i],0 , 0 ,0 ,0,0])
 if i <= 19:
 reactive = hourload[k][i+1]
 ierr5 =
psspy.load_data(load_data['number'][j],load_data['ID'][j],[load_data[
'status'][j] ,load_data['area'][j] ,load_data['zone'][j],

load_data['owner'][j]],[hourload[k][i],reactive,0,0,0])
# -------------------------------------------------------------------
---------------------------------------------------------------------
------------------
##################################################
## Run the power flow again for new load #########
##################################################ErrLF = psspy.fdns([0,0,0,0,0,1,0,0]) #
1.Tap:disable///2.AreaExchange:disable///2.PhaseShif:disable///4.dcTa
p:disable///5.ShuntAdj:disable///#
6.FlatStart:enable///7.ApplyVarL:on interations///8.nondivergent:disable
# -------------------------------------------------------------------
---------------------------------------------------------------------
------------------
##################################################
## Get the new loss in each area with new load ###
##################################################
 Lossa=[]
 for q in range(len(area)):
 ierr12, cmpval = psspy.ardat(area[q], 'LOSS')
 Lossa.append(cmpval.real)
 Loss [k-2] = Lossa
##-------------------------------------------------------------------
---------------------------------------------------------------------
------------------
##################################################
## Send the voltage to the provided csv file #####
##################################################
##rvaldimension = len(cmpval[0])
csvfile = input('Please one .csv file')
if csvfile: # open CSV file to write
 csvfile_h = open(csvfile,'w')
 report = csvfile_h.write
else: # send results to PSS/E
report window
 psspy.beginreport()
 report = psspy.report
for i in range(len(area)):
 busn = area[i]
 report("%(busn)6d," %vars())
 for j in range(len(Loss)):
 busvoltag = Loss[j][i]
 report("%(busvoltag)3.4F," %vars())
 report("\n ")
# -------------------------------------------------------------------
---------------------------------------------------------------------
------------------
##################################################
## Close The CSV file ############################
##################################################
if csvfile:
 csvfile_h.close()
 print '\n Done ..... Power Flow Results Report saved to file %s'
% csvfile
else:
 print '\n Done ..... Power Flow Results Report created in Report
window.'
# -------------------------------------------------------------------
---------------------------------
