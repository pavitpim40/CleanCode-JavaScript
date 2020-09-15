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


### หลีกเลี่ยง Side Effects (พาร์ท 1)

การหลีกเลี่ยง Side Effect อย่างแรกเลยคือการให้ฟังก์ชันแก้ไข Global Variable โดยตรง
ซึ่งอาจทำให้ตัวแปรนั้นมีการเปลี่ยนแปลงประเภทไปและไมสามารถนำไปใช้กับส่วนอื่นๆ ของโปรแกรมได้

**Bad:** ตัวฟังก์ชันแก้ไขค่าของตัวแปร name โดยตรงและให้ผลลัพธ์เป็น array

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Good:** นำตัวแปร newName มารับค่าจากการ spilt แทนทำให้ name ยังคงเป็น string เหมือนเดิม

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ back to top](#table-of-contents)**



### หลีกเลี่ยง Side Effects (part 2)

ไม่ควรแก้ไข Object, Array โดยตรง เนื่องจากเ็นการ passed by Refference  
ตัวอย่างเช่นผู้ใช้งานรายหนึ่งกำลังซื้อของผ่านช่องทางออนไลน์ ได้เพิ่มสินค้าเข้าไป 5 ชิ้น
และกดปุ่มจ่ายเงิน ปรากฏว่าเกิด bad request และระหว่างที่รอ response นั้น  
ลูกค้าเผลอไปกดเพิ่มสินค้าโดยไม่ได้ตั้งใจ ทำให้ผู้ใช้งานต้องจ่ายเงินสินค้า 6 ชิ้น
ทั้งหมดนี้จะเกิดขึ้นหากเรานำ cart ที่เก็บสินค้าไปใช้งานโดยตรงเลย
วิธีแก้คือควรจะ clone cart แยกออกมาเพิ่อส่งไป purchase เพื่อหลีกเลี่ยงการแก้ไขโดยตรง  
กับข้อมูลประเภท Array , Object 

**Bad:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Good:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ back to top](#table-of-contents)**

### อย่าเขียน Global function

ตัวอย่างโค้ดที่ยกมาคือเป็นการเพิ่มเมธอดให้ Array.prototype  
แต่หากคนอืนเอาโค้ดไปรวมอาจจะทำให้ clash เพราะทุกคนก็ใช้ Array.prototype เหมือนกัน  
และอาจจะบังเอิญตั้งชื่อฟังก์ชันที่เพิ่มเข้าไปเหมือนกันด้วย  
กล่าวคืออย่าพยายามไปแก้ไขอะไรที่มัน original ให้ extends ออกมา 

**Bad:** ฟังก์ชัน diff ใน Array.prototype อยู่ใน global Scope

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Good:** ใช้การ Extend Class แทนจะดีกว่า

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ back to top](#table-of-contents)**

### เน้นการเขียนแบบ Functional Programming มากว่า Imperative programming

การเขียนแบบ Imperative คือการอธิบายขั้นตอนการทำงานว่าโค้ดแต่ละบรรทัดมีการทำงานอย่างไร
ส่วนการเขียนแบบ Functional จะเน้นไปที่การประกาศตัวแปรแต่ละตัวว่าเป็อะไรแล้วค่อยประเมิณค่าทีเดียวตอน runtime 

**Bad:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Good:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ back to top](#table-of-contents)**

### ใช้การห่อหุ้มเงื่อนไข Encapsulate conditionals  

หากใส่เงื่อนไขยาวๆลงไปใน conditional statement จะทำให้อ่านยาก  
ให้นำเงื่อนไขที่ต้องการเช็คมาหุ้มด้วยฟังก์ชันแล้วรีเทิร์นค่าออกไปใช้ต่อจะดูอ่านง่ายกว่า
**Bad:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Good:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### หลีกเลี่ยงการใช้ negative conditionals  

ทำให้อ่านทำความเข้าใจยาก ต้องตีความซ้ำซ้อน

**Bad:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Good:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### หลีกเลี่ยงการใช้เงื่อนไข 

แนวคิดนี้อาจจะดูทำตามได้ยาก แต่เรายึดตามหลักที่ว่าหนึ่งฟังก์ชันควรจะทำหน้าที่แค่อย่างเดียวเท่านั้น  
การมีเงื่อนไขนั่นก็หมายความว่าฟังก์ชันเราทำงานได้หลายอย่างโดยปริยาย  
แนวคิดหลักที่เราจะใช้ก็คือ polymorphism ซึ่งก็ืคอการที่ออปเจ็คมีได้หลายรูปแบบ  
เราจะใช้การสร้างออปเจ็คที่มาจาก class ที่มี superclass เดียวกัน  
โดยใน subclass ใหม่ได้มีการกำหนดการทำงานใหม่ให้กับ method เพื่อให้ตรงจุดประสงค์ของ class นั้นๆ

 
**Bad:** ใช้ switch case 

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Good:** extend class ออกมาและเพิ่มเมธอดของใครของมัน

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid type-checking (part 1)

แม้ว่า JS จะเป็นภาษาแบบ Dynamics จนทำให้เราอยากเช็คประเภทของข้อมูลอยู่ตลอดเวลา  
แต่นั้นก็ขัดกับแนวคิดก่อนหน้านั้นว่า ถ้ามีเงื่อนไขฟังก์ชันก็จะทำงานได้มากกว่า 1 แบบ  
วิธีการแรกคือการพิจารณา APIs ที่ใช้อย่างสม่ำเสมอ

**Bad:** ตัดสินใจว่าจะปั่นหรือขับโดยดูว่าพาหนะเป็นชนิดใด

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Good:** เคลื่อนที่ไม่เลยขอแค่เป็นยานพาหนะ

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid type-checking (part 2)

หากเราทำงานกับ string , integer และใช้แนวคิด polymorp ไม่ได้  
แต่ยังต้องการ type checking แนะนำให้ใช้ภาษา TypeScript แทน  
หากเราใช้ JS แล้วต้องมาคอยเช็คประเภทตัวแปรตลอดเวลา จะทำให้เราเขียนโค้ด  
โดยใช้คำฟุ่มเฟือย ไมดูคลีน และสูญเสียความสามารถในการอ่านทำความเข้าใจ
ถ้าจะไม่ใช้ TypeScript ก็ไม่ต้องเช็คไปเลยใน JS ดูคลีนกว่า

**Bad:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**Good:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ back to top](#table-of-contents)**

### อย่าพยายาม optimize จนเกินไป 
โดยสรุปคือ Browser ยุคใหม่ๆค่อนข้างมี tool ที่ชวยเรา optimize มาในระดับหนึ่งแล้ว อ้างอิง [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)

**Bad:** สมัยก่อนไม่สามารถดักจับ list.length ได้ในการ iterate แต่ละรอบ

```javascript 
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good:** ตอนนี้ไม่้องสร้างตัวแปรมารับค่า list.length แล้ว

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### กำจัด dead code

ไม่มีเหตุผลที่จะเก็บ Code ที่ไม่ใช้ไว้  
หากคุณคิดว่ามีความจำเป็นที่ต้องใช้งานมันอีกครั้งให้ใช้ version control แทน

**Bad:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**Good:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ back to top](#table-of-contents)**
