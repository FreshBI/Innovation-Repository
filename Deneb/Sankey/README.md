# Deneb Sankey Chart

## Sections

- [PowerQuery](#power-queries)
- [Vega](#vega)

## Vega

For some reason, the collect transform isn't working to correct the order of items in the datasets.

### Visual Elements
[![SankeyChart](https://raw.githubusercontent.com/FreshBI/Innovation-Repository/main/Deneb/Sankey/Images/Sankey%20With%20Lines.png)](https://app.powerbi.com/view?r=eyJrIjoiZjVmZmZhNjQtODNlMi00MmU1LWIyODktY2MxNTE5NDEyYzZjIiwidCI6ImNhMjJhYmIxLTY3OWYtNDQyZi1iYTRkLTg4NWZlNWIxZTQ2NCIsImMiOjZ9)
- Black Arrows are "Plotted Stacks"
- Green Arrows are links from the "linkTable" 

### Datasets

There are 6 datasets in use for the Sankey chart. These were originally defined by Davide.

#### 1. Dataset
- This is the datset that deneb attempts to create for Vega using the InputData Dataset

#### 2. Stacks
- This dataset defines a simplified version of "Dataset"
- At this point we add any images for end nodes	
	- There might be a way to do it via the Dataset
	
#### 3. MaxValue
- This dataset defines the max value of the sankey, this helps with scale later on

#### 4. plottedStacks
- Defines the bounds for Stacks on the chart

#### 5. finalTable
- This is the final definition of the datset

#### 6. linkTable
- Defines the bounds for links on the chart


## Power Queries
### 1. Data Extraction

- **Description**: This Power Query extracts raw data from the source.
- **Steps**:
  - Connect to the data source.
  - Load the necessary data tables.
  - Perform initial data cleansing (if required).

### 2. NodeSort_StartingNodes

- **Description**: This Power Query focuses on getting the stack, sort and image of all starting nodes.
- **Steps**:
  - Remove all other columns.
  - Remove duplicates.
  - Standardize data names.

### 3. NodeSort_EndingNodes

- **Description**: This Power Query gets any ending nodes that are not listed in the starting node table.
- **Steps**:
  - Extract stack 6
  - Get everything BUT stack 6
  - Combine them both

### 4. DATE

- **Description**: This is a helper column that gets the distinct date dim items
- **Steps**:
  - Isolate DATE
  - Remove duplicates
  - Add Cross Join Key
  
### 5. NodeSort_CrossJoin

- **Description**: This Power Query combines NodeSort_StartingNodes and NodeSort_EndingNodes and adds the Date Dimension.
- **Steps**:
  - Append both Node Sorts
  - Add Cross Join Key
  - Cross Join to Date

### 6. NodeSize

- **Description**: This Power Query defines the actual Fact data.
- **Steps**:
  - Remove the categorical data ( sort, stack ) from Data Extraction

### 6. InputData

- **Description**: This Power Query final dataset for the Sankey
- **Steps**:
  - Append NodeSize & NodeSort_CrossJoin
  - Rename some columns