![Preview](https://raw.githubusercontent.com/tunguskha/Image-shadow/master/assets/imgs/Preview.jpg)
# Image shadow !

Simple and small Javascript script to display the shadow of the image!

---

### Download Shadow-image
```
$ git clone https://github.com/tunguskha/Image-shadow
```

---

### Import
Locally
```html
<script src="assets/js/image-shadow.js"></script>
```
CDN
```html
<script src="https://cdn.jsdelivr.net/gh/tunguskha/Image-shadow@latest/assets/js/image-shadow.js"></script>
```

---

### Use it
All you need is .ishadow wrapper to the image and blur value in data attribute
```html
<div class="ishadow">
  <img 
	data-blur="20" 
	src="assets/imgs/halt-and-catch-fire.jpg">
</div>
```

Also, you can make an element hoverable by adding `data-hover="true"`.
`false` by default
```html
<div class="ishadow">
  <img 
	data-blur="20" 
	data-hover="true" 
	src="assets/imgs/halt-and-catch-fire.jpg">
</div>
```
---

### Support
| IE | Edge| Firefox | Chrome | Safari | Opera |
|:-:|:--:|:-:|:-:|:-:|:-:|
|:x:| :x: |:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|:heavy_check_mark:|

---

### See it live
[Image-shadow](https://tunguskha.github.io/Image-shadow/)

