# Ex.No: 13 Mini Project – Visualization-Aided ML Preprocessor Using CLI  
### DATE:                                                                            
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

### Data Discription
```py
import pandas as pd

class DataDescription:
    # The Task associated with this class.
    tasks = [
        '\n1. Describe a specific Column',
        '2. Show Properties of Each Column',
        '3. Show the Dataset'
    ]

    def __init__(self, data):
        self.data = data

    # The function that prints the database on the command line.
    def showDataset(self):
        while(1):
            try:
                rows = int(input(("\nHow many rows(>0) to print? (Press -1 to go back)  ")))
                if rows == -1:
                    break
                if rows <= 0:
                    print("Number of rows given must be +ve...\U0001F974")
                    continue
                print(self.data.head(rows))
            except ValueError:
                print("Numeric value is required. Try again....\U0001F974")
                continue
            break
        return

    # function to print all the columns
    def showColumns(self):
        for column in self.data.columns.values:
            print(column, end="  ")

    # function to describe the dataset or any specific column.
    def describe(self):
        while(1):
            print("\nTasks (Data Description)\U0001F447")
            for task in self.tasks:
                print(task)

            while(1):
                try:
                    choice = int(input(("\n\nWhat you want to do? (Press -1 to go back)  ")))
                except ValueError:
                    print("Integer Value required. Try again.....\U0001F974")
                    continue
                break

            if choice==-1:
                break
                        
            elif choice==1:
                self.showColumns()
                while(1):
                    describeColumn = input("\n\nWhich Column?  ").lower()
                    try:
                        # describe() function is used to tell all the info regarding any specific column.
                        print(self.data[describeColumn].describe())
                    except KeyError:
                        print("No Column present with this name. Try again....\U0001F974")
                        continue
                    break
            
            elif choice==2:
                # describe() function is used to tell all the info about the database.
                print(self.data.describe())
                print("\n\n")
                print(self.data.info())

            elif choice==3:
                self.showDataset()

            else:
                print("\nWrong Integer value!! Try again..\U0001F974")
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
### Feature Scaling
```py
import pandas as pd
from data_description import DataDescription
from sklearn.preprocessing import MinMaxScaler, StandardScaler

class FeatureScaling:
    
    bold_start = "\033[1m"
    bold_end = "\033[0;0m"

    # All the Tasks associated with this class.
    tasks = [
        "\n1. Perform Normalization(MinMax Scaler)",
        "2. Perform Standardization(Standard Scaler)",
        "3. Show the Dataset"
    ]
    
    tasks_normalization = [
        "\n1. Normalize a specific Column",
        "2. Normalize the whole Dataset",
        "3. Show the Dataset"
    ]

    tasks_standardization = [
        "\n1. Standardize a specific Column",
        "2. Standardize the whole Dataset",
        "3. Show the Dataset"
    ]

    def __init__(self, data):
        self.data = data
    
    # Performs Normalization on specific column or on whole dataset.
    def normalization(self):
        while(1):
            print("\nTasks (Normalization)\U0001F447")
            for task in self.tasks_normalization:
                print(task)

            while(1):
                try:
                    choice = int(input(("\n\nWhat you want to do? (Press -1 to go back)  ")))
                except ValueError:
                    print("Integer Value required. Try again.....\U0001F974")
                    continue
                break
    
            if choice == -1:
                break
            
            # Performs normalization on the columns provided.
            elif choice == 1:
                print(self.data.dtypes)
                columns = input("Enter all the column"+ self.bold_start + "(s)" + self.bold_end + "you want to normalize (Press -1 to go back)  ").lower()
                if columns == "-1":
                    break
                for column in columns.split(" "):
                    # This is the basic approach to perform MinMax Scaler on a set of data.
                    try:
                        minValue = self.data[column].min()
                        maxValue = self.data[column].max()
                        self.data[column] = (self.data[column] - minValue)/(maxValue - minValue)
                    except:
                        print("\nNot possible....\U0001F636")
                print("Done....\U0001F601")

            # Performs normalization on whole dataset.
            elif choice == 2:
                try:
                    self.data = pd.DataFrame(MinMaxScaler().fit_transform(self.data))
                    print("Done.......\U0001F601")

                except:
                    print("\nString Columns are present. So, " + self.bold_start + "NOT" + self.bold_end + " possible.\U0001F636\nYou can try the first option though.")
                
            elif choice==3:
                DataDescription.showDataset(self)

            else:
                print("\nYou pressed the wrong key!! Try again..\U0001F974")

        return

    # Function to perform standardization on specific column(s) or on whole dataset.
    def standardization(self):
        while(1):
            print("\nTasks (Standardization)\U0001F447")
            for task in self.tasks_standardization:
                print(task)

            while(1):
                try:
                    choice = int(input(("\n\nWhat you want to do? (Press -1 to go back)  ")))
                except ValueError:
                    print("Integer Value required. Try again.....")
                    continue
                break

            if choice == -1:
                break
            
            # This is the basic approach to perform Standard Scaler on a set of data.
            elif choice == 1:
                print(self.data.dtypes)
                columns = input("Enter all the column"+ self.bold_start + "(s)" + self.bold_end + "you want to normalize (Press -1 to go back)  ").lower()
                if columns == "-1":
                    break
                for column in columns.split(" "):
                    try:
                        mean = self.data[column].mean()
                        standard_deviation = self.data[column].std()
                        self.data[column] = (self.data[column] - mean)/(standard_deviation)
                    except:
                        print("\nNot possible....\U0001F636")
                print("Done....\U0001F601")
                    
            # Performing standard scaler on whole dataset.
            elif choice == 2:
                try:
                    self.data = pd.DataFrame(StandardScaler().fit_transform(self.data))
                    print("Done.......\U0001F601")
                except:
                    print("\nString Columns are present. So, " + self.bold_start + "NOT" + self.bold_end + " possible.\U0001F636\nYou can try the first option though.")
                break

            elif choice==3:
                DataDescription.showDataset(self)

            else:
                print("\nYou pressed the wrong key!! Try again..\U0001F974")

        return

    # main function of the FeatureScaling Class.
    def scaling(self):
        while(1):
            print("\nTasks (Feature Scaling)\U0001F447")
            for task in self.tasks:
                print(task)
            
            while(1):
                try:
                    choice = int(input(("\n\nWhat you want to do? (Press -1 to go back)  ")))
                except ValueError:
                    print("Integer Value required. Try again.....\U0001F974")
                    continue
                break
            if choice == -1:
                break
            
            elif choice == 1:
                self.normalization()

            elif choice == 2:
                self.standardization()

            elif choice==3:
                DataDescription.showDataset(self)
            
            else:
                print("\nWrong Integer value!! Try again..\U0001F974")
        # Returns all the changes on the DataFrame.
        return self.data
```

### Imputation
```py
import pandas as pd
from data_description import DataDescription


class Imputation:
    
    bold_start = "\033[1m"
    bold_end = "\033[0;0m"

    # The Task associated with this class.
    tasks = [
        "\n1. Show number of Null Values",
        "2. Remove Columns",
        "3. Fill Null Values (with mean)",
        "4. Fill Null Values (with median)",
        "5. Fill Null Values (with mode)",
        "6. Show the Dataset"
    ]

    def __init__(self, data):
        self.data = data

    # function to show columns of the DataFrame
    def showColumns(self):
        print("\nColumns\U0001F447\n")
        for column in self.data.columns.values:
            print(column, end = "  ")
        return
    
    # function to print the number of NULL values present in each column
    def printNullValues(self):
        print("\nNULL values of each column:")
        for column in self.data.columns.values:
            # isnull checks on each value of a column that whether the value is null or not.
            print('{0: <20}'.format(column) + '{0: <5}'.format(sum(pd.isnull(self.data[column]))))
        print("")
        return

    # function to remove a column from the DataFrame
    def removeColumn(self):
        self.showColumns()
        while(1):
            columns = input("\nEnter all the column"+ self.bold_start + "(s)" + self.bold_end + "you want to delete (Press -1 to go back)  ").lower()

            if columns == "-1":
                break

            choice = input("Are you sure?(y/n) ")
            if choice=="y" or choice=="Y":
                try:
                    # inplace = True otherwise, the changes won't reflect on the DataFrame.
                    self.data.drop(columns.split(" "), axis = 1, inplace = True)
                except KeyError:
                    print("One or more Columns are not present. Try again.....\U0001F974")
                    continue
                print("Done.......\U0001F601")
                break
            else:
                print("Not Deleting........\U0001F974")
        return

    # function that fills null values with the mean of that column.
    def fillNullWithMean(self):
        self.showColumns()
        while(1):
            column = input("\nEnter the column name:(Press -1 to go back)  ").lower()
            if column == "-1":
                break
            choice = input("Are you sure? (y/n)  ")
            if choice=="y" or choice=='Y':
                try:
                    self.data[column] = self.data[column].fillna(self.data[column].mean())
                except KeyError:
                    print("Column is not present. Try again.....\U0001F974")
                    continue
                except TypeError:
                    # Imputation is only possible on some specific datatypes like int, float etc.
                    print("The Imputation is not possible here\U0001F974. Try on another column.")
                    continue
                print("Done......\U0001F601")
                break
            else:
                print("Not changing........\U0001F974")
        return


    # function that fills null values with the median of that column.
    def fillNullWithMedian(self):
        self.showColumns()
        while(1):
            column = input("\nEnter the column name:(Press -1 to go back)  ").lower()
            if column == "-1":
                break
            choice = input("Are you sure? (y/n)  ")
            if choice=="y" or choice=='Y':
                try:
                    self.data[column] = self.data[column].fillna(self.data[column].median())
                except KeyError:
                    print("Column is not present. Try again.....\U0001F974")
                    continue
                except TypeError:
                    print("The Imputation is not possible here\U0001F974.Try on another column.")
                    continue
                print("Done......\U0001F601")
                break
            else:
                print("Not changing........\U0001F974")
        return

    # function that fills null values with the mean of that column.

    # function that fills null values with the mode of that column.
    def fillNullWithMode(self):
        self.showColumns()
        while(1):
            column = input("\nEnter the column name:(Press -1 to go back)  ").lower()
            if column == "-1":
                break
            choice = input("Are you sure? (y/n)  ")
            if choice=="y" or choice=='Y':
                try:
                    # Mode provides us with dataframe so, we will take 1st value(most probable value).
                    # Look into the documentation, if any doubts.
                    self.data[column] = self.data[column].fillna(self.data[column].mode()[0])
                except KeyError:
                    print("Column is not present. Try again.....\U0001F974")
                    continue
                except TypeError:
                    print("The Imputation is not possible here\U0001F974. Try on another column.")
                    continue
                print("Done......\U0001F601")
                break
            else:
                print("Not changing........\U0001F974")
        return

    # main function of the Imputation Class.
    def imputer(self):
        while(1):
            print("\nImputation Tasks\U0001F447")
            for task in self.tasks:
                print(task)

            while(1):
                try:
                    choice = int(input(("\nWhat you want to do? (Press -1 to go back)  ")))
                except ValueError:
                    print("Integer Value required. Try again.....\U0001F974")
                    continue
                break

            if choice == -1:
                break

            elif choice==1:
                self.printNullValues()

            elif choice==2:
                self.removeColumn()

            elif choice==3:
                self.fillNullWithMean()

            elif choice==4:
                self.fillNullWithMedian()
            
            elif choice==5:
                self.fillNullWithMode()

            elif choice==6:
                DataDescription.showDataset(self)

            else:
                print("\nWrong Integer value!! Try again..\U0001F974")
        return self.data
```

### Visualization
```py
# import seaborn as sns
# import matplotlib.pyplot as plt
# from data_analysis import DataAnalysis
# class Visualization:
#     def __init__(self, data):
#         self.data = data
    
#     def show_boxplot(self, columns):
#         for column in columns:
#             plt.figure(figsize=(10, 5))
#             sns.boxplot(x=self.data[column])
#             plt.title(f'Boxplot for {column}')
#             plt.show()
#             input("Press Enter to view the next plot...")

#     def show_scatterplot(self, columns):
#         for column in columns:
#             plt.figure(figsize=(10, 5))
#             sns.scatterplot(x=self.data.index, y=self.data[column])
#             plt.title(f'Scatterplot for {column}')
#             plt.show()
#             input("Press Enter to view the next plot...")

#     def select_columns(self):
#         available_columns = self.data.select_dtypes(include=["float", "int"]).columns
#         print("\nAvailable numerical columns:")
#         for i, column in enumerate(available_columns, 1):
#             print(f"{i}. {column}")
        
#         selected_indices = input("Enter the numbers of columns to visualize (comma-separated): ")
#         selected_indices = [int(i.strip()) - 1 for i in selected_indices.split(",")]
        
#         selected_columns = [available_columns[i] for i in selected_indices if i in range(len(available_columns))]
#         return selected_columns

#     # def visualizeMain(self):
#     #     print("\nVisualization Options:")
#     #     print("1. Boxplot")
#     #     print("2. Scatterplot")
#     #     plot_choice = input("Choose the type of plot (1 or 2): ")

#     #     if plot_choice not in ["1", "2"]:
#     #         print("Invalid choice! Please try again.")
#     #         return

#     #     print("\n1. Visualize all columns")
#     #     print("2. Visualize selected columns")
#     #     column_choice = input("Do you want to visualize all columns or only selected ones? (1 or 2): ")

#     #     if column_choice == "1":
#     #         columns = self.data.select_dtypes(include=["float", "int"]).columns
#     #     elif column_choice == "2":
#     #         columns = self.select_columns()
#     #         if not columns:
#     #             print("No valid columns selected. Please try again.")
#     #             return
#     #     else:
#     #         print("Invalid choice! Please try again.")
#     #         return

#     #     if plot_choice == "1":
#     #         self.show_boxplot(columns)
#     #     elif plot_choice == "2":
#     #         self.show_scatterplot(columns)


#     def visualizeMain(self):
#         print("\nWould you like to analyze the dataset before visualization? (yes/no)")
#         analyze_choice = input().lower()

#         if analyze_choice == "yes":
#             DataAnalysis(self.data).analyze()

#         print("\nVisualization Options:")
#         print("1. Boxplot")
#         print("2. Scatterplot")
#         plot_choice = input("Choose the type of plot (1 or 2): ")

#         if plot_choice not in ["1", "2"]:
#             print("Invalid choice! Please try again.")
#             return

#         print("\n1. Visualize all columns")
#         print("2. Visualize selected columns")
#         column_choice = input("Do you want to visualize all columns or only selected ones? (1 or 2): ")

#         if column_choice == "1":
#             columns = self.data.select_dtypes(include=["float", "int"]).columns
#         elif column_choice == "2":
#             columns = self.select_columns()
#             if not columns:
#                 print("No valid columns selected. Please try again.")
#                 return
#         else:
#             print("Invalid choice! Please try again.")
#             return

#         if plot_choice == "1":
#             self.show_boxplot(columns)
#         elif plot_choice == "2":
#             self.show_scatterplot(columns)


import seaborn as sns
import matplotlib.pyplot as plt
from data_analysis import DataAnalysis
from visualization_options import VisualizationOptions
class DataVisualization:
    def __init__(self, data):
        self.data = data

    def get_color_choice(self):
        print("\nAvailable Colors: red, blue, green, purple, orange, brown, pink, gray, yellow, cyan")
        color = input("Enter your desired color: ").lower()
        label = input("Enter what this color represents: ")
        return color, label

    def analyze_data(self):
        print("Analyzing the dataset...\n")
        print("Data Types:\n", self.data.dtypes)
        print("\nNull Values:\n", self.data.isnull().sum())
        print("\nBasic Statistics:\n", self.data.describe())

    # def scatterplot(self, x_col, y_col):
    #     color, label = self.get_color_choice()
    #     plt.figure(figsize=(10, 6))
    #     sns.scatterplot(data=self.data, x=x_col, y=y_col, color=color)
    #     plt.title(f'Scatterplot of {y_col} vs {x_col}')
    #     plt.xlabel(x_col)
    #     plt.ylabel(f'{y_col} ({label})')
    #     plt.show()

    def scatterplot(self, x_col, y_col):
        colors = []
        labels = []

        # Ask for colors and labels for both columns
        for column in [x_col, y_col]:
            color, label = self.get_color_choice()
            colors.append(color)
            labels.append(label)

        plt.figure(figsize=(10, 6))
        
        # Plot the first column (x-axis) against the second column (y-axis)
        sns.scatterplot(data=self.data, x=self.data[x_col], y=self.data[y_col], color=colors[1], label=labels[1])
        
        plt.title(f'Scatterplot of {y_col} vs {x_col}')
        plt.xlabel(f'{x_col} ({labels[0]})')
        plt.ylabel(f'{y_col} ({labels[1]})')
        plt.legend()
        plt.show()


    def boxplot(self, x_col, y_col):
        color, label = self.get_color_choice()
        plt.figure(figsize=(10, 6))
        sns.boxplot(data=self.data, x=x_col, y=y_col, color=color)
        plt.title(f'Boxplot of {y_col} by {x_col}')
        plt.xlabel(x_col)
        plt.ylabel(f'{y_col} ({label})')
        plt.show()

    def pairplot(self, columns):
        sns.pairplot(self.data[columns])
        plt.title('Pairplot for Selected Columns')
        plt.show()

    def heatmap(self, columns):
        plt.figure(figsize=(12, 8))
        sns.heatmap(self.data[columns].corr(), annot=True, cmap='coolwarm')
        plt.title('Heatmap of Correlations')
        plt.show()

    def multivariate_analysis(self):
        columns = self.select_columns()
        print("\nMultivariate Analysis Options:")
        print("1. Pairplot")
        print("2. Heatmap")
        choice = input("Choose the type of multivariate analysis (1 or 2): ")

        if choice == "1":
            self.pairplot(columns)
        elif choice == "2":
            self.heatmap(columns)
        else:
            print("Invalid choice! Please try again.")

    def select_columns(self):
        print("\nAvailable columns:")
        for i, column in enumerate(self.data.columns, 1):
            print(f"{i}. {column}")
        
        selected_indices = input("Enter the numbers of columns to visualize (comma-separated): ")
        selected_indices = [int(i.strip()) - 1 for i in selected_indices.split(",")]
        selected_columns = [self.data.columns[i] for i in selected_indices if i in range(len(self.data.columns))]
        return selected_columns

    def run_visualization(self):
        print("\nWould you like to analyze the dataset before visualization? (yes/no)")
        analyze_choice = input().lower()

        if analyze_choice == "yes":
            self.analyze_data()

        print("\nVisualization Options:")
        print("1. Scatterplot")
        print("2. Boxplot")
        print("3. Multivariate Analysis")
        choice = input("Choose the type of visualization (1-3): ")

        if choice not in ["1", "2", "3"]:
            print("Invalid choice! Please try again.")
            return

        if choice == "1":
            columns = self.select_columns()
            if len(columns) < 2:
                print("You need to select at least two columns for a scatterplot.")
                return
            self.scatterplot(columns[0], columns[1])

        elif choice == "2":
            columns = self.select_columns()
            if len(columns) < 2:
                print("You need to select at least two columns for a boxplot.")
                return
            self.boxplot(columns[0], columns[1])

        elif choice == "3":
            self.multivariate_analysis()

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
