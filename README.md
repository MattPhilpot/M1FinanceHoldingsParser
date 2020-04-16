# M1FinanceHoldingsParser

## Description
Simple javascript to parse holdings on M1Finance into a CSV. 

All this does is grab the first table element on the page, read through the rows, and output a big string - but obviously use at your own risk. My purpose in sharing this is just for individual use within a single account and expressly not to support any type of automated scraping.

### Usage:

1. Go to desktop dashboard https://dashboard.m1finance.com/d/invest/holdings
2. Open Chrome developer tools > console
3. Paste into console and run
4. Copy and paste output CSV to Google Sheets
5. Click paste icon and select Split text to columns

### Javascript:
```javascript
function getFirstMatchFrom(container, elementClassName) {
    var currentNode,
    ni = container.createNodeIterator(container.documentElement, NodeFilter.SHOW_ELEMENT);

    while(currentNode = ni.nextNode()) {
        var name = currentNode.className;
        if (name && typeof name == "string" && name.indexOf(elementClassName) > -1) {
	    return currentNode;
	}
    }
}

function holdingsToCSV() {
    let table = getFirstMatchFrom(document, 'table');
    let rows = [...table.childNodes];
    rows.shift(); //remove first element, just headers

    let getSymbol = c => {
        return c.innerText
            .replace(/[\W|^\\n]/, '')
            .trim()
            .split("\n")[0];
    }

    let getRowData = row => {
        let cells = [...row.childNodes];
        cells.pop(); //remove last element, it's just a chevron
        let isData = cells[0].tagName.toUpperCase() == "TD";

        return cells.map((c, i) => {
            if(isData && i == 0) {
                return getSymbol(c);
            }
            return c.innerText.replace(",","");
        }).join(", ");
    };

    let data = rows
        .map(getRowData)
        .map(function(x) {
            //change newlines to commas
            var retVal = x.replace(/\n/g, ',');
            //remove the ★ from first element
            //it's janky af, but it'll do pig
            return retVal.substring(2);
        });

    return data.join("\n");
}

console.log(holdingsToCSV());
```
### Credit
Credit for the original script goes to someone else, not me. But I have since updated it for the latest M1Finance layout.
