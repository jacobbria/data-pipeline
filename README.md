# data-pipeline
Pipeline using Matillion, Snowflake, and PowerBI.

Tools: Matillion, Snowflake, PowerBI
<details>
  <summary>Why Those Tools?</summary>
  
</details>

Data Source: [Dirty Dataset to practice Data Cleaning](http://kaggle.com/datasets/amruthayenikonda/dirty-dataset-to-practice-data-cleaning)

<div align="center">

  ### Figure 1: Pipeline
<img width="843" height="262" alt="image" src="https://github.com/user-attachments/assets/e242c204-de80-4048-a199-b935f2a2bf6b" />
</div>

-------------------------------------

### Extract & Load - BRONZE
We will be lading the .CSV file into an internal staging ground using Snowflake directly. This data will go into your **Bronze Database** which is where un-normalized and as-is data sits in the warehouse for storage. Specifically, each source of data will have its own schema and so the _Dirty Dataset_ will be placed in **BRONZE.DIRECT.UPLOAD** schema.


<div align="center">
  
### Figure 2: Database/Schema Creation
<img width="591" height="248" alt="image" src="https://github.com/user-attachments/assets/f3247662-489d-477c-a250-552fb432486f" />

### Figure 3: Stage (Internal) Creation
<img width="664" height="200" alt="image" src="https://github.com/user-attachments/assets/4bf54d43-f27d-4422-bb2a-19b11ca5c014" />

</div>

Using the Snowflake GUI I imported the CSV right into a (newly created) table using the Snowflake GUI. It's just an easier process for one-off uploads like this - if I was converting larger amounts of
data or setting up a more continuous flow than scripting it/progammatically would be a better approach.


<div align="center">
  
### Figure 4: Messy Data Table
<img width="1178" height="715" alt="image" src="https://github.com/user-attachments/assets/0effa717-20b6-43cc-91c5-29cb09c34076" />

<details>
  <summary>Design Choice: Why all strings?</summary>
  Strings/VARCHAR is a safe choice when bringing in a dataset as we can take the existing format and later transform/convert it into a better data 
</details>

</div>

---------------------------------------------

### Transformation A - SILVER
With our data stored in **BRONZE** we now need to get it cleaned up and stored to be used in production. Our **SILVER** will be where the data lives for internal use. It needs to be accessible, system agnostic, and efficienlty stored for querying. The commonly used STAR SCHEMA will be put into place here. **Figure 4** belows whos how the this specific table will be stored.

<div align="center">

  ### Figure 5: Silver ERP 
<img width="597" height="556" alt="image" src="https://github.com/user-attachments/assets/add4728e-5db0-4bb9-93eb-be453373e185" />
<details>
  <summary>Design Choice: Why STAR?</summary>
  The STAR schema is simple to understand and quick to implement. In comparison to normalized/3NF schemas (something often taught and used by myself in college) they rely less on joins and are less taxing on computations.
</details>

</div>

The raw data also contains information that will demands specific design choices to address. In **Figrure 2** you can see the dataset contains ERRORS, NULLS, or other input which could break our tables, analysis, or converting the data into specific data types. 

The first test will be getting product names into the DIM_PRODUCT table. Inside of Matillion, I selected a Calculator, Distinct, and Output table component to transform the data. The goal was conolidate all distinct possible products and any errors or unknowns to be pushed into a catch-all category: "UNKNOWN". 

<div align="center">
  
### Figure 6: Matillion Flow
<img width="578" height="150" alt="image" src="https://github.com/user-attachments/assets/1465f84f-82e1-4859-9ef9-1ea474dc0c30" />

### Figure 7: Snowflake Results
<img width="859" height="485" alt="image" src="https://github.com/user-attachments/assets/01bf1f30-87a4-488a-a22c-d4256e636c75" />

<details>
  <summary>Design Choice: Why UNKNOWN</summary>
  There is a need for a fair way to evalulate the data and get as close to the truth as the data allows - garbage in, garbage out etc etc. By converting the problematic values into unknown we can still have a chance to see what our sales look like in general. 
</details>

</div>


