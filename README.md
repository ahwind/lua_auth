# hsas
静态页权限管理


### 说明

- 模块可以对静态页站点基于URL进行权限管理。
- 支持用户和组。


### 配置

#### nginx配置

```
lua_package_path '/usr/local/nginx/conf/include/?.lua;;';
lua_package_cpath '/usr/local/luajit/lib/?.so;;';


server
 {

   listen       192.168.1.50:80;


   server_name docs.example.com;

   expires -1;

   error_page 403 /403.html;

   location = /403.html {
      root /usr/local/nginx/html;
      #internal;
   }

   location ~* /_static|_images {
       root /www/build/html;	
   }


   location ~* / {
       root /www/build/html;
       default_type text/html;
       auth_basic "nginx basic auth";
       auth_basic_user_file include/httppasswd;
       autoindex on;
       access_by_lua_file 'conf/include/access.lua';
   }

 }
```

#### 用户配置

"*"表示完全权限， "work  business  base"表示根目录下名称， 如：http://docs.example.com/work/



```
{
    "zhangsan": {
        "auth":"*",
        "group": "admin"
    },
   
    "ops": {
        "auth":"work  business base",
        "group":"ops"
    }
}
```

#### 组配置


```
{
    "admin": "*",
    "ops": "duty work  _downloads",
    "dev": "duty dev"

}
```

