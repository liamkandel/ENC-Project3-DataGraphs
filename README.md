# ENC-Project3-DataGraphs
This is a part of Project 3 in my ENC class where we remediate a genre of our choice.
I wanted to represent algorithmic bias by writing a Python script that generates 500
applicants who are given a random set of the following features:

Race, Gender, Age, Birth Region, Education 

Based on their features, each applicants salary was altered in a way to show bias. 
I plotted the data using Seaborn:

# Line Plots
![line-race](https://github.com/liamkandel/ENC-Project3-DataGraphs/assets/84248497/857b90ad-0d1c-4e06-954e-f10ab6db8b12)

![line-gender](https://github.com/liamkandel/ENC-Project3-DataGraphs/assets/84248497/81e455b7-039e-4c2c-b486-192713783d85)

![line-education](https://github.com/liamkandel/ENC-Project3-DataGraphs/assets/84248497/22a992a2-7e20-4182-ae71-48f903cd4128)

![line-birthregion](https://github.com/liamkandel/ENC-Project3-DataGraphs/assets/84248497/e06f3b9f-d67a-4218-8224-8e5d64b7e527)

# Bar Graphs

![bar-race](https://github.com/liamkandel/ENC-Project3-DataGraphs/assets/84248497/14ec84d4-77ab-4b80-9d72-994f626558d4)

![bar-education](https://github.com/liamkandel/ENC-Project3-DataGraphs/assets/84248497/720a0aa0-3505-4a47-b723-f1ca86dc7d3f)

![bar-birthregion](https://github.com/liamkandel/ENC-Project3-DataGraphs/assets/84248497/afa1220a-9788-4c9f-8437-47d63c6ba5ec)

# Code for Dataset and Plot creation
```import random
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import json

SIZE = 500 # How many applicants to generate

genders = ["Man", "Woman"]
races = ["White", "Non-white"]
education_level = ["Ivy League", "Public University"]
birth_place = ["US Born", "Born outside of US"]

applicants = {"Name": [], "Gender": [], "Race": [], "Salary": [], "Age": [], "Education": [] , "Birth Region": []}

for i in range(SIZE):
    # Give each applicant a standard name indicating their number
    applicants["Name"].append("Applicant " + str(i + 1))
    # Give each applicant a random gender
    applicants["Gender"].append(random.choice(genders))
    # Give each applicant a random race
    applicants["Race"].append(random.choice(races))
    # Give each applicant an education
    applicants["Education"].append(random.choice(education_level))
    # Give each applicant an age
    applicants["Age"].append(random.randint(22,60))
    # Give each applicant a birth region
    applicants["Birth Region"].append(random.choice(birth_place))
    # Randomly generate a base salary for each applicant
    applicants["Salary"].append(random.randint(50000, 56000))

    # Here is where the bias ensues...

    if applicants["Gender"][i] == "Man":
        applicants["Salary"][i] *= 1.1
    else:
        applicants["Salary"][i] *= 0.95

    if applicants["Race"][i] == "White":
        applicants["Salary"][i] *= 1.1
    else:
        applicants["Salary"][i] *= 0.95

    if applicants["Education"][i] == "Ivy League":
        applicants["Salary"][i] *= 1.1
    else:
        applicants["Salary"][i] *= 0.95
    
   # if applicants["Age"][i] >= 40:
    applicants["Salary"][i] *= applicants["Age"][i] / 40
    #else:
        #applicants["Salary"][i] *= 0.95
    
    if applicants["Birth Region"][i] == "US Born":
        applicants["Salary"][i] *= 1.1
    else:
        applicants["Salary"][i] *= 0.95
    
    # Round the salary to 2 decimal places.
    applicants["Salary"][i] = round(applicants["Salary"][i], 2)
    

# Create an applicants.json and write the applicants into it
with open("applicants.json", "w") as outfile:
    json.dump(applicants, outfile)

# Create a dataframe using the json above
df = pd.read_json("applicants.json")

g = sns.lineplot(data=df, x="Age", y="Salary", hue="Gender")
g = sns.lineplot(data=df, x="Age", y="Salary", hue="Birth Region")
g = sns.lineplot(data=df, x="Age", y="Salary", hue="Education")
g = sns.lineplot(data=df, x="Age", y="Salary", hue="Race")

g = sns.barplot(data=df, x="Gender", y="Salary", hue="Race")
g = sns.barplot(data=df, x="Gender", y="Salary", hue="Education")
g = sns.barplot(data=df, x="Gender", y="Salary", hue="Birth Region")```
