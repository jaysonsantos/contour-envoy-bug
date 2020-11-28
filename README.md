To reproduce, install projectcontour with `kustomize build contour | kubectl apply -f`, but any already installed version should be able to reproduce it.
Then:

- load the nginx example `kubectl apply -f nginx-serving.yaml`
- forward the port locally `kubectl -n projectcontour port-forward svc/envoy 8000:80 &` or run inside the server
- download the file but it is important to make it slow, after around 30 seconds it should fail `curl --limit-rate 10k http://127.0.0.1:8000/bin-data -H "Host: nginx-serve-test" -v -o /dev/null`

```
curl --limit-rate 10k http://127.0.0.1:8000/bin-data -H "Host: nginx-serve-test" -v -o /dev/null
*   Trying 127.0.0.1:8000...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to 127.0.0.1 (127.0.0.1) port 8000 (#0)
> GET /bin-data HTTP/1.1
> Host: nginx-serve-test
> User-Agent: curl/7.68.0
> Accept: */*
>
  0     0    0     0    0     0      0      0 --:--:--  0:00:29 --:--:--     0* Recv failure: Connection reset by peer
  0     0    0     0    0     0      0      0 --:--:--  0:00:30 --:--:--     0
* Closing connection 0
curl: (56) Recv failure: Connection reset by peer
```
