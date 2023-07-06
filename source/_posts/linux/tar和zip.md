### tar

```shell
# 解压
tar -xvf proxy-darwin-amd64.tar.gz -C goproxy
# 解压删除
tar -tf proxy-darwin-amd64.tar.gz | xargs rm -rf
```



### zip

```shell
# 解压删除
zipinfo -l path/xx.zip | xargs rm -rf 
```

