
## myRetail RESTful service

# Overview:

I have created a REST service exposing CDS views for the requirement specified in Tech assessment case study using SAP trial cloud platform in Eclipse.
As per requirement, we are getting data from three different sources( NoSQL DB, redsky.target.com and within SAP ), aggregating the data and returning it as JSON to the caller.


# Please follow below steps to replicate the development 

- Lets first start with creating a PACKAGE by right clicking on project->New->ABAP Package. Name it 'ZPRODUCT_API'
- Create Data Dictionary elements by right clicking on package ZPRODUCT_API->New->Other ABAP Repository Object->Dictionay->Object type.
	- Create objects as specified in GITHUB folder 'ZPRODUCT_API/Dictionary' with object name specified as file name.
- Create Classes by right clicking on package ZPRODUCT_API->New->ABAP class.
	- Create classes as specified in GITHUB folder 'ZPRODUCT_API/Source Code Library/Classes' with object name specified as file name.
- Create CDS Views by right clicking on package ZPRODUCT_API->New->Other ABAP Repository Object->Core Data Services->Data Definition.
	- Select template "Define custom entity with Parameters" while creating CDS views.
	- Create CDS Views as specified in GITHUB folder 'ZPRODUCT_API/Core Data Services/Data Definitions' with View name specified as file name.
- Create Service Definition for the CDS view 'ZI_PRODUCT_DTLS' by right clicking on CDS view ZI_PRODUCT_DTLS and click on New Service Definition.
	- Name it ZSD_PRODUCT_DTLS and copy code from GITHUB file 'ZPRODUCT_ID/Business Services/Service Definitions/ZSD_PRODUCT_DTLS'.
- Create Service binding for Service Definition 'ZSD_PRODUCT_DTLS' by right clicking on service definition ZSD_PRODUCT_DTLS and click on New Service Binding.
	- Name it ZSB_PPRODUCT_DETAILS, Activate and Publish the service.
	

# Steps to test the service.

- Run class ZCL_ADD_PRODUCT_HDR_DATA to add some test data to our table ZPRODUCT_DTLS
- Click 'Service URL' from Service binding ZSB_PPRODUCT_DETAILS. This will open Browser with Service details.
	- URL looks like <https://48d61f78-e6aa-480f-94e4-ecaf26360dcb.abap-web.us10.hana.ondemand.com/sap/opu/odata/sap/ZSD_PRODUCT_DETAILS/?sap-client=100>
- From URL, replace the portion after /ZSD_PRODUCT_DETAILS/ with <b>product?&$format=json</b> to get all the product list.
	- URL looks like  <https://48d61f78-e6aa-480f-94e4-ecaf26360dcb.abap-web.us10.hana.ondemand.com/sap/opu/odata/sap/ZSD_PRODUCT_DETAILS/product?&$format=json>
- To get Product price, along with product list replace the portion after /ZSD_PRODUCT_DETAILS/ with <b>product?&\$expand=to_price&$format=json<b>
	- URL looks like  <https://48d61f78-e6aa-480f-94e4-ecaf26360dcb.abap-web.us10.hana.ondemand.com/sap/opu/odata/sap/ZSD_PRODUCT_DETAILS/product?&$expand=to_price&$format=json>
- To use filters,	replace the portion after /ZSD_PRODUCT_DETAILS/ with with <b>product&\$expand=to_price&\$filter=product_id%20eq%20%2713860428%27&$format=json</b>
	- URL looks like <https://48d61f78-e6aa-480f-94e4-ecaf26360dcb.abap-web.us10.hana.ondemand.com/sap/opu/odata/sap/ZSD_PRODUCT_DETAILS/product&$expand=to_price&$filter=product_id%20eq%20%2713860428%27&$format=json>.
	- We have following filters product_id, product_cat and id_type. We can mix and match filters as needed.
	
# NOTE: We are getting Product_name from redsky.target.com and Product_pice from NoSQL DB(In our case bonsai.io).

# I have added sample JSON to a separate file 'Sample JSON'
