# Ex.No: 13 Mini Project – Visualization-Aided ML Preprocessor Using CLI  
### DATE: 24/10/2024                                                                           
### REGISTER NUMBER :212222040154
### AIM: 
To write a program to visualize the data in dataset using CLI(Command Line Interface).
###  Algorithm:
1. Initialize CLI Tool
Start the command-line tool, setting up the initial configuration.
Prompt the user to load or specify the dataset (e.g., CSV or database).
2. Data Loading and Profiling
Load Data: Load the dataset and display a quick summary (e.g., number of rows, columns, and types of each feature).
Profile Data: Analyze the data, generating basic statistics (mean, median, mode, standard deviation, etc.) and detecting features like missing values, outliers, and unique values.
Display Initial Summary: Output the data profile summary with recommendations for preprocessing (e.g., columns with missing values, potential categorical columns for encoding).
3. Real-Time Data Visualization
Based on user input, display various types of visualizations using libraries like Matplotlib or Seaborn:
Distribution Plots: Plot histograms or KDEs for continuous features.
Outlier Detection: Display box plots or scatter plots highlighting outliers.
Correlation Heatmaps: Visualize correlations between features.
Interactive CLI Commands: Allow users to enter commands to specify the type of visualization they want to see, updating the display accordingly.
4. Data Transformation Commands
Provide CLI options for common transformations:
Handle Missing Values: Commands to drop, fill, or impute missing values.
Scaling and Normalization: Allow users to apply scaling techniques (e.g., Min-Max, Standard scaling) and visualize changes.
Encoding Categorical Variables: Automate encoding processes like one-hot encoding, and display how the data structure changes.
Preview Transformation Results: Show sample rows or summaries after each transformation.
5. Automated Preprocessing Suggestions
Based on the data profile, suggest preprocessing steps automatically:
For example, suggest scaling for numerical columns, encoding for categorical features, or outlier treatment.
Confirmation and Interactive Edits: Allow users to review and approve each suggested step, showing results interactively.
6. Save and Export Processed Data
After all transformations are confirmed, provide an option to save the preprocessed data in various formats (e.g., CSV, Excel).
Allow exporting a summary report of all transformations and visualizations applied.
7. End Session
End the session and provide options to restart or exit.

### Program:
### Categorical.py
```py
import pandas as pd
from sklearn.preprocessing import LabelEncoder, OneHotEncoder
from data_description import DataDescription

class Categorical:
    # The Task associated with this class.
    tasks = [
        '\n1. Show Categorical Columns',
        '2. Performing One Hot encoding',
        '3. Show the Dataset'
    ]
    def __init__(self, data):
        self.data = data

    # function to show all the categorical columns and number of unique values in them.
    def categoricalColumn(self):
        print('\n{0: <20}'.format("Categorical Column") + '{0: <5}'.format("Unique Values"))
        # select_dtypes selects the columns with object datatype(which could be further categorize)
        for column in self.data.select_dtypes(include="object"):
            print('{0: <20}'.format(column) + '{0: <5}'.format(self.data[column].nunique()))

    # function to encode any particular column
    def encoding(self):
        categorical_columns = self.data.select_dtypes(include="object")
        while(1):
            column = input("\nWhich column would you like to one hot encode?(Press -1 to go back)  ").lower()
            if column == "-1":
                break
            # The encoding function is only for categorical columns.
            if column in categorical_columns:
                self.data = pd.get_dummies(data=self.data, columns = [column])
                print("Encoding is done.......\U0001F601")
                
                choice = input("Are there more columns to be encoded?(y/n)  ")
                if choice == "y" or choice == "Y":
                    continue
                else:
                    self.categoricalColumn()
                    break
            else:
                print("Wrong Column Name. Try Again...\U0001F974")

    # The main function of the Categorical class.
    def categoricalMain(self):
        while(1):
            print("\nTasks\U0001F447")
            for task in self.tasks:
                print(task)

            while(1):
                try:
                    choice = int(input(("\n\nWhat you want to do? (Press -1 to go back)  ")))
                except ValueError:
                    print("Integer Value required. Try again...\U0001F974")
                    continue
                break

            if choice == -1:
                break
            
            elif choice == 1:
                self.categoricalColumn()

            elif choice == 2:
                self.categoricalColumn()
                self.encoding()

            elif choice == 3:
                DataDescription.showDataset(self)

            else:
                print("\nWrong Integer value!! Try again..\U0001F974")
        # return the data after modifying
        return self.data
```
### Data Analysis
```py
import pandas as pd
class DataAnalysis:
    def __init__(self, data):
        self.data = data

    def analyze(self):
        print("\nDataset Overview:")
        print(f"\nNumber of Rows: {self.data.shape[0]}")
        print(f"Number of Columns: {self.data.shape[1]}")
        print("\nColumn-wise Data Types and Null Value Counts:\n")
        print(self.data.info())
        print("\nNull Values in each Column:\n")
        print(self.data.isnull().sum())
```

### Data Input
```py
from os import path
import sys
import pandas as pd
# argv.py
class DataInput:
    
    bold_start = "\033[1m"
    bold_end = "\033[0;0m"

    # all extensions supported by this project.
    supported_file_extensions = [
        '.csv',
    ]

    # function to convert all the column names of any specific dataset into lowercase.
    def change_to_lower_case(self, data):
        for column in data.columns.values:
            data.rename(columns = {column : column.lower()}, inplace = True)
        return data

    # function that takes any dataset from the input file and convert it into DataFrame.
    # The print statements are well defined and tells about the state of the errors.
    def inputFunction(self):
        try:
            filename, file_extension = path.splitext(sys.argv[1])
            if file_extension == "":
                raise SystemExit(f"Provide the " + self.bold_start + "DATASET" + self.bold_end +" name (with extension).\U0001F643")

            if file_extension not in self.supported_file_extensions:
                raise SystemExit(f"This file extension is not " + self.bold_start + "supported.\U0001F643" + self.bold_end)
        
        except IndexError:
            raise SystemExit(f"Provide the " + self.bold_start + "DATASET" + self.bold_end +" name (with extension).\U0001F643")
        
        try:
            data = pd.read_csv(filename+file_extension)
        except pd.errors.EmptyDataError:
            raise SystemExit(f"The file is "+ self.bold_start + "EMPTY" + self.bold_end + "\U0001F635")

        except FileNotFoundError:
            raise SystemExit(f"File " + self.bold_start + "doesn't" + self.bold_end + " exist\U0001F635")

        data = self.change_to_lower_case(data)

        return data
```

### Download 
```py
import pandas as pd

class Download:

    bold_start = "\033[1m"
    bold_end = "\033[0;0m"

    def __init__(self, data):
        self.data = data

    # download the modified DataFrame as .csv file
    def download(self):
        toBeDownload = {}
        for column in self.data.columns.values:
            toBeDownload[column] = self.data[column]

        newFileName = input("\nEnter the " + self.bold_start +"FILENAME" + self.bold_end +" you want? (Press -1 to go back):  ")
        if newFileName=="-1":
            return
        newFileName = newFileName + ".csv"
        # index=False as this will not add an extra column of index.
        pd.DataFrame(self.data).to_csv(newFileName, index = False)
        
        print("Hurray!! It is done....\U0001F601")
        
        if input("Do you want to exit now? (y/n) ").lower() == 'y':
            print("Exiting...\U0001F44B")
            exit()
        else:
            return
```

### Main
```py
from data_description import DataDescription
from data_input import DataInput
from imputation import Imputation
from download import Download
from categorical import Categorical
from feature_scaling import FeatureScaling
from visualization import DataVisualization
from data_analysis import DataAnalysis


class Preprocessor:

    bold_start = "\033[1m"
    bold_end = "\033[0;0m"
    
    # The Task associated with this class. This is also the main class of the project.
    tasks = [
        '1. Data Description',
        '2. Handling NULL Values',
        '3. Encoding Categorical Data',
        '4. Feature Scaling of the Dataset',
        '5. Download the modified dataset',
        '6. Visualize the Dataset'
    ]

    data = 0
    
    def __init__(self):
        self.data = DataInput().inputFunction()
        print("\n\n" + self.bold_start + "WELCOME TO THE MACHINE LEARNING PREPROCESSOR CLI!!!\N{grinning face}" + self.bold_end + "\n\n")

    # function to remove the target column of the DataFrame.
    def removeTargetColumn(self):
        print("Columns\U0001F447\n")
        for column in self.data.columns.values:
            print(column, end = "  ")
        
        while(1):
            column = input("\nWhich is the target variable:(Press -1 to exit)  ").lower()
            if column == "-1":
                exit()
            choice = input("Are you sure?(y/n) ")
            if choice=="y" or choice=="Y":
                try:
                    self.data.drop([column], axis = 1, inplace = True)
                except KeyError:
                    print("No column present with this name. Try again......\U0001F974")
                    continue
                print("Done.......\U0001F601")
                break
            else:
                print("Try again with the correct column name...\U0001F974")
        return
    
    def printData(self):
        print(self.data)

    # main function of the Preprocessor class.
    def preprocessorMain(self):
        self.removeTargetColumn()
        while(1):
            print("\nTasks (Preprocessing)\U0001F447\n")
            for task in self.tasks:
                print(task)

            while(1):
                try:
                    choice = int(input("\nWhat do you want to do? (Press -1 to exit):  "))
                except ValueError:
                    print("Integer Value required. Try again.....\U0001F974")
                    continue
                break

            if choice == -1:
                exit()

            # moves the control into the DataDescription class.
            elif choice==1:
                DataDescription(self.data).describe()


            # moves the control into the Imputation class.
            elif choice==2:
                self.data = Imputation(self.data).imputer()
                

            # moves the control into the Categorical class.
            elif choice==3:
                self.data = Categorical(self.data).categoricalMain()


            # moves the control into the FeatureScaling class.
            elif choice==4:
                self.data = FeatureScaling(self.data).scaling()


            # moves the control into the Download class.
            elif choice==5:
                Download(self.data).download()

            elif choice == 6:
                DataVisualization(self.data).run_visualization()

            
            else:
                print("\nWrong Integer value!! Try again..\U0001F974")

# obj is the object of our Preprocessor class(main class).
obj = Preprocessor()
# the object 'obj' calls the main function of our Preprocessor class.
obj.preprocessorMain()
```
### Output:

![image](https://github.com/user-attachments/assets/73d001ae-6473-45ee-ab19-ef32bdc8a6bd)

![image](https://github.com/user-attachments/assets/aaf5536a-a397-4e59-90bc-5f724472ef64)


![image](https://github.com/user-attachments/assets/9b1b8270-86a6-4e83-86b9-33fd5eb1c994)


![image](https://github.com/user-attachments/assets/05ef403d-c9ea-4d43-b19d-146493559479)

![image](https://github.com/user-attachments/assets/bba3a1fd-c14e-4b32-8d60-016f6b8563a4)

![image](https://github.com/user-attachments/assets/adba61f8-ba2e-4a47-b03a-7652da30ba91)

![image](https://github.com/user-attachments/assets/970b54a1-3a22-455b-871c-a2ed3523ec54)

![image](https://github.com/user-attachments/assets/6f41de92-81e6-4870-82f6-6111a31d2463)


### Result:
Thus the program for visualizing and preprocessing the data has been successfully executed .
