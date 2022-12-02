# x-spreadsheet

[![npm package](https://img.shields.io/npm/v/x-data-spreadsheet.svg)](https://www.npmjs.org/package/x-data-spreadsheet)
[![NPM downloads](http://img.shields.io/npm/dm/x-data-spreadsheet.svg)](https://npmjs.org/package/x-data-spreadsheet)
[![NPM downloads](http://img.shields.io/npm/dt/x-data-spreadsheet.svg)](https://npmjs.org/package/x-data-spreadsheet)
[![Build passing](https://travis-ci.org/myliang/x-spreadsheet.svg?branch=master)](https://travis-ci.org/myliang/x-spreadsheet)
[![codecov](https://codecov.io/gh/myliang/x-spreadsheet/branch/master/graph/badge.svg)](https://codecov.io/gh/myliang/x-spreadsheet)
![GitHub](https://img.shields.io/github/license/myliang/x-spreadsheet.svg)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/myliang/x-spreadsheet.svg)
[![Join the chat at https://gitter.im/x-datav/spreadsheet](https://badges.gitter.im/x-datav/spreadsheet.svg)](https://gitter.im/x-datav/spreadsheet?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

> A web-based JavaScript spreadsheet

<p align="center">
  <a href="https://github.com/myliang/x-spreadsheet">
    <img width="100%" src="https://raw.githubusercontent.com/myliang/x-spreadsheet/master/docs/demo.png">
  </a>
</p>

## 分支后更新日志   
118代表基于myliang/x-spreadsheet 1.1.8版本的分支代码    
* 118.0.2   
1. 样式x-spreadsheet中background修改为transparent（透明色），如果需要自定义底色可以在外面包一层div里设置背景色。   
2. 初始化参数row里增加属性eoctdmIndexHeigth:int，控制表头的高度，≤0表示隐藏。   
3. 初始化参数增加属性eoctdmSelectedByRead:string，当mode为read，eoctdmSelectedByRead为off时，鼠标点击单元格后不显示选中框。   
4. 解决当表头和内容的高度不一样时，右边竖滚动条的顶部位置与内容顶部没对齐的问题。   
5. 初始化参数增加属性eoctdmGridStrokeStyle:string，在showGrid为true时设置网格样式，比如颜色值。   
6. 解决表格父节点字体设置为白色时，单元格输入框的字体色与底色相同表现出看不到内容的问题。    
7. 初始化参数增加属性eoctdmFormulas:array，用于加载用户自定义函数代码，示例代码如下：   
```javascript
eoctdmFormulas:[{
  key: 'TEST01', //仅支持全大写字母和数字
  title: '自定义函数',
  render: ary => JSON.stringify(ary)
}]
```   
8. 优化formula公式规则：1）入参无直接显示0，入参1个直接显示入参内容，入参≥2个时才交付formula的render执行；2）入参引入单元格（如a10）时如果值为数字会自动转换，不为数字原样返回，需要在render中类型判断处理。   
以上demo详见index.html中示例内容。   

## Document
* en
* [zh-cn 中文](https://hondrytravis.github.io/x-spreadsheet-doc/)

## CDN
```html
<link rel="stylesheet" href="https://unpkg.com/x-data-spreadsheet@1.1.5/dist/xspreadsheet.css">
<script src="https://unpkg.com/x-data-spreadsheet@1.1.5/dist/xspreadsheet.js"></script>

<script>
   x_spreadsheet('#xspreadsheet');
</script>
```

## NPM

```shell
npm install x-data-spreadsheet
```

```html
<div id="x-spreadsheet-demo"></div>
```

```javascript
import Spreadsheet from "x-data-spreadsheet";
// If you need to override the default options, you can set the override
// const options = {};
// new Spreadsheet('#x-spreadsheet-demo', options);
const s = new Spreadsheet("#x-spreadsheet-demo")
  .loadData({}) // load data
  .change(data => {
    // save data to db
  });

// data validation
s.validate()
```

```javascript
// default options
{
  mode: 'edit', // edit | read
  showToolbar: true,
  showGrid: true,
  showContextmenu: true,
  view: {
    height: () => document.documentElement.clientHeight,
    width: () => document.documentElement.clientWidth,
  },
  row: {
    len: 100,
    height: 25,
  },
  col: {
    len: 26,
    width: 100,
    indexWidth: 60,
    minWidth: 60,
  },
  style: {
    bgcolor: '#ffffff',
    align: 'left',
    valign: 'middle',
    textwrap: false,
    strike: false,
    underline: false,
    color: '#0a0a0a',
    font: {
      name: 'Helvetica',
      size: 10,
      bold: false,
      italic: false,
    },
  },
}
```

## import | export xlsx

https://github.com/SheetJS/sheetjs/tree/master/demos/xspreadsheet#saving-data

thanks https://github.com/SheetJS/sheetjs

## Bind events
```javascript
const s = new Spreadsheet("#x-spreadsheet-demo")
// event of click on cell
s.on('cell-selected', (cell, ri, ci) => {});
s.on('cells-selected', (cell, { sri, sci, eri, eci }) => {});
// edited on cell
s.on('cell-edited', (text, ri, ci) => {});
```

## update cell-text
```javascript
const s = new Spreadsheet("#x-spreadsheet-demo")
// cellText(ri, ci, text, sheetIndex = 0)
s.cellText(5, 5, 'xxxx').cellText(6, 5, 'yyy').reRender();
```

## get cell and cell-style
```javascript
const s = new Spreadsheet("#x-spreadsheet-demo")
// cell(ri, ci, sheetIndex = 0)
s.cell(ri, ci);
// cellStyle(ri, ci, sheetIndex = 0)
s.cellStyle(ri, ci);
```

## Internationalization
```javascript
// npm 
import Spreadsheet from 'x-data-spreadsheet';
import zhCN from 'x-data-spreadsheet/dist/locale/zh-cn';

Spreadsheet.locale('zh-cn', zhCN);
new Spreadsheet(document.getElementById('xss-demo'));
```
```html
<!-- Import via CDN -->
<link rel="stylesheet" href="https://unpkg.com/x-data-spreadsheet@1.1.5/dist/xspreadsheet.css">
<script src="https://unpkg.com/x-data-spreadsheet@1.1.5/dist/xspreadsheet.js"></script>
<script src="https://unpkg.com/x-data-spreadsheet@1.1.5/dist/locale/zh-cn.js"></script>

<script>
  x_spreadsheet.locale('zh-cn');
</script>
```

## Features
  - Undo & Redo
  - Paint format
  - Clear format
  - Format
  - Font
  - Font size
  - Font bold
  - Font italic
  - Underline
  - Strike
  - Text color
  - Fill color
  - Borders
  - Merge cells
  - Align
  - Text wrapping
  - Freeze cell
  - Functions
  - Resize row-height, col-width
  - Copy, Cut, Paste
  - Autofill
  - Insert row, column
  - Delete row, column
  - hide row, column
  - multiple sheets
  - print
  - Data validations

## Development

```sheel
git clone https://github.com/myliang/x-spreadsheet.git
cd x-spreadsheet
npm install
npm run dev
```

Open your browser and visit http://127.0.0.1:8080.

## Browser Support

Modern browsers(chrome, firefox, Safari).

## LICENSE

MIT
