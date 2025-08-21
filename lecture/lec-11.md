# การดึง API มาใช้ใน Frontend
link lec พี่ไมค์ [ep.11](https://docs.mikelopster.dev/c/web101/chapter-11/intro)

![alt text](./img/mamp.png)

## จาก fetch() เปลี่ยนมาใช้ axios
1. install [axios](https://www.npmjs.com/package/axios#cdn) ไป cop link cdn มาวาง
2. ที่ไฟล์ index.html วาง link
```html
<!-- JS -->
     <script src="https://cdn.jsdelivr.net/npm/axios@1.6.7/dist/axios.min.js"></script> //cdn
     <script src="index.js"></script>
    </body>
```
3. ลองใช้คำสั่ง ```axios``` ที่ console ถ้าถูกจะได้

![alt text](./img/axios.png)

## ลองส่งข้อมูลใน from (index.html)
### html
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>Register from </title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="style.css">
    </head>

    <body>
    <a href="user.html">go to user management</a>
    
    <div class="container">
        <div class="header from-input emoji"> <!-- มี 2 class แบบนี้ได้ เว้นด้วยเคาะ -->
            Register from 
        </div>

        <div class="flex from-input">
            <div class="header-from" >  
                first name 
            </div> 
            <input class="from" type="text" name="firstname">
        </div>
        <div class="flex from-input">
            <div class="header-from"> 
                last name 
            </div>
            <input class="from" type="text" name="lastname">
        </div>
        <div class="flex from-input">
            <div class="header-from">
                age
            </div> 
            <input class="from" type="number" min ="0" max="100" name="age">
        </div>

    
        <div class="gender flex from-input">
            <div class="header-from"> gender </div>
            <div class="flex">
                <div>
                    <input name="gender" type="radio" value="male"> male
                </div>
                <div>
                    <input name="gender" type="radio" value="female"> female
                </div>
            </div>
        </div>
    
        <div class="interest from-input">
            interest
            <div>
                <input class="interest" name="interest" type="checkbox" value="coding"> coding
            </div>
            <div>
                <input class="interest" name="interest" type="checkbox" value="drawing"> drawing
            </div>
            <div>
                <input class="interest" name="interest" type="checkbox" value="baking"> baking
            </div>
        </div>
    
        <div class="description from-input">
            identity description
            <div>
                <textarea name="description" id=""> type here </textarea>
            </div>
        </div>
        
       <div class="link from-input center">
        <a  class="link" href="https://picsum.photos/">Lorem Picsum</a>
       </div>
        
       <!--message-->
       <div class="message" id="message">
    
       </div>

        <!--ปุ่มส่ง-->
        <div class="center">
            <button class="button" onclick="submitData()">submit</button>
        </div>
        
    </div>
    <!-- JS -->
     <script src="https://cdn.jsdelivr.net/npm/axios@1.6.7/dist/axios.min.js"></script> <!--เพิ่มการเชื่อม axios-->
     <script src="index.js"></script>
    </body>
</html>
```

### js
```js
const BASE_URL = 'http://localhost:8000'

const submitData = async() => {
    //เข้าถึงทุกฟิลด์
    let firstnameDom = document.querySelector('input[name=firstname]')
    let lastnameDom = document.querySelector('input[name=lastname]')
    let ageDom = document.querySelector('input[name=age]')

    let genderDom = document.querySelector('input[name=gender]:checked')
    let interestDoms = document.querySelectorAll('input[name=interest]:checked')

    let descriptionDom = document.querySelector('textarea[name=description]')

    let interest = ''
    for (let index = 0; index < interestDoms.length; index++) {
        interest += interestDoms[index].value 
        if(index != interestDoms.length -1){ //จะใส่ , ถ้าไม่ใช่ตัวสุดท้าย
           interest +=  ' , '
        }
    }

    //สร้าง obj มาเก็บค่าเพื่อส่งให้ backend
    let userData = {
        firstname: firstnameDom.value ,
        lastname: lastnameDom.value ,
        age: ageDom.value ,
        gender: genderDom.value ,
        description: descriptionDom.value ,
        interrests: interest
    }

    console.log('submit data' , userData)
    const response = await axios.post(`${BASE_URL}/users`, userData) //ยิง userData ไปเก็บที่ MySQl เหมือนที่ลองยิงใน postman แล้วเก็บผลลัพธ์

    console.log('response', response.data)
    
}
```

**result**
![alt text](/img/result-from.png)

## จัดการ error จากหลังบ้าน มาแสดงหน้าบ้าน
```js
//สร้าง obj มาเก็บค่าเพื่อส่งให้ backend
    let userData = {
        firstname: firstnameDom.value ,
        lastname: lastnameDom.value ,
        age: ageDom.value ,
        gender: genderDom.value ,
        description: descriptionDom.value ,
        interrest: interest //error เพราะสะกดไม่ตรงกับใน MySQL
    }
```
* แสดงข้อความให้ user รู้
* จากไฟล์ index.js เดิม ให้เพิ่ม

```js
console.log('submit data' , userData)
    try {
        const response = await axios.post(`${BASE_URL}/users`, userData) //ยิง userData ไปเก็บที่ MySQl เหมือนที่ลองยิงใน postman แล้วเก็บผลลัพธ์
        console.log('response', response.data)

        messageDom.innerText = 'submit success'
        messageDom.className = 'message success'
    } catch (error) {
        if (error.response) {
            console.log(error.response.data.message)
        }

        messageDom.innerText = 'submit error'
        messageDom.className = 'message danger'
    }
```
* เพิ่มที่ index.html
```html
<!--message-->
       <div class="message" id="message">
    
       </div>
```

* เพิ่มที่ css
```css
.message {
    background-color: lightgray;
    padding: 10px;
    margin-bottom:10px;
    display: none; /*ให้ปิดไว้ก่อน*/
}

.message.success{
    background-color: lightgreen;
    display: block; /*ให้แสดงเวลามี class*/
}

.message.danger{
    background-color: lightcoral;
    display: block; /*ให้แสดงเวลามี class*/
}
```

**result error**
![alt text](/img/submit-error.png)


**result success**
![alt text](/img/submit-success.png)


## handle Error จากฝั่งหน้าบ้าน
* เมื่อ error เกิดที่หน้าบ้าน(กรอกข้อมูลไม่ครบ) จะแสดงอย่างไร

```js
const BASE_URL = 'http://localhost:8000'

/*1. สร้าง validateData () มาเพื่อเช็คว่ากรอกข้อมูลครบไหม*/
const validateData = (userData) => {
    let errors = []

    if (!userData.firstname) {
        errors.push('input firstname')
    }
    if (!userData.lastname) {
        errors.push('input lastname')
    }
    if (!userData.age) {
        errors.push('input age')
    }
    if (!userData.gender) {
        errors.push('input gender')
    }
    if (!userData.interrests) {
        errors.push('input interests')
    }
    if (!userData.description) {
        errors.push('input description')
    }

    return errors //errors ได้มาเป็น array string
}
/*-------------------------------------*/

const submitData = async () => {
    //เข้าถึงทุกฟิลด์
    let firstnameDom = document.querySelector('input[name=firstname]')
    let lastnameDom = document.querySelector('input[name=lastname]')
    let ageDom = document.querySelector('input[name=age]')

    let genderDom = document.querySelector('input[name=gender]:checked') || {}
    let interestDoms = document.querySelectorAll('input[name=interest]:checked') || {}

    let descriptionDom = document.querySelector('textarea[name=description]')

    let messageDom = document.getElementById('message')


    try {
        //สร้าง list ของ interest
        let interest = ''
        for (let index = 0; index < interestDoms.length; index++) {
            interest += interestDoms[index].value
            if (index != interestDoms.length - 1) { //จะใส่ , ถ้าไม่ใช่ตัวสุดท้าย
                interest += ' , '
            }
        }

        //สร้าง obj มาเก็บค่าเพื่อส่งให้ backend
        let userData = {
            firstname: firstnameDom.value,
            lastname: lastnameDom.value,
            age: ageDom.value,
            gender: genderDom.value,
            description: descriptionDom.value,
            interrests: interest
        }

        console.log('submit data', userData)
        const errors = validateData(userData) //ส่ง userData ไปเช็ค

        // เช็คว่ามี error โดย errors.length > 0
        if (errors.length > 0) {
            throw { //throw อกกไปที่ catch
                message: 'missing data',
                errors: errors
            }
        }

        const response = await axios.post(`${BASE_URL}/users`, userData) //ยิง userData ไปเก็บที่ MySQl เหมือนที่ลองยิงใน postman แล้วเก็บผลลัพธ์
        console.log('response', response.data)

        messageDom.innerText = 'submit success'
        messageDom.className = 'message success'
    } catch (error) {
        console.log('error message:', error.message)
        console.log('error', error.errors) //errors คือตัวแปรที่เป็นค่าที่ได้จาก validateData()
   

        /*2. แสดง error*/
        let htmlData = '<div>'
        htmlData += `<div>${error.message}</div>`

        htmlData += '<ul>'
        for (let index = 0; index < error.errors.length; index++) {
            htmlData += `<li>${error.errors[index]}</li>` //วนต่อ list errors
        }
        htmlData += '</ul>'
        htmlData += '</div>'

        messageDom.innerHTML = htmlData
        messageDom.className = 'message danger'
        /*----------------------------*/
    }

}
```
**result**
![alt text](/img/errors.png)

## handle Error จากฝั่งหลังบ้าน
* ให้หลังบ้านเป้นคนตรวจ แล้วหน้าบ้านแสดงผลลัพธ์

### ที่ server 
1. ตรง path  = POST 
```js
// path  = POST /users สำหรับการสร้าง users ใหม่บันทึกเข้าไป
app.post('/users', async (req, res) => {
  try {
    const userData = req.body    
    const errors = validateData(userData)
    if (errors.length > 0){
      throw {
        message: 'missing data' ,
        errors: errors
      }
    }                     
    const results  = await conn.query('INSERT INTO user SET ?', userData) //ใส่ ? เพื่อจะได้ใส่ userData

    res.json({ 
      message: 'insert ok', 
      data: results[0] 
    })

  } catch (error) {
    const errorMassage = error.message || 'somthing wrong'
    const errors = error.errors || []

    console.error('Error message', error.message)
    res.status(500).json({
      message: errorMassage ,
      errors: errors
    })
  }
})
```
2. เพิ่ม validateData()
```js
/*---------------------------------*/
const validateData = (userData) => {
    let errors = []

    if (!userData.firstname) {
        errors.push('input firstname')
    }
    if (!userData.lastname) {
        errors.push('input lastname')
    }
    if (!userData.age) {
        errors.push('input age')
    }
    if (!userData.gender) {
        errors.push('input gender')
    }
    if (!userData.interrests) {
        errors.push('input interests')
    }
    if (!userData.description) {
        errors.push('input description')
    }

    return errors
}
/*---------------------------------*/
```
### index.js
1. ปิดการ validateData
```js
/*const errors = validateData(userData)
        if (errors.length > 0) {
            // มี error
            throw {
                message: 'missing data',
                errors: errors
            }
        }*/
```

2. ตรง catch เพิ่ม
```js
catch (error) {
        console.log('error message:', error.message)
        console.log('error', error.errors)

        /*------------------------------*/
        if (error.response) { //response คือ error ที่หลังบ้านส่งมาให้
            console.log(error.response)
            error.message = error.response.data.message
            error.errors = error.response.data.errors
        }
        /*------------------------------*/
        
        let htmlData = '<div>'
        htmlData += `<div>${error.message}</div>`
        htmlData += '<ul>'
        for (let index = 0; index < error.errors.length; index++) {
            htmlData += `<li>${error.errors[index]}</li>`
        }
        htmlData += '</ul>'
        htmlData += '</div>'

        messageDom.innerHTML = htmlData
        messageDom.className = 'message danger'
    }
```
**result**
* error.message = error.response.data.message
* error.errors = error.response.data.errors

![alt text](/img/error-message.png)

## สน้างหน้าจัดการ user.js , user.html
* ในหน้านี้จะแสงข้อมูล user 
* มีปุ่มลบ
* มีปุ่มแก้ไข

### แสงข้อมูล user
* user.js
```js
const BASE_URL = 'http://localhost:8000'

window.onload = async () => {
    await loadData()
}

const loadData = async () => {
    console.log('on load')
    // Load user ทั้งหมดออกจาก API
    const response = await axios.get(`${BASE_URL}/users`)
    console.log(response.data)
}
```

* user.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <a href="index.html">Back to register</a>
    User Management
    <div id="user"></div>

    <script src="https://cdn.jsdelivr.net/npm/axios@1.6.7/dist/axios.min.js"></script>
    <script src="user.js"></script>
</body>
</html>
```

**result**
![alt text](/img/image.png)

### ปุ่มลบ
```js
const BASE_URL = 'http://localhost:8000'

window.onload = async () => {
    await loadData()
}

const loadData = async () => {
    console.log('on load')
    // Load user ทั้งหมดออกจาก API
    const response = await axios.get(`${BASE_URL}/users`)
    console.log(response.data)

    //นำ user ที่โหลดมาใส่กลับไปที่ html
    const userDOM = document.getElementById('user')
    let htmlData = '<div>'
    for (let index = 0; index < response.data.length; index++) {
        let user = response.data[index]
        htmlData += `<div>
        ${user.id} ${user.firstname} ${user.lastname}
        <button class='delete' data-id='${user.id}'>Delete</button> 
        </div>` //data คือ data attribute ในที่นี้จะเก็บค่า id

    }
    htmlData += '</div>'
    userDOM.innerHTML = htmlData 

    //การทำงานของปุ่ม delete 
    const deleteDom = document.getElementsByClassName('delete')

    for (let index = 0; index < deleteDom.length; index++) {
        deleteDom[index].addEventListener('click', async (event) => {
            // ดึง id  ออกมา
            const id = event.target.dataset.id //dataset.id เป้นการเข้าถึง data attribut
            try {
                await axios.delete(`${BASE_URL}/users/${id}`) //ยิงไปลบ
                loadData() //แล้วโหลดหน้าซ้ำ
            } catch (error) {
                console.log('error', error)
            }
        })

    }
}
```
### ปุ่มแก้ไข
* เมื่อกดปุ่ม edit จะกลับมาหน้า index แบบ query param ```<a href='index.html?id=${user.id}'><button>Edit</button></a>``` 

    ผลลัพธ์ ```index.html?id=1```

* ที่ user.js
```js
//เพิ่ม
<a href='index.html?id=${user.id}'><button>Edit</button></a> 
<button class='delete' data-id='${user.id}'>Delete</button> 
```

**result**
![alt text](/img/image-1.png)


* เมื่อกลับมาหน้า index ก็จะเปลี่ยน mode CREATE -> EDIT
เพิ่มที่ index.js
```js
const BASE_URL = 'http://localhost:8000'

let mode = 'CREATE' //defaul
let selectedId = ''

window.onload = async () => {
    const urlParams = new URLSearchParams(window.location.search)
    // ดึง id ออกมาจาก parameter
    const id = urlParams.get('id')
    console.log('id:', id)

    if (id) {
        // ถ้ามี id = เปลี่ยน mode และเก็บตัวแปร id เอาไว้
        mode = 'EDIT'
        selectedId = id

        try {
            const response = await axios.get(`${BASE_URL}/users/${id}`)
            console.log('data', response.data)
        } catch (error) {
            console.log('error', error)
        }
    }
}
```

**result**
![alt text](/img/image-2.png)

### ✅กรอก form ใหม่
```js
const BASE_URL = 'http://localhost:8000'

let mode = 'CREATE' //defaul
let selectedId = ''

window.onload = async () => {
    // นำ parameter ทั้งหมดมาใส่ตัวแปร urlParams
    const urlParams = new URLSearchParams(window.location.search)
    // ดึง id ออกมาจาก parameter
    const id = urlParams.get('id')
    console.log('id: ', id)

    if (id) {
        // ถ้ามี id = เปลี่ยน mode และเก็บตัวแปร id เอาไว้
        mode = 'EDIT'
        selectedId = id

        //1. ดึงข้อมูลเก่าของ user ออกมา
        try {
            const response = await axios.get(`${BASE_URL}/users/${id}`)
            console.log('data', response.data)
            
            const user = response.data

            // นำข้อมูล user กลับใส่เข้าไปที่ input html
            let firstnameDom = document.querySelector('input[name=firstname]')
            let lastnameDom = document.querySelector('input[name=lastname]')
            let ageDom = document.querySelector('input[name=age]')
            let descriptionDom = document.querySelector('textarea[name=description]')

            firstnameDom.value = user.firstname
            lastnameDom.value = user.lastname
            ageDom.value = user.age
            descriptionDom.value = user.description

            let genderDoms = document.querySelectorAll('input[name=gender]') 
            let interestDoms = document.querySelectorAll('input[name=interest]') 

            //วนดูว่าโดนติ้กอันไหน
            for (let index = 0; index < genderDoms.length; index++) {
                if (genderDoms[index].value == user.gender) {
                    genderDoms[index].checked = true
                }
            }

            //วนดูว่าอันไหนบ้างที่ติ๊ก
            for (let index = 0; index < interestDoms.length; index++) {
                if (user.interrests.includes(interestDoms[index].value)) {
                    interestDoms[index].checked = true
                }
            }

        } catch (error) {
            console.log('error', error)
        }
    }
}

const validateData = (userData) => {
    let errors = []

    if (!userData.firstname) {
        errors.push('input firstname')
    }
    if (!userData.lastname) {
        errors.push('input lastname')
    }
    if (!userData.age) {
        errors.push('input age')
    }
    if (!userData.gender) {
        errors.push('input gender')
    }
    if (!userData.interrests) {
        errors.push('input interests')
    }
    if (!userData.description) {
        errors.push('input description')
    }

    return errors
}

const submitData = async () => {
    //เข้าถึงทุกฟิลด์
    let firstnameDom = document.querySelector('input[name=firstname]')
    let lastnameDom = document.querySelector('input[name=lastname]')
    let ageDom = document.querySelector('input[name=age]')

    let genderDom = document.querySelector('input[name=gender]:checked') || {}
    let interestDoms = document.querySelectorAll('input[name=interest]:checked') || {}

    let descriptionDom = document.querySelector('textarea[name=description]')

    let messageDom = document.getElementById('message')

    try {
        let interest = ''
        for (let index = 0; index < interestDoms.length; index++) {
            interest += interestDoms[index].value
            if (index != interestDoms.length - 1) { //จะใส่ , ถ้าไม่ใช่ตัวสุดท้าย
                interest += ' , '
            }
        }

        //สร้าง obj มาเก็บค่าเพื่อส่งให้ backend
        //สะกด key ดีๆ ต้องตรงกับ field Table
        let userData = {
            firstname: firstnameDom.value,
            lastname: lastnameDom.value,
            age: ageDom.value,
            gender: genderDom.value,
            description: descriptionDom.value,
            interrests: interest
        }
        console.log('submit data', userData)

        const errors = validateData(userData)
        if (errors.length > 0) {
            // มี error
            throw {
                message: 'missing data',
                errors: errors
            }
        }

        //axios ต่อหน้าบ้าน หลังบ้าน
        let message = 'submit success'

        if (mode == 'CREATE') {
            const response = await axios.post(`${BASE_URL}/users`, userData)
            console.log('response', response.data)
        } else {
            const response = await axios.put(`${BASE_URL}/users/${selectedId}`, userData)
            message = 'edit success'
            console.log('response', response.data)
        }

        messageDom.innerText = message 
        messageDom.className = 'message success'

    } catch (error) {
        console.log('error message:', error.message)
        console.log('error', error.errors)
        if (error.response) {
            console.log(error.response)
            error.message = error.response.data.message
            error.errors = error.response.data.errors
        }

        let htmlData = '<div>'
        htmlData += `<div>${error.message}</div>`
        htmlData += '<ul>'
        for (let index = 0; index < error.errors.length; index++) {
            htmlData += `<li>${error.errors[index]}</li>`
        }
        htmlData += '</ul>'
        htmlData += '</div>'

        messageDom.innerHTML = htmlData
        messageDom.className = 'message danger'
    }

}
```

**result**
![alt text](/img/image-3.png)