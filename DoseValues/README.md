# DoseValues

This folder contains the files needed for running the DoseValues program. This program is built in Python, and can be run using Jupyter Notebook. 

To run this program, you will need to know how many tablets you want to run the program for. 

- DoseValues2 for 2 tablets
- DoseValues3 for 3 tablets
- DoseValues4 for 4 tablets
- DoseValues5 for 5 tablets

In the code, you can change values to configure them for what you want.

- minDose is used to configure the lowest dose to be administered to patients, and is what the program will start with when calculating a solution.
- maxDose is used to configure the largest dose to be administered to patients, and is what the program will end with when calculating a solution.
- lowestManufacturableTabletStrength is the value for changing the lowest manufacturable tablet strength. There will be no value in the solution that is less than the lowest manufacturable tablet strength.
- smallestPenaltyTabletSize is a penalty variable used in calculating the average. The penalty will be used in calculating the average by default. You can generate values for the top combinations without the penalty by running the last block in the program. 

We've gone over each of the variables for the first block of code. The second block of code calculates the best solution for the variables we specified. We can use the pickle save and load blocks to save and load variables for later.

The next block plots the best solutions on a graph. This graph is adjustable for presenting your data.

Finally, the last plot finds the top solutions, but calculating them without the penalty.
