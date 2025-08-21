# Backend Nodejs + Rest API(ep_9)
link lec พี่ไมค์ [ep.7](https://docs.mikelopster.dev/c/web101/chapter-7/intro)

link lec พี่ไมค์ [ep.9](https://docs.mikelopster.dev/c/web101/chapter-9/intro)

API testing ใช้ Github login [postman](https://www.postman.com/)

* ใช้ Node js เพราะเป็นภาษาที่ใช้ได้ทั้ง หน้าและหลังบ้าน

# Node js
## how to run 
1. node

      ```node index.js``` ใช้คำสั่งนี้ที่ powershell ท่ามาตรฐาน

1. nodemon **ใช้ตัวนี้ดีกว่า**

      ```npx nodemon index.js``` ใช้คำสั่งนี้ที่ cmd 
    * nodemon ตือ tool ที่ทำให้ไม่ต้องรันใหม่ เวลาแก้ไฟล์


## how to use POSTMAN 
1. เปิดขึ้นมา
2. ใส่ port
3. เลือก method


## note
* ```npm init``` ใช้ตรง cmd แล้ว enter จนจบ จะได้ไฟล์ ```package.json```

* ```npm install express``` ใช้ที่ cmd จะได้ไฟล์ ```package-lock.json```

* ลง Library ชื่อ express สำเร็จจะมีตรงนี้เพิ่มมาที่ไฟล์ ```package.json```
```json
"dependencies": { 
    "express": "^5.1.0" 
  }
```

* อะไรที่รัน powershell ไม่ได้ ให้ลองรันที่ cmd

* ```cd ..``` ย้อน path กลับไป

* ```node index.js``` เพื่อ run หลังจากนั้น ```Ctrl + c ``` เพื่อ ออก

* ```npm install nodemon --server-dev``` โหลด nodemon มาไว้ที่ไฟล์ 

* ลง nodemon สำเร็จจะมีตรงนี้เพิ่มมาที่ไฟล์ ```package.json```
```json
"devDependencies": {
    "nodemon": "^3.1.10"
  }
```


# Rest API = ช่องทางการสื่อสาร
* เปิด postman มาลองยิงได้เลย

## 1. GET (Read มาแสดงหน้าบ้าน)
**example GET**
* res.send ออกเป็น text
* res.json ออกเป็น json
```js
// เรียกใช้ library express ด้วยคำสั่ง require
const express = require('express')

// ประกาศเริ่มต้นการใช้ express
const app = express()
const port = 8000

// สร้าง API path '/' และคืนคำ Hello world ออกมาผ่าน API
app.get('/', (req, res) => {
  res.send('Hello World!') //res.send -> ออกเป็น text

  /*let user = {
    firstname: 'ทดสอบ',
    lastname: 'นามสกุล',
    age: 24
  }
  res.json(user)*/ //ออกเป็น json
})

// ประกาศ​gxbf http server ที่ port 8000 (ตามตัวแปร port)
app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```
![alt text](./img/path.png)


## 2. POST (Create จากหน้าบ้าน)
* POST , PUT ใช้ส่งของเหมือนกันแต่ POST จะส่งผ่าน Body

### โดยใช้ body-parser เอาไว้แกะข้อมูล body
* ลง library ```npm install body-parser``` cmd
```json
"dependencies": {
    "body-parser": "^2.2.0", //สำเร็จ 
    "express": "^5.1.0"
  },
```

**example POST 1**
* ส่งข้อความ 

```js
const express = require('express')
const app = express()
const bodyParser = require('body-parser')

app.use(bodyParser.text()) //บอกว่าจะ parser เป็น text

app.post('/test', (req, res) => {
  // ดึง data ออกจาก body
  const textData = req.body
  res.send(textData)
})

app.listen(8000, () => {
  console.log('Server started on port 8000')
})
```
![alt text./img/](image.png)


**example POST 2**
### JSON
* ยิง json ใน postman ต้องใส่  ```" "``` ด้วย

* ส่ง json
```js
//import library
const express = require('express')
const app = express()

const bodyparser = require('body-parser')
app.use(bodyparser.json())

const port = 8000


// สำหรับเก็บ user
let users = []

//path = GET /users
app.get('/users', (req, res) => {
  res.json(users)
})

// path  = POST /user
app.post('/user', (req, res) =>{
  //รับ user ที่ส่งเข้ามาผ่าน body
  let user = req.body
  console.log('user', user)

  //เอาข้อมูลไปเก็บใน array
  users.push(user)
  res.json({
    message: 'add ok',
    user: user
  })

  //output terminal
  res.send(req.body)
})


//output terminal
app.listen(port, (req, res) => {
  console.log('http server run at ' + port)
})
```
**result at console**

![alt text](./img/image-2.png)

**result POST**

![alt text](./img/image-1.png)

**result GET**

![alt text](./img/image-3.png)


## 3. PUT (Update ทั้งหมด)
* PUT ส่งผ่าน (/: )param เป็นการดึงค่าจาก url 

**example PUT 1**
```js
//import library
const express = require('express')
const app = express()

const bodyparser = require('body-parser')
app.use(bodyparser.json())

const port = 8000


// สำหรับเก็บ user
let users = []
let counter = 1

//path = GET /users
app.get('/users', (req, res) => {
  res.json(users)
})

// path  = POST /user
app.post('/user', (req, res) =>{
  //รับ user ที่ส่งเข้ามาผ่าน body
  let user = req.body
  user.id = counter //เพิ่มให้ user มี id เป็น atb

  counter += 1 //นับ id 
  console.log('user', user)

  //เอาข้อมูลไปเก็บใน array
  users.push(user)
  res.json({
    message: 'add ok',
    user: user //show json ที่ส่งเข้ามา
  })

  //output terminal
  res.send(req.body)
})

// path = PUT /user/:id (:id = parameter)
app.put('/user/:id', (req, res) => {
  let id = req.params.id
  res.send(id)
})
//output terminal
app.listen(port, (req, res) => {
  console.log('http server run at ' + port)
})
```

**result** จะเห็นว่า id มันนับเอง(running number)

![alt text](./img/image-4.png)

**example PUT 2**
```js
const express = require('express')
const app = express()

const bodyparser = require('body-parser')
app.use(bodyparser.json())

const port = 8000

//path = GET /users
app.get('/users', (req, res) => {
  res.json(users)
})

// สำหรับเก็บ user
let users = []
let counter = 1 //เอาไว้นับ id เพื่อจิ้ม

// path  = POST /user
app.post('/user', (req, res) =>{
  let user = req.body

  // นับ id 
  user.id = counter 
  counter += 1
  
  //เอาข้อมูลไปเก็บใน array
  users.push(user)
  res.json({
    message: 'add ok',
    user: user
  })

  //output terminal
  res.send(req.body)
})

// path = PUT /user/:id (:id = parameter)
app.put('/user/:id', (req, res) => {
  let id = req.params.id
  let updateUser = req.body

  //หา user จาก id ที่ส่งมา
  let selectedIndex = users.findIndex(user => user.id == id)
  //res.send('index: ' + selectedIndex + '') //+ '' เพื่อให้ selectedIndex กลายเป็น string

  //เหลือบรรทัดเดียวได้แบบข้างบน
  /*let selectedIndex = users.findIndex(user => {
    if (user.id == id) {
      return true
    } else {
      return false
    }
  })*/

  //update ข้อมูล user (null || 'ค่าที่ update')
  users[selectedIndex].firstname = updateUser.firstname || users[selectedIndex].firstname 
  users[selectedIndex].lastname = updateUser.lastname || users[selectedIndex].lastname

  //ส่งข้อมูลที่ update เสร็จแล้วกลับไป
  res.json({
    message: 'update user complete!',
    data: {
      user: updateUser,
      indexUpdate: selectedIndex
    }
  }) 
})
```
![alt text](./img/image-5.png)
![alt text](./img/image-6.png)


## 4. PATCH (Update รายฟิลด์)
**example PATCH**
```js
//import library
const express = require('express')
const app = express()

const bodyparser = require('body-parser')
app.use(bodyparser.json())

const port = 8000


// สำหรับเก็บ user
let users = []
let counter = 1

//path = GET /users
app.get('/users', (req, res) => {
  res.json(users)
})

// path  = POST /user
app.post('/user', (req, res) =>{
  //รับ user ที่ส่งเข้ามาผ่าน body
  let user = req.body
  user.id = counter
  counter += 1
  console.log('user', user)

  //เอาข้อมูลไปเก็บใน array
  users.push(user)
  res.json({
    message: 'add ok',
    user: user //show json ที่ส่งเข้ามา
  })

  //output terminal
  res.send(req.body)
})

// path = PATCH /user/:id (:id = parameter)
app.patch('/user/:id', (req, res) => {
  let id = req.params.id //รับ id มาชี้ตัวที่จะ update
  let updateUser = req.body //อ่านค่าที่จะ update

  //หา user จาก id ที่ส่งมา
  let selectedIndex = users.findIndex(user => user.id == id)
  
  //update ข้อมูล user 
  //if มีข้อมูลupdate อันไหน ค่อยupdate อันนั้น 
  if (updateUser.firstname) {
    users[selectedIndex].firstname = updateUser.firstname
  }
  
  if (updateUser.lastname) {
    users[selectedIndex].lastname = updateUser.lastname 
  }
   
  //ส่งข้อมูลที่ update เสร็จแล้วกลับไป
  res.json({
    message: 'update user complete!',
    data: {
      user: updateUser,
      indexUpdate: selectedIndex
    }
  }) 

})

//output terminal
app.listen(port, (req, res) => {
  console.log('http server run at ' + port)
})
```
![alt text](./img/image-7.png)
![alt text](./img/image-8.png)

## 5. DELETE (ลบ ผ่าน id)
**example DELETE**
```js
//import library
const express = require('express')
const app = express()

const bodyparser = require('body-parser')
app.use(bodyparser.json())

const port = 8000


// สำหรับเก็บ user
let users = []
let counter = 1

//path = GET /users
app.get('/users', (req, res) => {
  res.json(users)
})

// path  = POST /user
app.post('/user', (req, res) =>{
  //รับ user ที่ส่งเข้ามา
  let user = req.body
  user.id = counter
  counter += 1
  

  //เอาข้อมูลไปเก็บใน array
  users.push(user)
  res.json({
    message: 'add ok',
    user: user
  })

  //output terminal
  res.send(req.body)
})

// path = PATCH /user/:id 
app.patch('/user/:id', (req, res) => {
  let id = req.params.id
  let updateUser = req.body

  //หา user จาก id ที่ส่งมา
  let selectedIndex = users.findIndex(user => user.id == id)

  //update ข้อมูล user (null || 'ค่าที่ update')
  if (updateUser.firstname) {
    users[selectedIndex].firstname = updateUser.firstname
  }
  
  if (users[selectedIndex].lastname) {
    users[selectedIndex].lastname = updateUser.lastname 
  }
  

  //ส่งข้อมูลที่ update เสร็จแล้วกลับไป
  res.json({
    message: 'update user complete!',
    data: {
      user: updateUser,
      indexUpdate: selectedIndex
    }
  }) 
})

// path = DELETE /user/:id
app.delete('/user/:id', (req, res) => {
  let id = req.params.id

  //หา user จาก id ที่ส่งมา
  let selectedIndex = users.findIndex(user => user.id == id)

  //ลบ
  //delete users[selectedIndex] //ลบแบบนี้จะหายไปเลย ไม่สวย ทำแบบล่างแทน

  //(indexที่จะลบ, จำนวนที่จะลบ)
  users.splice(selectedIndex, 1)

  res.json({
    message: 'delete complete!',
    indexDelete: selectedIndex
  })
})

//output terminal
app.listen(port, (req, res) => {
  console.log('http server run at ' + port)
})
```
**จะเห็นว่าลบ id1 เหลือแต่ id2**

![alt text](./img/image-9.png)
![alt text](./img/image-10.png)