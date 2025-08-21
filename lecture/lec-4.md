# What I learned

## Responsive
เอาไว้เป็นแนวทาง 
* [material.io](https://m3.material.io/foundations/layout/applying-layout/window-size-classes)
* [tailwindcss](https://tailwindcss.com/docs/responsive-design)

    * 640px  mobile

    * 768px  tablet

    * มากกว่า 768px destop(1024)  desingหลัก

## breakpoint (@media screen)
* ```(max-width: 768px)``` แปลว่าหน้าจอที่ใหญืที่สุดที่ยังทำ cssนั้น (background-color: blue;) อยู่

```html
<div class="container">
    breakpoint
</div>
```

```css
/*ต้องเว้นวรรคให้เป๊ะด้วย*/
 @media screen and (max-width: 768px){
    .container{
        background-color: blue;
    }
}
```

## imgge srcset
```html
<picture>
    <source 
        media="(max-width: 768px)" 
        src="https://fastly.picsum.photos/id/73/200/200.jpg?hmac=IYjgRq-Ok9gn3_MVxJ4TlfhLPONQ97qWvp2Ir1Y1z6c"
    >

    <!--รูปหลัก-->
    <img 
        src="https://fastly.picsum.photos/id/107/200/200.jpg?hmac=bWV6B7Av2dY7XMiQYnj-0VMJT_fmwttT1Fumzc4Ct7g"
    >
</picture>
```

## layout (flex , grid)
* flex 
    * 768++ ให้เรียงแนวนอน 
    * 768 ให้เรียงแนวตั้ง (max-width: 768px)
```html
<div class="container">
        <div class="item"> Item1 </div>
        <div class="item"> Item2 </div>
</div>
```

```css
 .container{
    display: flex;
}

.container .item{
    padding: 10px;
    flex: 1;
}

 @media screen and (max-width: 768px){
    .container{
        flex-direction: column;
    }
}
```

**result**

![alt text](./img/flex-1.png)

![alt text](./img/flex-2.png)

----------

* grid
    * 768++ ให้เรียงแนวนอน 
    * 768 ให้เรียงแนวตั้ง (max-width: 768px)
```html
<div class="grid-container">
        <div class="item"> Item1 </div>
        <div class="item"> Item2 </div>
        <div class="item"> Item3 </div>
        <div class="item"> Item4 </div>
        <div class="item"> Item5 </div>
        <div class="item"> Item6 </div>
    </div>
```

```css
        .grid-container{
            display: grid;
            /*แบบนี้จะมี 3 col ถ้า 1fr 1fr ก็จะมี 2 col*/
            grid-template-columns: 1fr 1fr 1fr; 
        }

        .grid-container .item{
            padding: 10px;
            flex: 1;
            background-color: aqua;
        }

        /*tablet style*/
        @media screen and (max-width: 768px){
            .grid-container{
                grid-template-columns: 1fr 1fr; 
            }
        }

        /*mobile style*/
        @media screen and (max-width: 640px){
            .grid-container{
                grid-template-columns: 1fr; 
            }
        }
```
**result**

![alt text](./img/grid-1.png)

![alt text](./img/grid-2.png)

**result_mobile**

![alt text](./img/grid-3.png)

------------


## CSS selector มี 5 แบบ (1-2 ใช้บ่อย)

### 1. แม่-ลูก (เว้นวรรค)
```html
<div class="container">
        Container
        <div class="item">
            Item
        </div>
    </div>
```

```css
.container .item{ /*เข้าถึงตัวลูก*/
            background-color: aqua;
        }
```
### 2. base class(ไม่เว้นวรรค)
```html
    <button class="button">button</button>
    <button class="button active">button</button>
```

```css
.button{ /*ิbase class*/
            border: 0;
            background-color: gray;
            padding: 10px;
        }
        .button.active{
            background-color: blue;
        }
```

### 3.แม่-ลูก-หลาน ต้องการแต่งแค่ลูก (>)
```html
    <div class="main-container">
        Container
        <div class="item">
            Item
            <div class="item">
                Item 1
            </div>
            <div class="item">
                Item 2
            </div>
            <div class="item">
                Item 3
            </div>
        </div>
    </div>
```

```css
.main-container > .item{
            background-color: bisque;
            padding: 10px;
        }
```

### 4. เพื่อนกัน (adjacent)
```html
    <!--<div class="item2">Item 2</div>-->
    <div class="item1">Item 1</div>
    <div class="item2">Item 2</div>
```

```css
.item1 + .item2{
            /*เป็นเงื่อไขว่า .item2 ต้องติดอยู่หลัง item1 เท่านั้นถึงจะทำงาน*/
            background-color: aquamarine;
        }
```

### 5. attribte selrctor แต่งตัวที่ att เหมือนกัน
```html
    <input  type="text" name="firstname">
    <input  type="text" name="lastname">
    <input  type="text" name="description">
```

```css
/*ชื่อtag[att ไม่ต้องใส่ ""]*/
input[type=text]{
            border: 1px gray solid;
            height: 100px;
        }
```

## CSS pseudo selector มี 2 แบบ
### 1. pseudo class / pseudo selector (:)
* pseudo class 

    **ตัวที่ใช้บ่อย** เช่น ```:hover```, ```:active``` = กด, ```:focus``` = cursor กดอยู่

    ```html
    <input type="text">
    <button class="button">button</button>
    ```

    ```css
    .button{
            background-color: gray;
            border: 0;
            padding: 20px;
        }

        .button:hover{/*กด*/
            background-color: blue;
        }

        .button:active{/*กด*/
            background-color: orange;
        }

        .button:focus{/*cursor กดอยู่*/
            background-color: red;
        }

        input:focus{
             background-color: orange;
        }
    ```


* pseudo selector

    เราสามารถใช้ class เดียวกันเพื่อเป็นอันหนึ่งอันเดียวกัน แต่ก็สามารถแต่งแยกกันได้
```html
<div class="container">
    <div class="item">1</div> <!--:first-child-->
    <div class="item">2</div> <!--:nth-child(2)-->
    <div class="item">3</div>
    <div class="item">4</div>
    <div class="item">5</div> <!--:last-child-->
</div>
```

```css
.container .item:first-child{
            background-color: blue;
        }
        .container .item:nth-child(2){
            background-color: brown;
        }
        .container .item:last-child{
            background-color: orange;
        }
```

**result**

![alt text](./img/pseudo_selector.png)


### 2. pseudo element (::)

pseudo element คือมันจะถูกมองว่ามีอะไรมาครอบทำให้แต่ง style ได้

![alt text](./img/pseudo_element%20.png)

**ตัวที่ใช้บ่อย** เช่น ```::before``` , ```::after``` , ```::first-letter```

```html
<div class="mike smile">TestMike</div>
```

```css
        .mike{
            color: blue;
        }

        .mike::first-letter{
            font-size: 40px;
        }

        .mike::before{
            content: '?';
        }
        
        .smile::after {
            content: '😆';
        }
```