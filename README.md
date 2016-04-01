# hello-world
Hello!!!


#Prework2
# -*- coding: utf-8 -*-
"""
Created on Tue Mar 29 15:37:28 2016

@author: n.duro
"""
########################
#   import data        #
########################
import numpy as np
import pandas as pd
df = pd.read_csv('C:/Documents/n.duro/Desktop/Kaggle/train.csv', header=0)

########################
# Descriptive Analysis #
########################
print df.describe()


#global
x=len(df[(df['Survived'] == 1)])
y=len(df)
tx_surv= 1.0 * x / y
print tx_surv

#par pclass
def surv(var, taille):
    surv=[]
    for i in range(1,taille+1):
        surv.append(len(df[ (df['Survived'] == 1) & (df[var] == i) ]))
    return surv


def dead(var, taille):
    dead=[]
    for i in range(1,taille+1):
        dead.append(len(df[ (df['Survived'] == 0) & (df[var] == i) ]))
    return dead


def pclass():
    survivors=surv('Pclass', 3)
    deads=dead('Pclass', 3)
    taux_surv = np.zeros((3,2))
    #return survivors, deads, taux_surv
    for j in range(0,3):
        
        taux_surv[j][0]=j+1
        taux_surv[j][1]= 1.0*survivors[j]/(survivors[j]+deads[j])
      
    #print "taux de survie classe 1: %s ; tx surv classe 2: %s ; tx surv classe 3: %s"    %(taux_surv[0][2], taux_surv[1][2], taux_surv[2][2])

    return taux_surv
pclass()

#Par Embarked
Qs=(len(df[ (df['Survived'] == 1) & (df["Embarked"] == "Q") ]))
Qd=(len(df[ (df['Survived'] == 0) & (df["Embarked"] == "Q") ]))    
 
tx_Q= 1.0*Qs/(Qs+Qd)

Cs=(len(df[ (df['Survived'] == 1) & (df["Embarked"] == "C") ]))
Cd=(len(df[ (df['Survived'] == 0) & (df["Embarked"] == "C") ]))

tx_C= 1.0*Cs/(Cs+Cd)

Ss=(len(df[ (df['Survived'] == 1) & (df["Embarked"] == "S") ]))
Sd=(len(df[ (df['Survived'] == 0) & (df["Embarked"] == "S") ]))

tx_S= 1.0*Ss/(Ss+Sd)


print "taux de Q: %s ; taux de C: %s ; taux de S: %s" %(tx_Q, tx_C, tx_S)

#taux de Q: 0.38961038961 ; taux de C: 0.553571428571 ; taux de S: 0.336956521739




#histogramme

df["Age"].hist()

df.boxplot("Age")


############################
# Variables change         #
############################
#rename variables
def renamegen(df):
    df['Gender'] = df['Sex'].map( {'female': 0, 'male': 1} ).astype(int)

renamegen(df)


# delete variables not used

df = df.drop(['Name', 'Sex', 'Ticket', 'Cabin', 'PassengerId'], axis=1) 



#Missing value
def checkmiss(df):
    print 'Age Missing Value %s' %len(df[df['Age'].isnull()][['Pclass', 'Age']])
    print 'Pclass Missing Value %s' %len(df[df['Pclass'].isnull()][['Pclass']])
    print 'SibSp Missing Value %s' %len(df[df['SibSp'].isnull()][['Pclass', 'Age']])
    print 'Parch Missing Value %s' %len(df[df['Parch'].isnull()][[ 'Pclass', 'Age']])
    print 'Fare Missing Value %s' %len(df[df['Fare'].isnull()][[ 'Pclass', 'Age']])
    print 'Embarked Missing Value %s' %len(df[df['Fare'].isnull()][[ 'Pclass', 'Age']])

checkmiss(df)
print 'Survived Missing Value %s' %len(df[df['Survived'].isnull()][['Survived', 'Pclass', 'Age']])
    
# missing value on Age (177)
def agenan(df):
    median_ages = np.zeros((2,3))
    median_ages
    for i in range(0, 2):
        for j in range(0, 3):
            median_ages[i,j] = df[(df['Gender'] == i) & \
                                  (df['Pclass'] == j+1)]['Age'].dropna().median()
 
    median_ages

    for i in range(0, 2):
        for j in range(0, 3):
            df.loc[ (df.Age.isnull()) & (df.Gender == i) & (df.Pclass == j+1),\
                    'Age'] = median_ages[i,j]

agenan(df)

checkmiss(df)

df = df.dropna()

df['EmbarkedP'] = df['Embarked'].map( {'C': 1, 'Q': 2, 'S': 3} ).astype(int)
df = df.drop(['Embarked'], axis=1) 
def cross(df):
    df['FamilySize'] = df['SibSp'] + df['Parch']
    df['Age*Class'] = df.Age * df.Pclass
    return df
cross(df)


#############################
# Sample                    #
#############################
number = len(df)

size=int(0.3*number)

from sklearn.cross_validation import train_test_split

train_data, test_data=train_test_split(df, test_size=size)

#check taux de survie
x_train=len(train_data[(train_data['Survived'] == 1)])
y_train=len(train_data)
tx_surv_train= 1.0 * x_train / y_train

x_test=len(test_data[(test_data['Survived'] == 1)])
y_test=len(test_data)
tx_surv_test= 1.0 * x_test / y_test


print tx_surv        #0.383838383838
print tx_surv_train  #0.380417335474
print tx_surv_test   #0.387218045113

data=df.as_matrix(columns=None)
train=train_data.as_matrix(columns=None)
test=test_data.as_matrix(columns=None)


#############################
# Models                    #
#############################
#random forest
# Import the random forest package
from sklearn.ensemble import RandomForestClassifier 

# Create the random forest object which will include all the parameters
# for the fit
forest = RandomForestClassifier(n_estimators = 100)

# Fit the training data to the Survived labels and create the decision trees
forest = forest.fit(train[0::,1::],train[0::,0])

# Take the same decision trees and run it on the test data
output = forest.predict(test[0::,1::])


real=test[0::,0]

def verif(output):
    i=0
    j=0
    while i <len(real):
        if real[i]== output[i]:
            j=j+1
        i=i+1
    tx_rep=1.0*j/len(real)
    return tx_rep
verif(output)


#accuracy
a_rf=forest.score(test[0::,1::], real, sample_weight=None)
#a_rf=tx_rep

#logistic regression
# Import the random forest package
from sklearn import linear_model

logreg = linear_model.LogisticRegression(C=1e5)

# we create an instance of Neighbours Classifier and fit the data.
reg=logreg.fit(train[0::,1::],train[0::,0])

# Take the same decision trees and run it on the test data
output_reg = logreg.predict(test[0::,1::])


real=test[0::,0]

#accuracy
a_log=logreg.score(test[0::,1::], real, sample_weight=None)


if a_log>=a_rf: 
    print "Select: Logistic Regression"
else:
    print "Select: Random Forest"
    



#test sur la base fournie
import pandas as pd
test_imp = pd.read_csv('C:/Documents/n.duro/Desktop/Kaggle/test.csv', header=0)

test_imp.head()


#retraitement de la donnée
renamegen(test_imp)
test_imp = test_imp.drop(['Name', 'Sex', 'Ticket', 'Cabin', 'PassengerId'], axis=1) 
checkmiss(test_imp)
agenan(test_imp)
checkmiss(test_imp)
test_imp = test_imp.dropna()


test_imp['EmbarkedP'] = test_imp['Embarked'].map( {'C': 1, 'Q': 2, 'S': 3} ).astype(int)
test_imp = test_imp.drop(['Embarked'], axis=1) 
cross(test_imp)
testons=test_imp.as_matrix(columns=None)

#test model
output_reg = forest.predict(testons[0::,0::])


#taux de surv
number_passengers = np.size(output_reg.astype(np.float))
number_survived = np.sum(output_reg.astype(np.float))
proportion_survivors = number_survived / number_passengers
print "Proportion survivors: %s" %proportion_survivors

#Proportion survivors: 0.364508393285
