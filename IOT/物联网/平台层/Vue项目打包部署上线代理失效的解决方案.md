
添加反向代理
![|500](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/https/cdn.jsdelivr.net/gh/xuezhaorong/Picgo/Source/fix-dir/picgo/picgo-clipboard-images/2025/02/22/2025/02/22/14-02-31-2e5094c562c6efaaf10a91adee64bf95-14-02-03-2e5094c562c6efaaf10a91adee64bf95-20250222140202-02a408-8ec5ef.png)

```
location ^~/api/ {
     proxy_pass http://xxx.xxx.xxx.xxx:8889/api/;
}
```