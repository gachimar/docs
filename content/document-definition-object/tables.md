+++
title = "Tables"
description = ""
weight = 23
alwaysopen = true
+++


Conceptually tables are similar to columns. They can however have headers, borders and cells spanning over multiple columns/rows.

```js
var docDefinition = {
  content: [
    {
      layout: 'lightHorizontalLines', // optional
      table: {
        // headers are automatically repeated if the table spans over multiple pages
        // you can declare how many rows should be treated as headers
        headerRows: 1,
        widths: [ '*', 'auto', 100, '*' ],

        body: [
          [ 'First', 'Second', 'Third', 'The last one' ],
          [ 'Value 1', 'Value 2', 'Value 3', 'Value 4' ],
          [ { text: 'Bold value', bold: true }, 'Val 2', 'Val 3', 'Val 4' ]
        ]
      }
    }
  ]
};
```

##### Table-cell properties

* `fillColor: string`: the background color of a table cell
* `fillOpacity: string`: the background opacity of a table cell


##### Table layouts

Can be used `layout` property for set table layout.

Available table layouts:

* `noBorders`
* `headerLineOnly`
* `lightHorizontalLines`

##### Own table layouts

Own table layouts must be defined before calling `pdfMake.createPdf(docDefinition)`.
```js
// define table layouts
var tableLayouts = {
  exampleLayout: {
    hLineWidth: function (i, node) {
      if (i === 0 || i === node.table.body.length) {
        return 0;
      }
      return (i === node.table.headerRows) ? 2 : 1;
    },
    vLineWidth: function (i) {
      return 0;
    },
    hLineColor: function (i) {
      return i === 1 ? 'black' : '#aaa';
    },
    paddingLeft: function (i) {
      return i === 0 ? 0 : 8;
    },
    paddingRight: function (i, node) {
      return (i === node.table.widths.length - 1) ? 0 : 8;
    }
  }
};

// add table layouts
pdfMake.addTableLayouts(tableLayouts);

// download the PDF
pdfMake.createPdf(docDefinition).download();
```


- On the server side:

```js
var pdfmake = require('pdfmake');
pdfmake.addFonts(fonts);

// Declaring your layout
var myTableLayouts = {
    exampleLayout: {
        /*
        Your layout here.
        */
    }
};

// Building the PDF
var pdf = pdfmake.createPdf(docDefinition);

// Writing it to disk
pdf.write('document.pdf').then(() => {
	// success event
}, err => {
  // error event
	console.error(err);
});
```

All concepts related to tables are covered by TABLES example in [playground](http://pdfmake.org/playground.html).
