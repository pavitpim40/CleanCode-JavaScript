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

### ใช้คำศัพท์เดียวกันสำหรับตัวแปรประเภทเดียวกัน

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

### ใช้ชื่อตัวแปรที่อธิบายตัวเองได้

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

### หลีกเลี่ยงการ Map โดยใช้ชื่อตัวแปรที่คนอ่านต้องไปนึกต่อเอาเอง

ตั้งชื่อให้ชัดเจนดีกว่าย่อแบบกำกวม

**Bad:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // เดี๋ยวนะ l คิออะไรนะ
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

### อย่าใช้คำฟุ่มเฟือยโดยไม่จำเป็น

ถ้าชื่อคลาสหรือออปเจ็คบอกอะไรเราบางอย่างแล้ว ไม่ต้องใช้คำคำเดิมซ้ำในบริบทหรือสโคปเดียวกัน

**Bad:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car) {
  car.carColor = "Red";
}
```

**Good:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car) {
  car.color = "Red";
}
```

**[⬆ back to top](#table-of-contents)**  

### ใช้ default arguments แทนการ short circuit หรือการใช้ condition

หากใช้วิธี Defalut argument จะทำให้โค้ดดูคลีนกว่าการทำ Short Circuit  
แต่ใช้ระวังไว้อย่างนึงคือฟังก์ชันจะใช้ค่า default value เมื่อค่า argument เป็น  `undefined` เท่านั้น  
ส่วนค่า falsy อื่นๆเช่น `''`, `""`, `false`, `null`, `0`, และ
`NaN` จะไม่ถูกแทนด้วยค่า default value

**Bad:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Good:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

## **Functions**

### Function arguments (ใช้แค่ 2 ตัวหรือน้อยกว่านั้นได้จะยิ่งดี)

โดยทั่วไปจะให้ฟังก์ชันนึงมีจำนวน argument ไม่เกิน 2 ตัวหรือ 3 ก็ได้แต่ควรเลี่ยง  
เพื่อลดโอกาสการทำงานผิดพลาดของโปรแกรมเมื่อต้องมีการ Test หลายๆเคส

หากต้องการใส่ argument เป็นจำนวนมากแนะนำให้ใช้ object แทนการใช้ function

เราสามารถใช้ ES2015/ES6 ร่วมกับ Destructuring Syntax   
ในการทำให้รู้ว่าฟังก์ชันของเราต้องการ Property ใด  
โดยประโยชน์ของการใช้งานมีดังนี้

1. ถ้ามีใครมาดูตัวฟังก์ชันเรา เขาจะทราบทันทีว่าฟังก์ชันนี้ต้องการ property อะไรบ้าง  
2. สามารถใช้จำลองชื่อของพารามิเตอร์.
3. ช่วย clones ค่า primitive values ของ argument ก่อน pass เข้าฟังก์ชัน  
หลีกเลี่ยง Side Effect 
  
4. Linters ช่วยแจ้งเตือน property ที่ไม่ถูกใช้งานได้ 

**Bad:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Good:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ back to top](#table-of-contents)**  

### ฟังก์ชันควรมีหน้าที่ทำอย่างใดเพียงอย่างเดียวเท่านั้น

นี่น่าจะเป็นกฏข้อที่สำคัญที่สุด  
เมื่อฟังก์ชันทำงานมากกว่าหนึ่งงาน จะทำให้การ compose,การ test ทำได้ยากลำบากยิ่งขึ้น  
หากฟังก์ชันของเราทำเพียงงานใดงานหนึ่ง จะทำให้ให้โค้ดดูคลีนและง่ายต่อการแยกส่วนโค้ด

**Bad:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ back to top](#table-of-contents)**


### ชื่อของฟังก์ชันควรบ่งบอกว่าตัวมันเองทำอะไรได้

**Bad:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Good:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ back to top](#table-of-contents)**

### ฟังก์ชันควรมีระดับความ abstract อยู่แค่หนึ่งระดับ

ถ้าฟังก์ชันมีระดับความ abstract เกินหนึ่งระดับนั่นอาจหมายความว่า  
ฟังก์ชันของคุณอาจจะทำงานหลายอย่างมากเกินไป 
การ spilt ฟังก์ชันออกมาเป็นหลายๆฟังก์ชันจะทำให้การ reuse โค้ดและการ test ทำได้ง่ายกว่า
testing.

**Bad:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**Good:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ back to top](#table-of-contents)**

### กำจัดการใช้โค้ดซ้ำกันหลายที่ (duplicate code)

นึกภาพว่าหากเรามีลิสต์จดไว้หลายที เวลาอัพเดททีก็ต้องตามไปอัพเดททุกที่ที่จดไว้  
การ duplicate โค้ดก็เช่นกัน

บ่อยครั้งการ duplicate code มาจากการที่เราต้องการใช้บางอย่างหลายๆที่ แต่มีปัจจัยที่แตกตต่างกันเล็กน้อย  
แต่กระนั้นเราควรจะเป็นสองฟังก์ชันหรือมากกว่านั้นจะดีกว่า  

การกำจัด duplicate code อาจหมายึงการสร้าง abstract อย่างหนึ่งขึ้นมา เช่น function/module/class
เพือจัดการกับงานในบริบทที่แตกต่างกัน


**Bad:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ back to top](#table-of-contents)**

### ใช้ Object.assign ในการตั้งค่า defualt ของ Object  

**Bad:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Good:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ back to top](#table-of-contents)**  

### อย่าใช้ตัวบอกสถานะ (flag) เป็นฟังก์ชันพารามิเตอร์

หากค่าสถานะเป็นหนึ่งในพารามิเตอร์แสดงว่าฟังก์ชันของคุณทำงานได้มากกว่าหนึ่งอย่าง  
แยกฟังก์ชันของคุณหากต้องการให้โค้ดทำงานต่างกันโดยอ้างอิงจาก Boolean


**Bad:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ back to top](#table-of-contents)**