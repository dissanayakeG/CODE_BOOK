**select all checkboxes inside specific element with class**
```js
$('.content-wrapper input:checkbox')
```

**add event**
```js
$('.content-wrapper input:checkbox').click(function (event) {})
```

**select all checkboxes inside DOM**

```js
$("input[type='checkbox']")
````
**add event**
```js
$("input[type='checkbox']").click(function (event) {})
```

**access attribute of selected element**

```js
$("input[type='checkbox']").click(function (event) {
    $(this).attr("class")
})
```

**find all input elements inside a specific element's closest element**
```js
$("#" + parentId).closest('div.checkboxes-wrapper').find("input")
```


**GET ALL CHECKBOXES INSIDE A DIV**
```js
let selectedScopeElement= document.getElementsByClassName('checkboxes-wrapper '+ func_name)[0].querySelectorAll('input[type="checkbox"]');
```
