```bash
ssh user@server -o "ProxyCommand nc -X 5 -x 127.0.0.1:7890 %h %p"
```