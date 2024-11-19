# Custom Order Import Flow in Hotwax Commerce

The **Custom Order Import Flow** in Hotwax Commerce is designed to integrate Shopify orders into the system based on each client’s specific requirements. This flow involves transforming the order JSON using Apache NiFi and making client-specific adjustments for seamless import into Hotwax Commerce.

---

## **New Era Caps Specific Flow**

### **Step 1: The ‘Import orders’ job runs in HotWax Commerce, downloading new order JSONs from Shopify.**
- **Job Name**: Import Orders  
- **Enum Id**: `JOB_IMP_ORD`  
- **Enum Name**: Import Orders  
- **Config Id**: `DNLD_ORDR`  

When the job runs, it downloads new order JSON files from Shopify.  
- **SFTP Path**: `./home/newera-uat-sftp/sync_contents/datamanager/imported/DNLD_ORDR`

---

### **Step 2: NiFi Transformation**
NiFi picks up the downloaded JSON files from the SFTP path and processes the following attributes from the payload:

#### **Attributes in the JSON Payload**
```json
[
  {"name": "代引き手数料", "value": "770円"},
  {"name": "配送日", "value": "2023/10/01"},
  {"name": "配送時間帯", "value": "18:00-20:00"}
]
```
#### **1. COD Fee**
- The field **"代引き手数料"** represents the COD Fee.  
- The COD Fee is split into:
  - **COD Fee VAT (10%)** → Mapped to `COD_FEE_TAX`  
  - **COD Fee (90%)** → Mapped to `COD_FEE`

#### **Example Calculation (COD Fee = 770)**
- **COD Fee Tax (10%)**: 70  
- **COD Fee (90%)**: 700  

#### **Order Adjustment Mapping**
| Order Adjustment Id | Order Adjustment Type Id | Order Id  | Amount |
|----------------------|--------------------------|-----------|--------|
| 100094              | COD_FEE_TAX             | NEC45433  | 70     |
| 100095              | COD_FEE                 | NEC45433  | 700    |

---

#### **2. Requested Delivery Date**
- The field **"配送日"** is mapped to the `requestedDeliveryDate` of the `order_item` entity.

---

#### **3. Requested Delivery Time**
- The field **"配送時間帯"** is mapped to the `requestedDeliveryTime` of the `order_item` entity.

---

### **Step 3: NiFi places the transformed JSON file back on the SFTP**

### **Step 4: The custom order import job runs in HotWax to pick up the file from SFTP.**
#### **The job runs and calls the service “ importJsonListData” which retrieves the transformed JSON from the SFTP path  /home/newera-uat-sftp/hotwax/oms/CustomOrderImport and imports it into Hotwax Commerce. The importJsonListData service calls these services in its implementation**

---

### **1. Custom Order Import Job**
- **Job Name**: Custom Order Import  
- **Enum Id**: `JOB_CSTM_IMP_ORD`  
- **Enum Name**: Custom Order Import  
- **Config Id**: `CSTM_ORDR_IMP`  
- **Property Resource**: `FTP_CONFIG`  

The job calls the **`importJsonListData`** service, which retrieves the transformed JSON from the SFTP path and imports it into Hotwax Commerce.

---

#### **Services Called**

##### **1. createShopifyOrder**
**Parameters**:  
- `UserLogin`  
- `Locale`  
- `timeZone`  
- `payload`  
- `shopifyConfigId`  

#### **2. createUpdateOrderAdjustment**
**Parameters**:  
- `orderAdjustmentTypeId`  
- `amount`  
- `orderItemSeqId`  
- `shipGroupSeqId`  

#### **3. updateAllOrderItems**
**Parameters**:  
- `requestedDeliveryDate`  
- `requestedDeliveryTime`  

---

## **Summary**
The Custom Order Import Flow handles the logic for the COD Fee as well as the requested delivery date and time of the new order JSON. It adjusts these fields according to the specific requirements of Hotwax Commerce. The custom order import job runs in Hotwax, retrieving the transformed JSON from the SFTP and ensuring the correct order details are imported into the system.

---

