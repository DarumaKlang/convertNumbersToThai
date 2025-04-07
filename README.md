# convertNumbersToThai
Script JavaScript `thai_number_converter.js`

```java
function convertNumbersToThai() {
  const thaiDigits = ["๐", "๑", "๒", "๓", "๔", "๕", "๖", "๗", "๘", "๙"];
  const numberRegex = /\d+/g; // Regular expression เพื่อค้นหาตัวเลข

  function replaceNumbers(node) {
    if (node.nodeType === Node.TEXT_NODE) {
      node.nodeValue = node.nodeValue.replace(numberRegex, (match) => {
        let thaiNumber = "";
        for (let i = 0; i < match.length; i++) {
          thaiNumber += thaiDigits[parseInt(match[i])];
        }
        return thaiNumber;
      });
    } else if (node.nodeType === Node.ELEMENT_NODE) {
      // ตรวจสอบ attribute ที่อาจมีตัวเลข (เช่น alt, title)
      if (node.alt) {
        node.alt = node.alt.replace(numberRegex, (match) => {
          let thaiNumber = "";
          for (let i = 0; i < match.length; i++) {
            thaiNumber += thaiDigits[parseInt(match[i])];
          }
          return thaiNumber;
        });
      }
      if (node.title) {
        node.title = node.title.replace(numberRegex, (match) => {
          let thaiNumber = "";
          for (let i = 0; i < match.length; i++) {
            thaiNumber += thaiDigits[parseInt(match[i])];
          }
          return thaiNumber;
        });
      }
      // เรียกฟังก์ชันนี้ซ้ำสำหรับลูก node ทั้งหมด
      for (let i = 0; i < node.childNodes.length; i++) {
        replaceNumbers(node.childNodes[i]);
      }
    }
  }

  // เริ่มต้นการแปลงจาก body ของเอกสาร
  replaceNumbers(document.body);
}

// เรียกฟังก์ชันเมื่อ DOM โหลดเสร็จแล้ว
document.addEventListener('DOMContentLoaded', convertNumbersToThai);
```

## วิธีการใช้งาน :

1. คัดลอกโค้ด : คัดลอกโค้ด JavaScript ด้านบนทั้งหมด
2. เพิ่มลงในหน้าเว็บ -> คุณสามารถเพิ่มโค้ดนี้ได้ 2 วิธีหลักๆ :
  - แทรกใน <script> tag : วางโค้ดภายใน <script> tag ที่ส่วนท้ายของ <body> หรือใน <head> ของหน้า HTML ของคุณ :
    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
      <title>หน้าเว็บของฉัน</title>
    </head>
    <body>
      <h1>สินค้า 123 รายการ</h1>
      <p>ราคา: 456 บาท</p>
    
      <script>
        function convertNumbersToThai() {
          // ... โค้ด JavaScript ที่คัดลอกมา ...
        }
    
        document.addEventListener('DOMContentLoaded', convertNumbersToThai);
      </script>
    </body>
    </html>
    ```
    
  - สร้างไฟล์ JavaScript ภายนอก : บันทึกโค้ดเป็นไฟล์ .js เช่น `thai_number_converter.js` ลิงก์ไฟล์ JavaScript นี้ในส่วน `<head>` หรือก่อนปิด `<body>` ของหน้า HTML :
    
    ```html
    <!DOCTYPE html>
    <html>
    <head>
      <title>หน้าเว็บของฉัน</title>
      <script src="thai_number_converter.js"></script>
    </head>
    <body>
      <h1>สินค้า 123 รายการ</h1>
      <p>ราคา: 456 บาท</p>
    </body>
    </html>
    ```

**คำอธิบายการทำงานของสคริปต์ :**
1. **thaiDigits Array** : เก็บตัวเลขไทยตั้งแต่ ๐ ถึง ๙
2. **numberRegex** : `Regular expression /\d+/g` ใช้เพื่อค้นหาตัวเลขที่เป็นชุด (หนึ่งตัวขึ้นไป) ทั่วทั้งข้อความ
3. **replaceNumbers(node) Function** :
  - ตรวจสอบ Text Node : ถ้า `node` เป็น `Text Node` (ส่วนของข้อความ) จะทำการแทนที่ตัวเลขที่พบด้วยตัวเลขไทย
  - ตรวจสอบ Element Node : ถ้า `node` เป็น `Element Node` (เช่น `<h1>`, `<p>`, `<span>`) จะตรวจสอบ `attribute alt` และ `title` ด้วย (เนื่องจากบางครั้งตัวเลขอาจอยู่ใน `attribute` เหล่านี้) และเรียกฟังก์ชัน `replaceNumbers` เองซ้ำ (recursion) สำหรับลูก `node` ทั้งหมดของ `element` นี้ เพื่อให้ครอบคลุมทุกส่วนของ `DOM`
  - การแทนที่ตัวเลข : เมื่อพบตัวเลข (ผ่าน `match` ใน `replace`) จะทำการวนลูปแต่ละหลักของตัวเลขนั้น และแปลงเป็นตัวเลขไทยจาก `thaiDigits array`
4. **document.addEventListener(`'DOMContentLoaded'`, `convertNumbersToThai`);** : บรรทัดนี้ทำให้แน่ใจว่าฟังก์ชัน `convertNumbersToThai` จะถูกเรียกใช้หลังจากที่ DOM (Document Object Model) ของหน้าเว็บโหลดและพร้อมใช้งานแล้ว เพื่อให้สคริปต์สามารถเข้าถึงและแก้ไขเนื้อหาทั้งหมดได้อย่างถูกต้อง

**ข้อควรระวัง :**
- สคริปต์นี้จะแปลงตัวเลขทั้งหมดที่พบในหน้าเว็บ ซึ่งอาจรวมถึงตัวเลขที่ไม่ต้องการแปลง เช่น ตัวเลขใน URL, ตัวเลขในโค้ด JavaScript อื่นๆ (ถ้ามีอยู่ในหน้าเดียวกัน), หรือตัวเลขในรูปแบบ วันที่/เวลา **ที่คุณอาจต้องการให้แสดงเป็นเลขอารบิก**
- ประสิทธิภาพ : หากหน้าเว็บของคุณมีเนื้อหาจำนวนมาก การวนลูปและตรวจสอบทุก `node` อาจส่งผลต่อประสิทธิภาพเล็กน้อย
- การเปลี่ยนแปลงแบบไดนามิก : หากเนื้อหาของหน้าเว็บมีการเปลี่ยนแปลงแบบไดนามิก (เช่น ผ่าน **AJAX** หรือ **JavaScript** อื่นๆ) คุณอาจต้องเรียกใช้ฟังก์ชัน `convertNumbersToThai` อีกครั้งหลังจากที่เนื้อหาส่วนนั้นถูกโหลดหรืออัปเดตแล้ว
- หากคุณมีข้อกำหนดที่เฉพาะเจาะจงมากขึ้น (เช่น ต้องการแปลงเฉพาะตัวเลขในบางส่วนของหน้าเว็บ หรือยกเว้นบางส่วน) คุณอาจต้องปรับปรุงสคริปต์ให้ตรงกับความต้องการของคุณ
