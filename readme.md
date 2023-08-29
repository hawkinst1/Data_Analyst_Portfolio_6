Brief
=============================
Client is opening a new pizza shop with only take outs and they need data:
1) Build a bespoke relational database
2) Create a dashboard for monitoring the data

The main areas of focus they require are
- Orders
- Stock Control
- Staff

===========================
**Design**

I will design the database around what fields they need data in,
then proceed with **normalising** the data and establishing relationships.
The fields required by the business are shown below, but I will use other
examples to inform what other columns should be added.

Customer Orders table
- Item name
- Item price
- Quantity
- Customer name
- Delivery address

Using quick DBD we can quickly create a representation of the tables.
It becomes apparent that there is a need to create mulitple tables with relationships to remove redundant data from one larger table
and is laid out:

orders
-
row_id int pk
order_id varchar(100)
created_at datetime
item_id int
quantity int
cust_id int
delivery boolean
address_id int

customer
-
cust_id int pk FK >- orders.cust_id
cust_firstName varchar(100)
cust_lastName varchar(100)

address
-
address_id int pk FK >- orders.address_id
delivery_address1 varchar(255)
delivery_address2 varchar(255) NULL
delivery_city varchar(50)
delivery_postcode varchar(20)

Item
-
item_id int pk FK >- orders.item_id
sku int
item_name varchar(5)
item_cat varchar(5)
item_size varchar(5)
item_price decimal(5,2)

**Stock Control Requirements**
what does it need to do?
- know when stock is empty
- existing stock amount

adding these tables to our scheme:
ingredient
-
ing_id int pk FK >- recipe.ing_id
ing_name varchar(255)
ing_weight int
ing_meas varchar(20)
ing_price decimal(5,2)

recipe
-
row_id int pk
recipe_id varchar(20) FK >- Item.sku
ing_id varchar(10) FK >- inventory.inv_id
quantity int

inventory
-
inv_id int pk
item_id varchar(10)
quantity int










