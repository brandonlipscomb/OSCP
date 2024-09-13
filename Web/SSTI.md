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
