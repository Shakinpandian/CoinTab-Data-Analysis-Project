# CoinTab-Data-Analysis-Project

<p align="center">
     <img width="400" height="200" src="https://user-images.githubusercontent.com/119164734/217199260-5be1827e-9677-47de-9720-420bfa942003.jpg")
">
</p>

## 1.Problem Statement

You are a data analyst and have a large ecommerce company in India (let’s call it X) as a client.X gets a few thousand orders via their website on a daily basis and they have to deliver them as fast as they can. For delivering the goods ordered by the customers, X has tied up with multiple courier companies in India who charge them some
amount per delivery.The charges are dependent upon two factors:

**● Weight of the product**

**● Distance between the warehouse (pickup location) and customer’s delivery address
(destination location)**

On an average, the delivery charges are Rs. 100 per shipment. So if X ships 1,00,000 orders per month, they have to pay approximately Rs. 1 crore to the courier companies on a monthly basis as charges.
**As the amount that X has to pay to the courier companies is very high, they want to verify if the
charges levied by their partners per Order are correct.**

**Input Data**

Left Hand Side (LHS) Data (X’s internal data spread across three reports)
● Website order report which will list Order IDs and various products (SKUs) part of each order. Order ID is common identifier between X’s order report and courier company invoice

● SKU master with gross weight of each product. This should be used to calculate total weight of each order and during analysis compare against one reported by courier
company in their CSV invoice per Order ID. The courier company calculates weight in slabs of 0.5 KG multiples, so first you have to figure out the total weight of the shipment and then figure out applicable weight slabs.

For example:

**- If the total weight is 400 gram then weight slab should be 0.5**

**- If the total weight is 950 gram then weight slab should be 1**

**- If the total weight is 1 KG then weight slab should be 1**

**- If the total weight is 2.2 KG then weight slab should be 2.5**

Warehouse pincode to All India pincode mapping (this should be used to figure out delivery zone (a/b/c/d/e) and during analysis compare against one reported by courier
company in their CSV invoice per Order ID ,RHS Data (courier company invoice in CSV file)

● Invoice in CSV file mentioning AWB Number (courier company’s own internal ID), OrderID (company X’s order ID), weight of shipment, warehouse pickup pincode, customer
delivery pincode, zone of delivery, charges per shipment, type of shipment.

● Courier charges rate card at weight slab and pincode level. If the invoice mentions “Forward charges” then only forward charges (“fwd”) should be applicable as per zone and fixed & additional weights based on weight slabs. If the invoice mentions “Forward and rto charges” then forward charges (“fwd”) and RTO charges (“rto”) should be applicable as per zone and fixed & additional weights based on weight slabs.

● For the first 0.5 KG, “fixed” rate as per the slab is applicable. For each additional 0.5 KG, “additional” weight in the same proportion is applicable. Total charges will be “fixed” + “total additional” if any

**Output Data 1**

Create a resultant CSV/Excel file with the following columns:

**● Order ID**

**● AWB Number**

**● Total weight as per X (KG)**

**● Weight slab as per X (KG)**

**● Total weight as per Courier Company (KG)**

**● Weight slab charged by Courier Company (KG)**

**● Delivery Zone as per X**

**● Delivery Zone charged by Courier Company**

**● Expected Charge as per X (Rs.)**

**● Charges Billed by Courier Company (Rs.)**

**● Difference Between Expected Charges and Billed Charges (Rs.)**

**Output Data 2**

Create a summary table

|  | Count | Amount |
|---|---|---|
|Total orders where X has been correctly charged |||
|Total Orders where X has been overcharged |||
|Total Orders where X has been undercharged|||

## 2. Importing Libaries

The following libaries and tools are used in the project.
<p align="center">
  <img width="400" height="200" src="https://user-images.githubusercontent.com/119164734/208314443-2ef2756b-470c-4619-b7a4-1a48bf3c6cd3.png">
</p>

- **Pandas**: Importing for panel data analysis                     

- **Numpy**: For numerical python operations

- **Matplotlib (Pyplot)**: A popular plotting library used along with pandas

- **Seaborn**: A library, built on matplotlib, to create beautiful plots

- **Scikit Learn**: To perform all tasks realted to Machine Learning

## 3. Loadinng Data

- The dataset has been collected from the **Cointab Career Portal** and the dataset in **CSV** format.

- To load the data **pd.read_csv("data//path")**.

- From the data **567 rows** and **13 features** are gained.

## 4 Merge the common tables:

<p align="center">
     <img width="400" height="200" src="https://user-images.githubusercontent.com/119164734/217211451-dd26338a-c6a5-4d43-b235-1247516d0423.jpeg">
</p>



Combining a company_X_Order_Report with Courier_Company_invoice by using merge function 

For merging a data table, i have used a **"Order_ID"** feature and User-defined variable "o"

**O=pd.merge(Order,invoice,on="Order_ID",how="right")**

To combine a user_defined variable "o" of a new data table with Company_X_SKU_Master table by using merge function

For merging a data table, i have used a "SKU" feature and User-defined variable "Ex"

**Ex=pd.merge(O,sku,on="SKU",how="left")**

To combine a user_defined variable "Ex" of a new data table with Company_X_Pincode_Zone table by using merge function

For merging a data table, i have used a Customer_Pincode feature and User-defined variable "Ones"

**Ones=pd.merge(Ex,X_post,how="inner",on="Customer_Pincode")**

## 5. Rename the columns name as per require:

**Ones.rename**(columns={"Charged Weight":"Charged_Weight","Warehouse_Pincode_x":"Company_Warehouse_Pincode",
                                                      "Customer_Pincode":"Company_Customer_Pincode","Zone_x":"Company_Zone",
                                                       "Warehouse_Pincode_y":"X_Warehouse_Pincode","Zone_y":"X_Zone","Weight (g)":"Weight_(g)"},inplace=True)
                     
**Ones.rename**(columns={"Company_Zone":"Delivery_Zone_charged_by_Courier_Company","X_Zone":"Delivery_Zone_as_per_X"},inplace=True)

**Ones.rename**(columns={"Charged_Weight":"Total_weight_as_per_Courier_Company_(KG)"},inplace=True)

## 6. Calculation:

   ## Multiplication of Weight(g)*Order_qty, to find out the Total_weight_as_per_X(g)
   
   ## Convertion of gram to Kg of the total weight as per X company
   
**Ones["Total_weight_as_per_X(g)"]=Ones["Order_Qty"]*Ones["Weight_(g)"]**

**Ones["Total_weight_as_per_X(Kg)"]=Ones["Total_weight_as_per_X(g)"]/1000**

## 7. If condition To find out the Weight_slab_charged_by_Courier_Company_(KG),Expected_Charge_as_per_X_(Rs.),Charges_Billed_by_Courier_Company_(Rs.) and Difference_Between_Expected_Charges_and_Billed_Charges_(Rs.)

## 8. Conclusion:

| Summary Table | Count | Amount |
|---|---|---|
|Total orders where X has been correctly charged |17|1642.4|
|Total Orders where X has been overcharged |537|44676.1|
|Total Orders where X has been undercharged|13|1161.5|


 
 
