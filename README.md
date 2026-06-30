# data-pipeline
Pipeline using Matillion, Snowflake, and PowerBI.

Tools: Matillion, Snowflake, PowerBI
<details>
  <summary>Why Those Tools?</summary>
  
</details>

Data Source: [Dirty Dataset to practice Data Cleaning](http://kaggle.com/datasets/amruthayenikonda/dirty-dataset-to-practice-data-cleaning)


### Extract & Load
We will be lading the .CSV file into an internal staging ground using Snowflake directly. This data will go into your **Bronze Database** which is where un-normalized and as-is data sits in the warehouse for storage. Specifically, each source of data will have its own schema and so the _Dirty Dataset_ will be placed in **BRONZE.DIRECT.UPLOAD** schema.


<div align="center">
  
### Figure 1: Database/Schema Creation
<img width="591" height="248" alt="image" src="https://github.com/user-attachments/assets/f3247662-489d-477c-a250-552fb432486f" />

### Figure 2: Stage (Internal) Creation
<img width="664" height="200" alt="image" src="https://github.com/user-attachments/assets/4bf54d43-f27d-4422-bb2a-19b11ca5c014" />

</div>

Using the Snowflake GUI I imported the CSV right into a (newly created) table using the Snowflake GUI. It's just an easier process for one-off uploads like this - if I was converting larger amounts of
data or setting up a more continuous flow than scripting it/progammatically would be a better approach.


<div align="center">
  
### Figure 3: Messy Data Table
<img width="1178" height="715" alt="image" src="https://github.com/user-attachments/assets/0effa717-20b6-43cc-91c5-29cb09c34076" />

<details>
  <summary>Design Choice: Why all strings?</summary>
  Strings/VARCHAR is a safe choice when bringing in a dataset as we can take the existing format and later transform/convert it into a better data 
</details>

</div>

### Transformation A - Silver
With our data stored in **BRONZE** we now need to get it cleaned up and stored to be used in production. Our **SILVER** will be where the data lives for internal use. It needs to be accessible, system agnostic, and efficienlty stored for querying. The commonly used STAR SCHEMA will be put into place here. **Figure 4** belows whos how the this specific table will be stored.

<div align="center">

  ### Figure 4: Silver ERP 
<img width="597" height="556" alt="image" src="https://github.com/user-attachments/assets/add4728e-5db0-4bb9-93eb-be453373e185" />
<details>
  <summary>Design Choice: Why STAR?</summary>
  The STAR schema is simple to understand and quick to implement. In comparison to normalized/3NF schemas (something often taught and used by myself in college) they rely less on joins and are less taxing on computations.
</details>

</div>


