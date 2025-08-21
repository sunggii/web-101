# MySQL จาก nodejs 
(ใช้ไฟล์เดิมต่อจาก ep_9) รันไฟล์ server/index.js
link lec พี่ไมค์ [ep.10](https://docs.mikelopster.dev/c/web101/chapter-10/intro)

## HTTP response status codes
* Informational responses (100 – 199)
* Successful responses (200 – 299)
* Redirection messages (300 – 399)
* Client error responses (400 – 499)
* Server error responses (500 – 599)

[ref](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)


## note
* ```npm install mysql``` library ที่ node จะคุยกับ MySQL 

## how to run index.js(in ep_10)
1. coppy code จาก ```ep_10/workshop.md``` มาวงที่ ```server/index.js```
2. check path ที่ cmd ว่าใช่ ```D:\mike\mike-web\ep_10_11\server``` ?
3. ใช้ ```npx nodemon index.js``` เพื่อรัน server
4. เปิด MAMP จะเด้งไปที่ Browser ให้ใส่ ```http://localhost:8888/phpMyAdmin5/```
5. เปิด postman ลองใช้ GET ```http://localhost:8000/users```

## Connect API <-> DB(MySQL) แบบที่ 1
* promise
* async await

**ทวนการเปิด MySQL**

1. เปิด ```MAMP``` เช็คดูว่า root ✅

![alt text](/img/mamp.png)

2. ให้เปิด ```http://localhost:8888/phpMyAdmin5/``` เพื่อดูข้อมูลใน SQL เพื่อความชัว
3. ไปลองยิง GET ที่ postman


### ต่อแบบ promise ใช้กับอะไรที่ต้องรอ
```js
//import library
const express = require('express') 
const bodyparser = require('body-parser')
const mysql = require('mysql2/promise') 

const app = express()
app.use(bodyparser.json())

const port = 8000 //port server

/*-------------- promise ---------------*/
app.get('/testdb', (req, res) => {
  mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'tutorials', //ใส่ชื่อ data base
    port: 8889
  }).then((conn) => {
    // สิ่งนี้เราเรียกกันว่า promise
    conn //connection object ที่ได้จากการเชื่อมต่อ MySQL
    .query('SELECT * FROM user') //ใส่ชื่อ from
    .then((results) => {
      res.json(results[0]) //เป็นการแสดงข้อมูลใน MySQL ของ library mysql2
    })
    .catch((error) => {
      console.error('Error fetching users:', error.message)
      res.status(500).json({ error: 'Error fetching users' }) //500 คือ status code
    })
  })
})
/*-----------------------------------------*/

//output terminal
app.listen(port, (req, res) => {
  console.log('http server run at ' + port)
})
```
**result**

![alt text](/img/promise.png)

### ต่อแบบ async await เหมือน promise แค่โค้ดจะสวยกว่า
```js
//import library
const express = require('express') 
const bodyparser = require('body-parser')
const mysql = require('mysql2/promise') 

const app = express()
app.use(bodyparser.json())

const port = 8000 //port server

/*-------------- async await ---------------*/
app.get('/testdb', async (req, res) => {
  try {
    const conn = await mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'tutorials', //ใส่ชื่อ data base
    port: 8889 //port data base
  })
  const results = await conn.query('SELECT * FROM user') 
  res.json(results[0])
  } catch (error) {
    console.error('Error fetching users:', error.message)
    res.status(500).json({ error: 'Error fetching users' }) 
  }
})
/*-----------------------------------------*/

//output terminal
app.listen(port, (req, res) => {
  console.log('http server run at ' + port)
})
```
**same result**


## Connect API <-> DB(MySQL) แบบที่ 2
* เปลี่ยน connnection เป็น function เพื่อที่ทุก method จะได้เรียกใช้ conn ได้

```js
//import library
const express = require('express') //จัดการ API
const bodyparser = require('body-parser')
const mysql = require('mysql2/promise') 

const app = express()
app.use(bodyparser.json())

const port = 8000 //port server

/*------------- connect MySQL --------------*/
let conn = null //ตัวแปรเอาไว้คุยกับ MySQL
const initMySQL = async () => {
  conn = await mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'tutorials', //ใส่ชื่อ data base
    port: 8889
  })
}
/*------------------------------------------*/

/*-------------- async await ---------------*/
//path = '/testdb' ดึงข้อมูลทั้งหมดใน table มาโชว์
app.get('/testdb', async (req, res) => {
  try {
  const results = await conn.query('SELECT * FROM user') //เรียกใช้ conn
  res.json(results[0])
  } catch (error) {
    console.error('Error fetching users:', error.message)
    res.status(500).json({ error: 'Error fetching users' }) 
  }
})
/*-----------------------------------------*/

//output terminal
app.listen(port, async (req, res) => {
  await initMySQL() //รอ connect MySQL (initMySQL()) ให้เสร็จ
  console.log('http server run at ' + port)
})
```
**same result**


## รวมคำสั่ง query ที่เจอใน woekshop.md
ระวังอย่าสะกดผิด ที่อยู่ใน ```(' ')``` คือคำสั่ง
1. GET   
    const results = await conn.query('SELECT * FROM'```<ชื่อ Table>```)

    **ตัวอย่าง** 
    ```const results = await conn.query('SELECT * FROM user')```

    ----------------------------

2. POST  
    const results  = await conn.query('INSERT INTO ```<ชื่อ Table>``` SET ?', ```<ตัวแปรที่ดึงข้อมูลมาเก็บ จะถูกแทนที่ในตำแหน่ง ?>```)

    **ตัวอย่าง** 
    ```const results  = await conn.query('INSERT INTO user SET ?', userData)```   

    ![alt text](/img/post.png)

    ----------------------------

3. PUT 
    const results  = await conn.query('UPDATE ```<ชื่อ Table>``` SET ? WHERE id = ?', [updateUser , id])

    **ตัวอย่าง** 
    ```const results  = await conn.query('INSERT INTO user SET ?', userData)```

    ----------------------------

4. DELETE  ไอเดียเดียวกับ PUT

    **ตัวอย่าง**
    ```const results  = await conn.query('DELETE FROM user WHERE id = ?', id)```


## ลองต่อกับหน้าบ้าน
1. ```npm install cors ``` library

2. ไปเพิ่ม library
```js
//import library
const express = require('express') 
const bodyparser = require('body-parser')
const mysql = require('mysql2/promise')
const cors = require('cors') //เพิ่ม


const app = express()
app.use(bodyparser.json())
app.use(cors()) //เพิ่ม

const port = 8000

/*
✅GET /users สำหรับ get users ทั้งหมดที่บันทึกเข้าไปออกมา
✅POST /users สำหรับการสร้าง users ใหม่บันทึกเข้าไป
✅GET /users/:id สำหรับการดึง users รายคนออกมา
✅PUT /users/:id สำหรับการแก้ไข users รายคน (ตาม id ที่บันทึกเข้าไป)
✅DELETE /users/:id สำหรับการลบ users รายคน (ตาม id ที่บันทึกเข้าไป)
*/

let conn = null

const initMySQL = async () => {
  conn = await mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'tutorials', //ใส่ชื่อ data base
    port: 8889
  })
}


//path = GET /users สำหรับ get users ทั้งหมดที่บันทึกเข้าไปออกมา
app.get('/users', async (req, res) => {
  const results = await conn.query('SELECT * FROM user') //ชื่อต้องตรงกับ DB
    res.json(results[0])
})


// path  = POST /users สำหรับการสร้าง users ใหม่บันทึกเข้าไป
app.post('/users', async (req, res) => {
  try {
    const userData = req.body                         
    const results  = await conn.query('INSERT INTO user SET ?', userData) //ใส่ ? เพื่อจะได้ใส่ userData

    res.json({ 
      message: 'insert ok', 
      data: results[0] 
    })

  } catch (error) {
    console.error('Error message', error.message)
    res.status(500).json({
      message: 'somthing wrong'
    })
  }
})


//path = GET /users/:id สำหรับการดึง users รายคนออกมา
app.get('/users/:id', async (req, res) => {
  try {
    let id = req.params.id
    const results = await conn.query('SELECT * FROM user WHERE id = ?' , id) //ชื่อต้องตรงกับ DB
    console.log('result: ', results)

    //if check ว่ามีไหม เพราะถ้าไม่มีมันจะเป็นค่าว่าง หรือ == 0
    if (results[0].length == 0 ){
      throw {statusCode: 404, message: 'not found'}
    }
    res.json(results[0][0]) //จะได้ออกมาเป็น obj
    //res.json(results[0]) //จะได้ออกมาเป็น array

  } catch (error) {
    console.error('Error message', error.message)
    let statusCode = error.statusCode || 500
    res.status(statusCode).json({
      message: 'somthing wrong' ,
      errorMassage: error.message
    })
  }
})


//path = PUT /users/:id สำหรับการแก้ไข users รายคน (ตาม id ที่บันทึกเข้าไป)
app.put('/users/:id', async (req, res) => {
  try {
    let id = req.params.id
    let updateUser = req.body
    const results  = await conn.query(
      'UPDATE user SET ? WHERE id = ?', 
      [updateUser , id]
    )

    res.json({ 
      message: 'update ok', 
      data: results[0] 
    })

  } catch (error) {
    console.error('Error message', error.message)
    res.status(500).json({
      message: 'somthing wrong'
    })
  }
})


// path = DELETE /users/:id สำหรับการลบ users รายคน (ตาม id ที่บันทึกเข้าไป)
app.delete('/users/:id', async (req, res) => {
 try {
    let id = req.params.id
    const results  = await conn.query('DELETE FROM user WHERE id = ?', id)

    res.json({ 
      message: 'delete ok', 
      data: results[0] 
    })

  } catch (error) {
    console.error('Error message', error.message)
    res.status(500).json({
      message: 'somthing wrong'
    })
  }
})


//output terminal
app.listen(port, async (req, res) => {
  await initMySQL()
  console.log('http server run at ' + port)
})
```

3. สร้างไฟล์ connect.html code script จาก [doc พี่ไมค์](https://docs.mikelopster.dev/c/web101/chapter-10/bonus)
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Connect API</title>
</head>

<body>

    <script>
        fetch('http://localhost:8000/users') // /users --> path API
            .then((response) => {
                return response.json()
            })
            .then((responseData) => {
                console.log('responseData', responseData)
            })
    </script>
</body>

</html>
```

**ถ้าต่อถูก จะได้**
![alt text](/img/connet.png)