# SPCN-011

## ขั้นตอนและวิธีการสร้าง vm and ct on Proxmox cluster 
### 1.Create Master VM (ubuntu-22.04)
  - **1.1 ไปที่เมนูมุมบนขวา กด createVM** 
    
     
   ![image](https://user-images.githubusercontent.com/115439255/208156861-31ba52a5-145c-46d0-a61d-883db4b53e30.png)
   
  - **1.2 ทำการกรอกข้อมูลทำการสร้างให้เสร็จ**
  - **1.3 ทำการเข้าตัว VM ไป set profile**
  
   ![image](https://user-images.githubusercontent.com/115439255/208159929-0be9ef2d-3a81-4bd3-ae01-b092e69b6db5.png)
   
  - **1.4 ทำการ set SSH**
  
   ![image](https://user-images.githubusercontent.com/115439255/208160276-c0b76cdd-8459-427e-8257-b848fb52897d.png)
   
  - **1.5 เมื่อทำทุกอย่างเสร็จแล้วก็ login เข้ามาแล้วจะได้หน้าตาประมาณนี้**
  
   ![image](https://user-images.githubusercontent.com/115439255/208161154-1268fd2e-c954-463d-abed-1a5183b6daf0.png)
  
  - **1.6 ทำการ clone master vm to tamplate โดยคลิกขวาที่ตัว VM ที่เราสร้างขึ้นแล้วคลิกที่ Convert to template**
  
   ![image](https://user-images.githubusercontent.com/115439255/208161722-9b5d19ad-d315-4a03-b6fe-2a158c1196c3.png)
   
  - **1.7 ทำการ clone from template สร้าง vm ใหม่จำนวน 2 VM โดยคลิกขวาที่ตัว VM**
  
  ![image](https://user-images.githubusercontent.com/115439255/208162430-de5a31bc-38f3-4673-8e01-89426457365e.png)
  ![image](https://user-images.githubusercontent.com/115439255/208163027-a323167f-332f-42c1-b143-b3093df80e5a.png)

#### CODE เปลี่ยน Host Name
       
       sudo hostnamectl set -hostname (ตามด้วยชื่อที่จะตั้ง)

#### CODE เปลี่ยนตั้งเวลา
      
       sudo timedatectl set-timezone Asia/Bangkok (เซ็ตเวลา)

       date (ดูเวลา)
       
#### CODE ที่ใช้เกี่ยวกับ QEMU 

      sudo apt update; sudo apt upgrade -y (อัพเดท)

      sudo apt install qemu-guest-agent (ติดตั้ง QEMU Guest Agent)

      sudo systemctl start qemu-guest-agent (เปิดการทำงาน QEMU Guest Agent)

      sudo systemctl status qemu-guest-agent (ตรวจสอบสถานะการทำงานของ QEMU Guest Agent)
      
#### CODE ที่ใช้เปลี่ยน IP
      
      rm /var/lib/dbus/machine-id (ลบข้อมูล machine-id ในไฟล์ทั้งหมด)
      
      nano /etc/machine-id 
      
      ln -s /etc/machine-id /var/lib/dbus/machine-id (link ต่อกับ machine-id)
    
   - **1.8 summary screen**
      - summary Master VM
      
          ![image](https://user-images.githubusercontent.com/115439255/208167877-bdff2b4b-03a8-45bb-96f5-93348e1c2ef0.png)

      - summary clone 1
    
          ![image](https://user-images.githubusercontent.com/115439255/208167386-9f64040c-eae0-4add-906d-35ada309e285.png)
          
      - summary clone 2

          ![image](https://user-images.githubusercontent.com/115439255/208167611-3a37d820-e263-4e66-a4f4-54dec152c6c9.png)


### 2.Ceate vm from other OS
   - **2.1 ไปที่เมนูมุมบนขวา กด Create VM**
   
      ![image](https://user-images.githubusercontent.com/115439255/208156861-31ba52a5-145c-46d0-a61d-883db4b53e30.png)
      
   - **2.2 แล้วทำการกรอกข้อมูลให้เสร็จ โดย OS ที่ผมเลือกใช้เป็น debian ดังรูป**

      ![image](https://user-images.githubusercontent.com/115439255/208168783-4cc97b19-cbe3-4c17-b010-5cc7c5f0b71e.png)
      
      ![image](https://user-images.githubusercontent.com/115439255/208302266-cf43ff5d-5b45-443e-954a-06a0dea9068d.png)

   - **2.3 แล้วทำการเข้าตัว OS ไปเปลี่ยน timezone กับ เปิดใช้ตัว qemu โดยใช้โค้ดเดียวกับตอนสร้าง Master VM**
      - นี่คือ หน้า console ของ OS
      
        ![image](https://user-images.githubusercontent.com/115439255/208302338-eeb5c0b1-dff8-40a4-9357-49a63435fa3b.png)
      - นี่คือ หน้า summary
      
        ![image](https://user-images.githubusercontent.com/115439255/208302343-bb337a0e-68bc-460b-80db-71af764c424e.png)
        
### 3.create container template (select from CT list)
   - **3.1 ไปที่เมนูมุมบนขวา กด Create CT**

      ![image](https://user-images.githubusercontent.com/115439255/208156861-31ba52a5-145c-46d0-a61d-883db4b53e30.png)
      
   - **3.2 กรอกข้อมูลให้เรียบร้อย**
 
      ![image](https://user-images.githubusercontent.com/115439255/208171599-bf3f9de5-96a4-420f-917a-679bcb25da88.png)

   - **3.3 ทำการเปิด CT**
   
      ![image](https://user-images.githubusercontent.com/115439255/208171796-4cbfa4d4-471a-490b-b1bb-6b9c28b09338.png)

   - **3.4 summary ของ CT**
   
      ![image](https://user-images.githubusercontent.com/115439255/208171978-15dec9de-7d52-4beb-8440-cd7f5963a7c1.png)




 


   
   

  
   

  
