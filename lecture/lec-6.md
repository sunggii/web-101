# JS DOM

link lec พี่ไมค์ [ep.6](https://docs.mikelopster.dev/c/web101/chapter-6/selectdom)

javascript สามารถสื่อสารกับ html (DOM) ปกติเราจะแบ่งไอเดียง่ายๆออกเป็น 2 แบบคือ

* javascript access ไปยัง html (DOM)
* html ส่ง Event ไปยัง javascript

## Select DOM มี 4 แบบ
* ไอเดียของ Select DOM เหมือนกับ Selector CSS คือ
    1. เลือกใคร (html element ตัวไหน)
    2. ทำอะไร (เช่น ดึงค่าออกมา, แทนค่า)
## 1. getElementById 
**step**
* ```id="firstname" ``` เข้าถึง id 
* สร้างตัวแปรมาเก็บค่าที่ select ```let firstnameDOM = document.getElementById('firstname')```
* ```firstnameDOM.value``` ใช้  ```.value``` เพื่อเข้าถึงค่าใน DOM ออกมา

```html
<body>
    first name: 
    <input 
        id="firstname" 
        type="text" 
        name="firstname" 
        value="ทดสอบ" 
    >
    
    <script>
        console.log('Hello wolrd')

        let firstnameDOM = document.getElementById('firstname')

        // สำหรับแสดง DOM html ออกมา
        console.log('firstname DOM', firstnameDOM)

        // สำหรับการดึงค่า value ออกมา
        console.log('firstname DOM', firstnameDOM.value)
    </script>
</body>
```
**result**

![alt text](img/select-id.png)

## 2. getElementByClassname
**step** เหมือน id 

* ไอเดียเหมือน CSS เราจะใช้ classname ก็ต่อเมื่อเราอยากเข้าถึงทีละหลายๆตัว 

แต่สิ่งที่ได้มาจะเป็น arr ดังนั้นเราต้อง loop เพื่อเข้าถึง value

```html
<body>
    first name: <input class="input" id="firstname" type="text" name="firstname" value="ทดสอบ">
    last name: <input class="input" id="lastname" type="text" name="firstname" value="last">
    
    <script>
        let inputsDOM = document.getElementsByClassName('input')

        // สำหรับแสดง DOM html ออกมา
        console.log(inputsDOM)
        
        // สำหรับการดึงค่า value ออกมา
        for (let i = 0; i < inputsDOM.length; i++) {
            console.log(inputsDOM[i].value)
        }
    </script>
</body>
```

**result**

![alt text](img/select-class.png)

## 3. querySelector จะหยิบตัวเดียว
* select ได้ 3 ท่า ใช้ไอเดีย CSS
    1. selet ไปที่ id (#) 
    ```html
    <body>
    first name: <input class="input" id="firstname" type="text" name="firstname" value="ทดสอบ">
    
    <script>
        let firstnameDOM = document.querySelector('#firstname')
        
        // สำหรับการดึงค่า value ออกมา
        console.log(firstnameDOM.value)
    </script>
    </body>
    ```
    2. selet ไปที่ class (.) จะไม่ออกมาเป็น arry เหมือน ```getElementByClassname```
    ```html
    <body>
    first name: <input class="input" id="firstname" type="text" name="firstname" value="ทดสอบ">
    
    <script>
        let firstnameDOM = document.querySelector('.input')
        
        // สำหรับการดึงค่า value ออกมา
        console.log(firstnameDOM.value)
    </script>
    </body>
    ```

    3. selet ไปที่ tag  ชื่อ tag[attribute name] ```.querySelector('input[name=firstname]')```

    **result(1-3)**

    ![alt text](img/query.png)

## 4. querySelectorAll เหมือน querySelecto 
* แต่จะหยิบหลายตัว และผลลัพธ์จะออกมาเป็น arry
```html
interst:
    <input class="input" type="checkbox" name="interst" value="book"> book
    <input class="input" type="checkbox" name="interst" value="code"> code
    <input class="input" type="checkbox" name="interst" value="JS"> JS

    <script>
        //ได้ arr มาเก็บที่ checkboxDOM
        let checkboxDOM = document.querySelectorAll('input[name=interst]')

        // สำหรับการดึงค่า value ออกมา
        for (let i = 0; i < checkboxDOM.length; i++) {
            console.log('checkbox value', checkboxDOM[i].value)
        }
    </script>
```
**result**

![alt text](img/queryAll.png)


## Javascript Event
## 1. onclick 
* การดักจับ event เมื่อ user มีการคลิกที่ element ใน htm

**stepe**
1. ใส่ ``` onclick="userClick()"``` ใน DOM ที่ต้องการดัก
2. ไปสร้าง function ที่ js ให้ชื่อเหมือนกัน ```userClick()```

```html
    <button onclick="userClick()">ทดลองกด</button>
    
    <script>
        // สร้าง function สำหรับการรับ Event ชื่อ userClick และ function นี้จะถูกเรียกเมื่อ onclick ทำงาน
        function userClick() {
            console.log('user clicked')
        }
    </script>
```

## 2. onchange
* การดักจับเมื่อ input มีการเปลี่ยนแปลงค่า (ปกติใช้ได้กับ input อย่าง text, radio, checkbox)
*  radio ต้องใช้ name เดียวกันไม่งั้นจะเลือกได้ม > 1

**stepe** เหมือน onclick

**ตัวอย่าง** การใช้ querySelectorAll กับ onChenge

```html
    <input type="radio" name="gender" value="male" onchange="changeGender()"> male
    <input type="radio" name="gender" value="female" onchange="changeGender()"> female

    <script>
        function changeGender() {
            let genderInputs = document.querySelectorAll('input[name=gender]')
            let gender = ''

            //ใช้ for เพื่อวนเข้าถึงข้อมูล เพราะ querySelectorAll จะได้ของออกมาเป็น array
            for (let index = 0; index < genderInputs.length; index++) {
                //if check ว่าใครถูกติ้ก ถ้าถูกติ้ก ก็จะเอาไปเก็บที่ gender
                if (genderInputs[index].checked) {
                    gender = genderInputs[index].value
                } 
            }

            console.log('selected gender', gender) //ดูค่า
        }
    </script>
```

## 3. onkeydown, onkeyup, onkeypress
    * onkeydown = เมื่อ user กดลงบน keyboard (จังหวะกด)
    * onkeyup = เมื่อ user กดปล่อยมือจากปุ่ม keyboard (จังหวะปล่อยหลังกด) จะได้ค่าที่อัปเดทแล้ว
    * onkeypress = จังหวะที่ตัวอักษรถูกส่งออกมา (เกิดหลัง onkeydown, เกิดก่อน onkeyup)

**ตัวอย่าง** การใช้ 

```html
<body>
    first name <input type="text" name="firstname" onkeyup="changeFirstname()">
    <script>
        function changeFirstname() {
            //เข้าถึง input firstname
            let firstNameDom = document.querySelector('input[name = firstname]')
            //เอาค่า firstname ออกมา
            console.log('change first name:', firstNameDom.value)
        }
    </script>
</body>
```

## 4. onmouseover, onmouseup, onmousedown 

**ตัวอย่าง** การใช้ 
```html
    <div 
    onmouseover="overItem()"
    onmouseup="upItem()"
    onmousedown="dowItem()"
    >click</div>

    <script>
        function overItem() {
            console.log('mouse over')
        }

        function upItem() {
            console.log('mouse up')
        }

        function dowItem() {
            console.log('mouse dow')
        }

    </script>
```
## addEventListener เป็นอีกท่าของ event

* จะได้ผลลัพธ์เหมือน  onclick เลือกใช้ตามที่ถนัด

```html
<button id="testbutton">ปุ่มที่ 1</button>
<button onclick="clickButton()">ปุ่มที่ 2</button>

<script>
document.getElementById('testbutton').addEventListener('click', function() {
  console.log('คลิกปุ่มที่ 1')
})
function clickButton () {
  console.log('คลิกปุ่มที่ 2')
}
</script>
```

## การเข้าถึงและการ update HTML DOM

**step** การ update HTM 

## 1. ทำการดึงข้อความทั้งหมดของ element นั้นออกมา เพื่ออ่าน
* หรืออาจจะใช้ดูทค่า DOM ที่ไม่ได้กำหนด value

มีการอ่าน 3 แบบ 

* textContent
* innerText
* innerHTML ใช้กับการ update

```html
<div id="hello">
        Hello <span style="display: none;">Mike</span>
    </div>

    <script>
        let helloDom = document.getElementById('hello')
        
        console.log(helloDom.textContent) //ดึงข้อความทั้งหมดออกมา
        console.log(helloDom.innerText)   //ดึงข้อความที่ User เห็น
        console.log(helloDom.innerHTML)   //ดึงมาทั้ง html
    </script>
```
**result**

 ![alt text](/img/innertext.png)

## 2. การ update 

**example 1**

```html
    <div id="hello">
        Hello <span style="display: none;">Mike</span>
    </div>

    <script>
        let helloDom = document.getElementById('hello') //เข้าถึง DOM input
        helloDom.innerHTML ='Hello <b>P</b>' //เขียนข้อความใหม่ทับ DOM เดิม
        
        console.log(helloDom.textContent) //ดึงข้อความทั้งหมดออกมา
        console.log(helloDom.innerText)   //ดึงข้อความที่ User เห็น
        console.log(helloDom.innerHTML)   //ดึงมาทั้ง html
    </script>
```

**example 2**

```html
    <div>
        <h2>Interest</h2>
        <div id="content">
            <!--แสดงสิ่งที่ user สนใจ -->
        </div>
    </div>

    <input type="checkbox" name="Interest" value="book"> book
    <input type="checkbox" name="Interest" value="coding"> coding
    <input type="checkbox" name="Interest" value="cooking"> cooking
    <input type="checkbox" name="Interest" value="sport"> sport

    <button onclick="submitInterest()">submit</button>

    <script>
        function submitInterest () {
            //เลือกไปยัง checkbox ทั้งหมด
            let interestDom = document.querySelectorAll('input[name = Interest]')
            let contentHTML = '<ul>'

            for (let index = 0; index < interestDom.length; index++) {
                //if เอาต่า value ออกมาเฉพาะตัวที่เลือก 
                if (interestDom[index].checked) {
                    contentHTML += '<li>' + interestDom[index].value + '</li>' //มัดรวมก้อนนนี้ไปต่อ contentHTML
                } 
            }
            //วนค่าตัวที่เลือก ใส่ html กลับไป
            contentHTML += '</ul>' //จะได้ List ออกมา

            //ใส่ html กลับเข้าไป
            let contentDom = document.getElementById('content') //เข้าถึง div id="content"
            contentDom.innerHTML = contentHTML //เอา list ที่ได้ใส่กลับเข้าไป
        }
    </script>
```

## การอ่านและอัพเดท Attribute ลง html element
* ใช้ ```getAttribute('')``` เพื่อเข้าถึง
* ใช้ ```setAttribute(' ' , ' ')``` เพื่ออัพเดท

**ตัวอย่าง** การ get, set

```html
    <a id="thislink" href="https://google.com" target="_blank">Google</a>

    <script>
        //let thislinkDom = document.getElementById('thislink')
        let thislinkDom = document.querySelector('#thislink')

        //ดูข้อมูลใน att ต่างๆ
        console.log(thislinkDom.getAttribute('id')) 
        console.log(thislinkDom.getAttribute('href'))
        console.log(thislinkDom.getAttribute('target'))
        
                                //เปลี่ยนอะไร , เป็นอะไร
        thislinkDom.setAttribute('href' , 'https://yahoo.com')
  
    </script>
```

**ตัวอย่าง** การใช้ get, set กับ button ให้กดแล้วจะ disabled

```html
    <button id="thisbutton" onclick="submitData()">submit</button>

    <script>
        function submitData() {
            let thisbuttonDOM = document.getElementById('thisbutton')
            thisbuttonDOM.setAttribute('disabled', 'true')
            //thisbuttonDOM.style.backgroundColor = 'red' //.style จะเปลี่ยน css ได้
        }
  
    </script>
```

**ตัวอย่าง** การใช้ตัวแปร ```even```

* ```even``` เป็นตัวแปรที่ชี้ตัวเองว่าเกิดอะไรขึ้นที่ตัวเอง code สั้นลงและได้ผลลัพธ์เหมือนข้างบน

```html
    <button onclick="submitData(event)">submit</button>

    <script>
        function submitData(event) {
            let thisbuttonDOM = event.target
            thisbuttonDOM.style.backgroundColor = 'red' 
            console.log(thisbuttonDOM.style.backgroundColor)
        }
  
    </script>
```
