# data-pipeline
Pipeline using Matillion, Snowflake, and PowerBI.

Tools: Matillion, Snowflake, PowerBI
<details>
  <summary>Why Those Tools?</summary>
  
</details>

Data Source: [Dirty Dataset to practice Data Cleaning](http://kaggle.com/datasets/amruthayenikonda/dirty-dataset-to-practice-data-cleaning)


### Extract Phase 
We will be lading the .CSV file into an internal staging ground using Snowflake directly. This data will go into your **Bronze Database** which is where un-normalized and as-is data sits in the warehouse for storage. Specifically, each source of data will have its own schema and so the _Dirty Dataset_ will be placed in **BRONZE.DIRECT.UPLOAD** schema.


<div align="center">
  
### Figure 1: Database/Schema Creation
<img width="591" height="248" alt="image" src="https://github.com/user-attachments/assets/f3247662-489d-477c-a250-552fb432486f" />

### Figure 2: Stage (Internal) Creation
<img width="664" height="200" alt="image" src="https://github.com/user-attachments/assets/4bf54d43-f27d-4422-bb2a-19b11ca5c014" />

</div>






<img width="1598" height="846" alt="image" src="https://github.com/user-attachments/assets/b5f0c6fe-f177-484d-a89e-38b7aa5e6301" />



