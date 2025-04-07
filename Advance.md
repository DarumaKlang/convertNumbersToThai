# Advance

หากคุณต้องการแก้ไขเฉพาะบางส่วนของหน้าเว็บ คุณสามารถปรับปรุงสคริปต์ให้เลือกเฉพาะส่วนที่คุณต้องการแปลงตัวเลขได้ โดยมีหลายวิธีในการทำเช่นนี้ ขึ้นอยู่กับโครงสร้าง HTML ของหน้าเว็บของคุณ

**วิธีที่ 1 : เลือก Element ด้วย ID**

หากส่วนที่คุณต้องการแปลงมี ID ที่เฉพาะเจาะจง คุณสามารถแก้ไขสคริปต์ให้เลือกเฉพาะ Element นั้นได้

```Java
function convertNumbersToThai(elementId) {
  const thaiDigits = ["๐", "๑", "๒", "๓", "๔", "๕", "๖", "๗", "๘", "๙"];
  const numberRegex = /\d+/g;

  const targetElement = document.getElementById(elementId);

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
      for (let i = 0; i < node.childNodes.length; i++) {
        replaceNumbers(node.childNodes[i]);
      }
    }
  }

  if (targetElement) {
    replaceNumbers(targetElement);
  } else {
    console.error(`ไม่พบ Element ที่มี ID: ${elementId}`);
  }
}

// เรียกฟังก์ชันโดยระบุ ID ของ Element ที่ต้องการแปลง
document.addEventListener('DOMContentLoaded', () => {
  convertNumbersToThai('my-content'); // เปลี่ยน 'my-content' เป็น ID จริงของ Element
});
```

**วิธีใช้งาน :**

1. ให้ Element ที่คุณต้องการแปลงมี ID ที่ไม่ซ้ำกัน เช่น `<div id="my-content">...</div>`
2. แก้ไขการเรียกฟังก์ชัน `convertNumbersToThai('my-content');` โดยเปลี่ยน `'my-content'` เป็น ID จริงของ Element นั้น

**วิธีที่ 2 : เลือก Element ด้วย Class Name**

หากส่วนที่คุณต้องการแปลงมี Class Name ที่เหมือนกัน คุณสามารถเลือก Element เหล่านั้นได้

```Java
function convertNumbersInClassToThai(className) {
  const thaiDigits = ["๐", "๑", "๒", "๓", "๔", "๕", "๖", "๗", "๘", "๙"];
  const numberRegex = /\d+/g;
  const targetElements = document.getElementsByClassName(className);

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
      for (let i = 0; i < node.childNodes.length; i++) {
        replaceNumbers(node.childNodes[i]);
      }
    }
  }

  for (let i = 0; i < targetElements.length; i++) {
    replaceNumbers(targetElements[i]);
  }
}

// เรียกฟังก์ชันโดยระบุ Class Name ของ Elements ที่ต้องการแปลง
document.addEventListener('DOMContentLoaded', () => {
  convertNumbersInClassToThai('convert-thai-numbers'); // เปลี่ยน 'convert-thai-numbers' เป็น Class Name จริง
});
```

**วิธีใช้งาน :**

1. ให้ Elements ที่คุณต้องการแปลงมี Class Name ที่เหมือนกัน เช่น `<div class="convert-thai-numbers">...</div>` หรือ `<span class="convert-thai-numbers">...</span>`
2. แก้ไขการเรียกฟังก์ชัน `convertNumbersInClassToThai('convert-thai-numbers');` โดยเปลี่ยน `'convert-thai-numbers'` เป็น Class Name จริง

**วิธีที่ 3 : เลือก Element ด้วย CSS Selector**

คุณสามารถใช้ CSS Selector ที่มีความซับซ้อนมากขึ้นเพื่อเลือก Element ที่ต้องการได้ เช่น เลือก Element ที่มี Class Name หนึ่ง และอยู่ในอีก Element หนึ่ง

```Java
function convertNumbersWithSelectorToThai(selector) {
  const thaiDigits = ["๐", "๑", "๒", "๓", "๔", "๕", "๖", "๗", "๘", "๙"];
  const numberRegex = /\d+/g;
  const targetElements = document.querySelectorAll(selector);

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
      for (let i = 0; i < node.childNodes.length; i++) {
        replaceNumbers(node.childNodes[i]);
      }
    }
  }

  targetElements.forEach(element => {
    replaceNumbers(element);
  });
}

// เรียกฟังก์ชันโดยระบุ CSS Selector ของ Elements ที่ต้องการแปลง
document.addEventListener('DOMContentLoaded', () => {
  convertNumbersWithSelectorToThai('.section-content p'); // เลือก <p> ภายใน Element ที่มี Class 'section-content'
});
```

**วิธีใช้งาน :**

1. กำหนด CSS Selector ที่จะเลือก Element ที่คุณต้องการแปลง (เช่น `.my-container`, `#specific-part h2`, `.item .price`)
2. แก้ไขการเรียกฟังก์ชัน `convertNumbersWithSelectorToThai('.section-content p');` โดยเปลี่ยน `'.section-content p'` เป็น CSS Selector ที่คุณต้องการ

## การตัดสินใจเลือกวิธี :
- ใช้ ID หากคุณต้องการแปลงตัวเลขใน Element ที่มี ID เดียวเท่านั้น
- ใช้ Class Name หากคุณต้องการแปลงตัวเลขในหลาย Elements ที่มีสไตล์หรือความหมายเดียวกัน
- ใช้ CSS Selector หากคุณต้องการเลือก Element ที่มีความซับซ้อนกว่า ID หรือ Class Name เช่น เลือกตามลำดับชั้น `(parent-child)` หรือ `attribute` อื่นๆ
หลังจากที่คุณเลือกวิธีที่เหมาะสมกับโครงสร้าง HTML ของคุณแล้ว ให้แก้ไขโค้ด JavaScript ตามตัวอย่างและนำไปใช้งานในหน้าเว็บของคุณ
