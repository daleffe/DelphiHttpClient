# DelphiHttpComponent
Delphi component wrapper for WinInet library. Written in Delphi 2010.

## How to use

### GET
```delphi
procedure TForm1.Button1Click(Sender: TObject);
begin
  if HttpRequest1.Get('https://httpbin.org/get') then
    ShowMessage(HttpRequest1.Response.ContentAsString)
  else
    ShowMessage('ERROR ' + IntToStr(HttpRequest1.Response.StatusCode));
end;
```
### POST
Posting a simple text/JSON:
```delphi
procedure TForm1.Button1Click(Sender: TObject);
begin
  if HttpRequest1.Post('https://httpbin.org/put', 'testing a POST') then
    ShowMessage(HttpRequest1.Response.ContentAsString)
  else
    ShowMessage('ERROR ' + IntToStr(HttpRequest1.Response.StatusCode));
end;
```
#### Posting a file with a Multi-Part form:
```delphi
procedure TForm1.Button1Click(Sender: TObject);
var
  Body: TMultipartFormBody;
begin
  Body := TMultipartFormBody.Create;
  Body.ReleaseAfterSend := True;
  Body.Add('code', '2');
  Body.AddFromFile('image', 'C:\Users\User\Desktop\image.png');

  HttpRequest1.Post('https://httpbin.org/post', Body);
  ShowMessage(HttpRequest1.Response.ContentAsString);
end;
```
#### Posting a URL-encoded form:
```delphi
procedure TForm1.Button1Click(Sender: TObject);
var
  Body: TUrlEncodedFormBody;
begin
  Body := TUrlEncodedFormBody.Create;
  Body.ReleaseAfterSend := True;
  Body.Add('code', '1');
  Body.Add('name', 'John');

  HttpRequest1.Post('https://httpbin.org/post', Body);
  ShowMessage(HttpRequest1.Response.ContentAsString);
end;
```

### Auth
 - Method 1
 ```delphi
  HttpRequest1.Username := 'admin';
  HttpRequest1.Password := '123';
 ```

 - Method 2
 ```delphi
 HttpRequest1.Get('http://admin:123@contoso.com');
 ShowMessage(HttpRequest1.Response.ContentAsString);
 ```
 
## Improvements
- Added the possibility to send data using GET method, similar to POST;
- Added _ContentType_ parameter to methods that can send raw text;
- Added _Basic Authentication_ (URL inline OR username and password).

## Fixes
- Solved bug that don't return _Content-Length_.