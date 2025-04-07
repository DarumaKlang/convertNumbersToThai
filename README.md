# convertNumbersToThai
Script JavaScript convertNumbersToThai

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
