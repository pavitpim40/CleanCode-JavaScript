# CleanCode-JavaScript

## สารบัญ

1. [Introduction](#introduction)
2. [Variables](#variables)
3. [Functions](#functions)
4. [Objects and Data Structures](#objects-and-data-structures)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)

## Introduction

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

หลักการ Clean Code สร้างขึ้นจากพื้นฐาน 3R  
1.Readable : อ่านทำความเข้าใจง่าย เช่น ชื่อตัวแปรสื่อความหมาย ,โค้ดมีรูปแบบเดียวกันสม่ำเสมอ  
2.Resuable : สามารถใช้โค้ดซ้ำได้ เช่น ไม่ต้องเขียนโมดูลบางอย่าใหม่หมด , ฟังก์ชันมีขนาดเล็ก , มีการหลีกเลี่ยง Side effect  
3.Refactorable : สามารถแก้ไขแแยกส่วนโค้ดได้ โดยไม่ส่งผลกับโปรแกรมใหญ่ทั้งหมด


อีกสิ่งหนึ่งคืออย่าพยายามทำให้สมบูรณ์แบบตั้งแต่ในทีแรก  
อย่าเร่งรีบเอาชนะตัวเองด้วยโค้ดที่สมบูรณ์แบบ เปลี่ยนมาเอาชนะโค้ดทีละเล็กทีละน้อยแทน  

## **Variables**

### ใช้ที่ตัวแปรที่สือความหมายชัดเจน

**Bad:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Good:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ back to top](#table-of-contents)**  

### Use the same vocabulary for the same type of variable

**Bad:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good:**

```javascript
getUser();
```

**[⬆ back to top](#table-of-contents)**

### ใช้ชื่อตัวแปรที่สามารถค้นหาได้

หากเราไม่ตั้งชื่อตัวแปรใสามารถห้ค้นหาได้จะทำให้เพื่อนร่วมงานหรือคนรีวิวโค้ดนั้นทำงานต่อจากเราปวดเศียรเวียนเกล้า  มีเครื่องมือจำพวก 
[buddy.js](https://github.com/danielstjules/buddy.js) และ
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
ที่สามารถช่วยให้เราระบุตัวแปรค่าคงที่ที่ยังไม่ถูกตั้งชื่อได้

**Bad:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**Good:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86_400_000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ back to top](#table-of-contents)**  

### Use explanatory variables

**Bad:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Good:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ back to top](#table-of-contents)**

### หลีกเลี่ยงการ Map โดยชือตัวแปรที่คนอ่านต้องไปนึกต่อเอาเอง

ตั้งชื่อให้ชัดเจนดีกว่าย่อแบบมีนัย

**Bad:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**Good:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ back to top](#table-of-contents)**