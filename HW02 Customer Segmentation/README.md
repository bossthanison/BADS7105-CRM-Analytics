# HW02 Customer Segmentation
## 1. Import Library
[![](https://img.shields.io/badge/-Pandas-red)](#) [![](https://img.shields.io/badge/-Numpy-red)](#) [![](https://img.shields.io/badge/-Scipy-red)](#) [![](https://img.shields.io/badge/-Sklearn-red)](#) [![](https://img.shields.io/badge/-Matplotlib-red)](#) 
## 2. Covert transaction data to customer single view

เนื่องจากข้อมูลที่ได้มาเป็นข้อมูล Transaction data (1 แถวเป็น 1 การซื้อ) แต่เราจะแบ่งกลุ่มของลูกค้า ดังนั้นเราจึงต้องทำการแปลงข้อมูลให้อยู่ในรูป Costomer single view (1 แถวเป็น 1 ลูกค้า)

![ccc](https://user-images.githubusercontent.com/78030264/147262041-d81f0634-0a09-41cc-9075-4304368bae50.png)

โดยทำการเลือก Feature สำหรับการแปลงดังนี้
### Feature
* ```TotalSpend```  : ค่าใช้จ่ายทั้งหมดของลูกค้า
* ```TotalVisits``` : จำนวนครั้งทั้งหมดที่เข้ามาในร้าน
* ```TotalProd``` : จำนวนสินค้าที่รายการไม่ซ้ำที่ซื้อในร้าน
* ```SHOP_WEEKDAYS``` : วันไหนที่ลูกค้าเข้ามาในร้านบ่อยสุด
* ```SHOP_HOURS``` : ชั่วโมงไหนที่ลูกค้าเข้ามาในร้านบ่อยสุด
* ```BASKET_DOMINANT``` : ประเภทของสินค้าแบบไหนที่ลูกค้าซื้อบ่อยสุด {'Grocery':0,'Fresh':1,'Mixed':2,'Nonfood':3,'XX':4}
* ```TicketSize``` : ค่าใช้จ่ายที่ซื้อต่อจำนวนครั้งที่เข้ามา
## 3. Exploratory data

ทำการสำรวจข้อมูลของลูกค้า เพื่อให้เห็นภาพรวม และเข้าใจลูกค้ามากขึ้น โดยในที่นี้จะขอพล๊อต Histogram 

![explor (1)](https://user-images.githubusercontent.com/78030264/147194218-b5855b14-696c-4798-95b5-974a503b0c4c.png)

### Insight
* ```TotalSpend,TotalVisits,TotalProd,TicketSize```  : กราฟจะคล้ายๆกัน โดยกราฟเบ้ขวา ลูกค้าที่ยิ่งซื้อมาก มาบ่อยจะยิ่งลดลงเรื่อยๆ
* ```SHOP_WEEKDAYS``` : ลูกค้าส่วนใหญ่จะมาวันจันทร์ อังคารมากสุด และจะมาน้อยสุดในวันเสาร์และอาทิตย์
* ```SHOP_HOURS``` : ชั่วโมงที่ลูกค้ามาบ่อยสุดจะเป็น12.00, 14.00, 20.00
* ```BASKET_DOMINANT``` : ลูกค้าส่วนใหญ่จะซื้อสินค้าประเภท Fresh มากสุด
## 4. K-mean
ทำการจัดกลุ่ม customer โดยจำเป็นต้องหาจำนวนกลุ่มที่เหมาะสม โดยการพล๊อต Elbow เพื่อเลือกจำนวนกลุ่มที่เหมาะสม 
<img src="https://user-images.githubusercontent.com/78030264/147193125-01889e58-c8f3-420a-af69-fe911b42a7cb.png" width="600" >

จากกราฟเลือกจำนวนกลุ่มเท่ากับ 4 เนื่องจากหลังจาก 4 เป็นต้นไป จำนวนกลุ่มเพิ่มแต่การเปลี่ยนแปลงเกิดขึ้นเพียงเล็กน้อย

## 5. PCA for visualization
เนื่องจากจำนวน feature มีมากกว่า 2 ทำให้เราไม่สามารถ visualization ได้ ดังนั้นจึงนำ PCA มาใช้สำหรับการ Visualization โดยจะเห็นว่า 4 กลุ่มของลูกค้าค่อนข้างแบ่งกันได้อย่างชัดเจน

<img src="https://user-images.githubusercontent.com/78030264/147194376-4176521b-5e5d-4bbe-b5fa-8aec388318b6.png" width="600" >



## 6. Interpret results
เนื่องจากเราจำเป็นต้องอธิบายได้ว่าแต่ละกลุ่มมีคุณลักษณะเป็นอย่างไร เพื่อที่เราจะสามารถไปทำ action ได้ถูกกับลูกค้าแต่ละกลุ่ม โดยจะทำ 2 วิธี คือ

### Descriptive Statistics
เป็นการเช็คสถิติในแต่ละกลุ่ม เพื่อดูว่าแต่ละกลุ่มมีคุณสมบัติเป็นอย่างไร

![bbbb](https://user-images.githubusercontent.com/78030264/147261367-79d1991a-045d-4481-bab8-1bead0a8f1d6.png)

### Tree model
ทำการนำ K-mean ที่ได้ไปเข้า Tree model เนื่องจาก Tree model สามารถพล็อตเป็น Tree diagram แล้วตรวจสอบดูว่า  algorithm ใช้กฎเกณฑ์อะไรในการแยกแต่ละกลุ่ม
![tree](https://user-images.githubusercontent.com/78030264/147202996-1b3fbe59-6b82-4778-b1ae-34d01164eee5.png)

## 7. Conclusion
ทำการสรุปผลแต่ละกลุ่ม โดยตั้งชื่อกลุ่มตามการวิวัฒนาการของมนุษย์ โดยไม่ได้เอาชื่อมนุษย์มาใช้เพราะทุกคนล้วนเป็นมนุษย์เหมือนกันทั้งหมด

<img src="https://earth-chronicles.com/wp-content/uploads/2018/12/fdaa8dbac4a71ffe210dccdb66ec3b5a1.jpg" width="400" >

![aaaaaaa](https://user-images.githubusercontent.com/78030264/147260781-abd1f8f5-6e9a-45c8-9c94-810372469543.png)


