---
title: .NET Core API笔记
tags:
    - 随笔
    - .NET Core
    - API
    - 服务端
---

### 自定义路由
    Startup.cs:
    ```C#
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            ...

            app.UseMvc(routes =>
            {
                routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
            });
        }
    ```