---
marp: true
theme: gaia
paginate: true
footer: 
style: "section footer{color:black;font-size: 20px;}"
---
<style scoped>
section h1 {text-align: center;font-size: 60px;color:black;}
section {
  background-image:url('./fm.png');
  background-size:cover
}
footer{color:black;font-size: 20px;} 
</style>
<!-- _class: lead gaia -->

# Traceable Secret Sharing: Strong Security and Efficient Constructions
111
Dan Boneh, Aditi Partap, and Lior Rotem
Stanford University

Crypto 2024


---
<style >

section{
  background-image:url('./bg.png');
  background-size:cover;
  position: absolute;
  }
section h1 {font-size:40px;color:black;margin-top:px;}
section h2 {font-size:30px;color:black;margin-top:px;}
section p {font-size: 25px;color:black;}
section table {text-align: center;font-size: 32px;color:black;}
section a {font-size: 25px;color:black;}
li {font-size: 30px;text-align: left;}

img {
    margin-left: auto; 
    margin-right:auto; 
    display:block;
    margin:0 auto;
    width:25cm;
    }
</style>

<style scoped>
section h1 {font-size:60px;color:black;margin-top:px;}
li {font-size: 50px;text-align: left;}
</style>

# 总览
1. 可追踪秘密分享应用场景
2. 符号表示与定义
3. 基于Shamir秘密分享
4. 基于 Blakley 秘密共享

---
# 使用场景

假设 Alice 使用 $t$-out-of-$n$ 的秘密共享方案将她的密钥存储在 $n$ 个服务器上。只要这 $n$ 个服务器中有 $t$ 个没有串通，她的密钥就会受到保护。然而，如果有少于 $t$ 个服务器决定出售它们持有的共享怎么办？在这种情况下，Alice 应该能够追责这些服务器，否则将无法阻止它们出售共享。

<style scoped>
section p {font-size: 35px;color:black;}
</style>

---

因此，我们需要一种秘密共享方案满足以下两个保证：
1. **可追踪性（Traceability）**  
	首先，如果 Alice 获得了泄露的信息，她应该能够将其追踪回泄露的服务器。此外，为了追究这些服务器的责任，她还应该能够提供一个证明，表明这些服务器确实泄露了她的秘密信息。
	
2. **非可归责性（Non-Imputability）**  
	第二个需求是**非可归责性**。这一性质要求 Alice 不应该能够伪造一份虚假的证明，以诬陷任何未参与泄露的诚实服务器。

<style scoped>
section p {font-size: 35px;color:black;}
</style>

---
# 首页修改
我预设了适合展示的标题大小,当然如果是需要修改适合投影的字体大小可在源代码中第 12 行修改`h1的font-size`.

改变`background-image:url('./fm.png');`可以修改首页背景图片.

```css
<style scoped>
section h1 {text-align: center;font-size: 80px;color:black;}
section {
  background-image:url('./fm.png');
  background-size:cover
}
footer{color:black;font-size: 20px;} 
</style>
```

---
# 背景修改
修改模板源代码 27~31 行`background-image:url('./bg.png');`中背景图片即可.
```css
section{
  background-image:url('./bg.png');
  background-size:cover;
  position: absolute;
  }
```

---
# 字体修改
全局格式修改在源代码 31~35 行
`h1`一级大标题
`h2`二级大标题
`p`正文字体
`table`表格字体,居中
`li`列表字体,居左
```css
section h1 {font-size:40px;color:black;margin-top:px;}
section h2 {font-size:30px;color:black;margin-top:px;}
section p {font-size: 25px;color:black;}
section table {text-align: center;font-size: 32px;color:black;}
section a {font-size: 25px;color:black;}
li {font-size: 30px;text-align: left;}
```

---
# 图片格式
默认居中,临时在某一slides中修改可以
```css
<style scoped>
img {
    margin-left: auto; margin-right:auto; 
    display:block;margin:0 auto;width:25cm;
    }
</style>
或者:
![w:2cm h:2cm](fm.png)
```
![w:2cm h:2cm](fm.png)

---
# 左右排布
```
![bg left:40% w:5cm h:5cm](fm.png)
```
![bg left:40% w:5cm h:5cm](fm.png)

---
# 左右排布
```
![bg right:40% w:5cm h:5cm](fm.png)
```
![bg right:40% w:5cm h:5cm](fm.png)

---
# CSS 说明
```
https://www.w3school.com.cn/css/css_image_gallery.asp

https://marpit.marp.app/theme-css
```

# 更多细节
```
https://marpit.marp.app/usage
```

# 更多Markdown 语法
```
https://www.markdown.xyz/basic-syntax/
```

---
<style scoped>
section h1 {text-align: center;font-size: 100px;color:black;}
section {
  background-image:url('./fm.png');
  background-size:cover
}
footer{color:black;font-size: 20px;} 
</style>
<!-- _class: lead gaia -->

# 谢谢朋友们