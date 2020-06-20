# Kibana Enhanced Table

This Kibana visualization plugin is like a Data Table, but with enhanced features like computed columns, filter bar and pivot table.

**NEW:** Now the plugin contains a second visualization named 'Document Table'. This visualization does the same thing than 'Enhanced Table' visualization, but for single documents (not aggregations). It especially allows to have enhanced features, compared to a saved search (like column custom labels, computed columns and filter bar)

## Features

- Add computed columns, based on other columns :
  - Support for [expr-eval](https://github.com/silentmatt/expr-eval#expression-syntax) expressions
  - Support for numeric columns (ex: `col0 + col1` or `col[0] + col[1]`)
  - Support for string columns, including HTML (ex: `col0 > 0 ? 'OK' : 'KO'`)
  - Support for date columns
  - Ability to reference total hits count returned by ES query (ex: `col0 / total * 100`)
  - Ability to reference a column by its label (ex: `col['Sum of duration'] / col['Count']`)
  - Ability to reference a column total (ex: `col['Sales by month'] / total['Sales by month']`)
  - Ability to define/reference arrays, do variable assignment and define custom functions in expressions
  - Ability to compute column total using formula
  - Support for numeric pretty format using [Numeral.js](http://numeraljs.com/#format) (ex: `0,0.00`)
  - Support for date pretty format using [Moment.js](http://momentjs.com/docs/#/displaying/format/) (ex: `YYYY-MM-DD`)
  - Support for column alignment (ex: `left`, `right`)
  - Support for template rendering using [Handlebars](https://handlebarsjs.com/guide/expressions.html) (ex: `<strong>{{value}}</strong>`)
  - Template can reference other columns (ex: `<span style="color: {{col0}}">{{value}}</span>`)
  - Template can reference another column by its label (ex: `<span style="color: {{col['color']}}">{{value}}</span>`)
  - Template can encode a value to render it as a URL parameter (ex: `<a href="my-dashboard?param={{{encodeURIComponent value}}}">{{value}}</a>`)
  - Support for computed column filtering (Filter for/out value) if formula simply references a column value (ex: `col0`)
  - More documentation [here](#computed-column-formula--lines-computed-filter-documentation)
- Filter table lines based on a computed formula (ex: `col0 > 0`)
  - More documentation [here](#computed-column-formula--lines-computed-filter-documentation)
- Hide some table columns (ex: `0,1,Col2 Label` hides columns 0, 1 and the column labeled 'Col2 Label')
  - Note that the column label must be written as is (including whitespaces), with no surrounding quotes.
  - Column labels containing commas are not supported since the comma is used to separate the columns
  - It is recommended to use column labels to hide columns rather than their indices. Using the 'Split cols' feature, in fact, indices might depend on the global timerange that has been set. 
- Add a filter bar (ex: when user enters `cat` filter, it will display only rows that contain "cat")  
  - Works also with numeric and date columns
  - Ability to enable case sensitive filter
  - Ability to make filter bar hideable
  - Ability to filter as you type
  - Ability to filter each term separately
  - Ability to highlight filter results
  - Ability to define filter bar width
- Support for a new bucket type : 'Split Cols'. It lets to create a pivot table
  - When combined with computed columns, each computed column can be added per split column or after all split columns  
- Hide export links (when checked, it will hide "Raw" and "Formatted" export links)
- Add a total label on total line first column (ex: `Total:`)
- Ability to display striped rows
- Kibana supported versions : all versions from 5.5 to 7.7

## Demo

![Demo](docs/demo.gif)


## Install

Every release package includes a Plugin version (X.Y.Z) and a Kibana version (A.B.C).

- Go to [releases](https://github.com/fbaligand/kibana-enhanced-table/releases "Go to releases!") and choose the right one for your Kibana
- launch a shell terminal and go to $KIBANA_HOME folder
- use Kibana CLI to install :
  - directly from Internet URL :
`./bin/kibana-plugin install https://github.com/fbaligand/kibana-enhanced-table/releases/download/vX.Y.Z/enhanced-table-X.Y.Z_A.B.C.zip`
  - locally after manual download :
`./bin/kibana-plugin install file:///path/to/enhanced-table-X.Y.Z_A.B.C.zip`
- restart Kibana


## Computed Column Formula / Lines Computed Filter documentation

Common features available for 'Computed Column Formula' and 'Lines Computed Filter':
- Support for [expr-eval](https://github.com/silentmatt/expr-eval#expression-syntax) expressions
- Support for features brought by expr-eval 2.0:
  - Ability to reference arrays. Especially useful to reference a 'Top Hits' metric column: `col1[0]`
  - New functions for arrays available: `join, map, filter`
  - Variable assignment: `x = 4`
  - Custom function definitions: `myfunction(x, y) = x * y`
  - Evaluate multiple expressions by separating them with `;`
- Ability to reference total hits count returned by ES query (ex: `col0 / total * 100`)
- Ability to reference a column by its label (ex: `col['Sum of duration'] / col['Count']`)
- Column reference validation (by number or label), with error notification
- Formula validation, with error notification
- Support for pre-defined functions provided by [expr-eval](https://github.com/silentmatt/expr-eval#pre-defined-functions)
- Additional custom functions listed in table below (ex: `col['Expiration Date'] > now() ? 'OK' : 'KO'`)

Function     | Description
:----------- | :----------
col(colRef, defaultValue)  | Returns column value referenced by `colRef` (if it exists), or else `defaultValue`. `colRef` is either the column label (ex: `'Count'`) or the column index (ex: `1`).
countSplitCols  | Returns the count of all split columns (only if 'Split cols' bucket is used).
[encodeURIComponent(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)  | Encodes the provided string as a Uniform Resource Identifier (URI) component.
[indexOf(strOrArray, searchValue\[, fromIndex\])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf)  | Returns the index within the calling String or Array object of the first occurrence of the specified value, starting the search at fromIndex. Returns -1 if the value is not found.
[lastIndexOf(strOrArray, searchValue\[, fromIndex\])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/lastIndexOf)  | Returns the index within the calling String or Array object of the last occurrence of the specified value, searching backwards from fromIndex. Returns -1 if the value is not found.
[now()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/now)  | Returns the number of milliseconds elapsed since January 1, 1970 00:00:00 UTC
[replace(str, substr, replacement)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)  | Returns a new string with first match of substr replaced by a replacement. Only the first occurrence will be replaced.
[replaceRegexp(str, regexp, replacement)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)  | Returns a new string with all matches of a regexp replaced by a replacement. All the occurrences will be replaced.
[search(str, regexp)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/search)   | Executes a search for a match between a regular expression on 'str' String. Returns the index of the first match or -1 if not found.
[sort(array\[, compareFunction\])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)   | Sorts the elements of an array in place and returns the sorted array. A compare function can be provided to customize the sort order. Example for an array of numbers: `comparator(a, b) = a - b; sort(col0, comparator)`
[substring(str, indexStart\[, indexEnd\])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring)   | Returns the part of the string between the start and end indexes, or to the end of the string (if no index end is provided).
sumSplitCols   | Returns the sum of all split column values (only if 'Split cols' bucket is used).
[toLowerCase(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)   | Returns the calling string value converted to lowercase.
[toUpperCase(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)   | Returns the calling string value converted to uppercase.
[trim(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trim)   | Removes whitespace from both ends of a string.
[uniq(array)](https://lodash.com/docs/3.10.1#uniq)   | Removes duplicates from provided array so that array contains only unique values.


## Change Log

Versions and Release Notes are listed in [Releases](https://github.com/fbaligand/kibana-enhanced-table/releases) page


## Credits

This Kibana plugin is inspired from [computed-columns](https://github.com/seadiaz/computed-columns) and [kbn_searchtables](https://github.com/dlumbrer/kbn_searchtables) plugins.  
Thanks for their great work !


## Development

To run enhanced-table plugin in development mode (that enables hot code reload), follow these instructions:
- execute these commands :
``` bash
git clone https://github.com/elastic/kibana.git
cd kibana
git reset --hard vX.Y.Z # replace 'X.Y.Z' by desired Kibana version
mkdir plugins
cd plugins
git clone https://github.com/fbaligand/kibana-enhanced-table.git enhanced-table
```
- install the version of Node.js listed in the `kibana/.node-version` file
- ensure that `node` binary is both in `PATH` environment variable and in `kibana/node` folder
- install the latest version of [yarn](https://yarnpkg.com)
- execute these commands :
``` bash
cd kibana
yarn kbn bootstrap
cd plugins/kibana-enhanced-table
yarn install
yarn start
```
- in your browser, call `https://localhost:5601` and enjoy!


To build a distributable archive, execute this command :
``` bash
yarn build --kibana-version X.Y.Z # replace 'X.Y.Z' by desired Kibana version
```

## Donation

If this plugin helps you and you want to support it, you can give me a cup of coffee :)

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=fbaligand%40gmail.com&currency_code=EUR&source=url)
